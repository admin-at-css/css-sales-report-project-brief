# Synchronization Strategy
## CSS Sales Report - Offline-First Sync Logic

**Tujuan:** Panduan implementasi sync yang komprehensif untuk production deployment
**Terakhir Diperbarui:** Oktober 2025
**Versi:** 3.0 (Production Ready)

---

## 📋 Table of Contents

1. [Overview](#overview)
2. [Sync Triggers](#sync-triggers)
3. [Sync Order & Dependencies](#sync-order--dependencies)
4. [Transaction Management](#transaction-management)
5. [Conflict Resolution](#conflict-resolution)
6. [Retry Logic](#retry-logic)
7. [Photo Upload Strategy](#photo-upload-strategy)
8. [Error Handling](#error-handling)
9. [Edge Case Handling](#edge-case-handling)
10. [Testing & Validation](#testing--validation)

---

## 1. Overview

### Sync Philosophy

**Prinsip Offline-First:**
- ✅ Semua operations bekerja offline (writes langsung ke Drift)
- ✅ Sync bersifat background/asynchronous (tidak pernah memblokir UI)
- ✅ Local data adalah source of truth sampai di-sync
- ✅ Server bersifat authoritative setelah sync selesai
- ✅ Failures dapat di-retry (tidak ada permanent data loss)

### Sync Flow Diagram

```
┌──────────────────────────────────────────────────────────────┐
│ 1. USER ACTION (offline atau online)                         │
│    └─> Create/Edit/Delete → Write ke Drift segera            │
└────────────────────────────┬─────────────────────────────────┘
                             │
                             ↓
┌──────────────────────────────────────────────────────────────┐
│ 2. CONNECTIVITY CHECK                                         │
│    └─> Online? → Trigger sync                                │
│    └─> Offline? → Queue untuk nanti                          │
└────────────────────────────┬─────────────────────────────────┘
                             │
                             ↓
┌──────────────────────────────────────────────────────────────┐
│ 3. SYNC ENGINE (Background Service)                          │
│    ├─> Buat sync transaction log                             │
│    ├─> Upload dalam dependency order                         │
│    ├─> Handle conflicts (Last-Write-Wins untuk MVP)          │
│    └─> Update local is_synced flags                          │
└────────────────────────────┬─────────────────────────────────┘
                             │
                             ↓
┌──────────────────────────────────────────────────────────────┐
│ 4. SUPABASE (Cloud Database)                                 │
│    ├─> Validate dengan RLS policies                          │
│    ├─> Write ke PostgreSQL                                   │
│    ├─> Upload photos ke Storage                              │
│    └─> Return success/error                                  │
└────────────────────────────┬─────────────────────────────────┘
                             │
                             ↓
┌──────────────────────────────────────────────────────────────┐
│ 5. COMPLETION                                                 │
│    ├─> Success: Mark is_synced = true, tampilkan toast       │
│    ├─> Failure: Log error, schedule retry                    │
│    └─> Partial: Mark synced items, retry failed items        │
└──────────────────────────────────────────────────────────────┘
```

---

## 2. Sync Triggers

### Kapan Sync Terjadi?

| Trigger | Deskripsi | Priority | User Visible? |
|---------|-----------|----------|---------------|
| **Auto on Connection** | App mendeteksi network tersedia | High | 🔔 Toast: "Syncing..." |
| **Manual Button** | User tap tombol "Sync Now" | High | 🔔 Loading indicator |
| **After Create/Edit** | User save report/project | Medium | 🔔 Badge: "Pending sync" |
| **Periodic Background** | Setiap 15 menit jika online | Low | ❌ Silent |
| **App Resume** | User kembali ke app dari background | Medium | ❌ Silent kecuali ada pending |

### Implementasi: Connectivity Detection

```dart
import 'package:connectivity_plus/connectivity_plus.dart';

class SyncTriggerService {
  final Connectivity _connectivity = Connectivity();
  StreamSubscription<ConnectivityResult>? _subscription;

  void initializeAutoSync() {
    // Listen ke connectivity changes
    _subscription = _connectivity.onConnectivityChanged.listen((result) {
      if (result != ConnectivityResult.none) {
        // Network tersedia → trigger sync
        _triggerSync(reason: 'auto_connection');
      }
    });
  }

  Future<void> _triggerSync({required String reason}) async {
    if (await _hasPendingSyncs()) {
      print('[Sync] Triggered by: $reason');
      await SyncEngine.instance.syncAll();
    }
  }

  Future<bool> _hasPendingSyncs() async {
    final db = AppDatabase.instance;
    final counts = await db.countPendingSyncs();
    return counts.values.any((count) => count > 0);
  }

  void dispose() {
    _subscription?.cancel();
  }
}
```

### Tombol Manual Sync (UI)

```dart
// Di reports list screen atau profile screen
ElevatedButton.icon(
  onPressed: _isSyncing ? null : () async {
    setState(() => _isSyncing = true);

    try {
      final result = await SyncEngine.instance.syncAll();

      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('✅ Synced ${result.successCount} items')),
      );
    } catch (e) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('❌ Sync failed: ${e.toString()}')),
      );
    } finally {
      setState(() => _isSyncing = false);
    }
  },
  icon: _isSyncing
      ? CircularProgressIndicator(strokeWidth: 2)
      : Icon(Icons.sync),
  label: Text(_isSyncing ? 'Syncing...' : 'Sync Now'),
)
```

---

## 3. Sync Order & Dependencies

### Critical: Referential Integrity

**Sync HARUS terjadi dalam urutan exact ini untuk menghindari foreign key violations:**

```
1. Companies (tanpa dependencies)
   ↓
2. Contacts (depends on companies)
   ↓
3. Projects (depends on companies + contacts)
   ↓
4. Reports (depends on projects)
   ↓
5. Report Attendees (depends on reports + contacts)
   ↓
6. Project Value Log (depends on projects)
   ↓
7. Attachments (depends on reports)
```

### Implementasi: Ordered Sync

```dart
class SyncEngine {
  final AppDatabase _db;
  final SupabaseClient _supabase;

  Future<SyncResult> syncAll() async {
    final txId = Uuid().v4();
    final result = SyncResult();

    try {
      // Buat batch transaction log
      await _createSyncTransaction(txId, 'batch', 'all');

      // CRITICAL: Sync dalam dependency order
      result.add(await _syncCompanies());
      result.add(await _syncContacts());
      result.add(await _syncProjects());
      result.add(await _syncReports());
      result.add(await _syncReportAttendees());
      result.add(await _syncProjectValueLog());
      result.add(await _syncAttachments());

      // Tandai batch sebagai success
      await _completeSyncTransaction(txId, 'success');

      return result;

    } catch (e, stackTrace) {
      // Tandai batch sebagai failed
      await _completeSyncTransaction(txId, 'failed', e.toString());

      // Kirim ke error tracking (Sentry)
      Sentry.captureException(e, stackTrace: stackTrace);

      rethrow;
    }
  }

  Future<EntitySyncResult> _syncCompanies() async {
    final pending = await _db.getPendingSyncCompanies();
    final result = EntitySyncResult(entityType: 'companies');

    for (final company in pending) {
      final txId = Uuid().v4();

      try {
        await _createSyncTransaction(txId, 'company', company.id);

        // Upload ke Supabase
        await _supabase.from('companies').upsert(company.toJson());

        // Tandai sebagai synced secara lokal
        await _db.update(_db.localCompanies)
          ..where((tbl) => tbl.id.equals(company.id))
          ..write(LocalCompaniesCompanion(
            isSynced: Value(true),
            syncedAt: Value(DateTime.now()),
          ));

        await _completeSyncTransaction(txId, 'success');
        result.successCount++;

      } catch (e) {
        await _completeSyncTransaction(txId, 'failed', e.toString());
        result.failedCount++;
        result.errors.add('Company ${company.companyName}: ${e.toString()}');
      }
    }

    return result;
  }

  // Method serupa untuk _syncContacts(), _syncProjects(), dll.
}
```

### Mengapa Order Penting (Edge Case)

**Scenario:** Sync reports sebelum projects

```
1. User membuat offline:
   - Project A (UUID: proj-123)
   - Report B (UUID: rpt-456, project_id: proj-123)

2. Sync dimulai, urutan salah:
   - ❌ Coba sync Report B terlebih dahulu
   - Server: "project_id proj-123 tidak ada" (foreign key violation)
   - Report B sync gagal

3. Urutan yang benar:
   - ✅ Sync Project A terlebih dahulu (sukses)
   - ✅ Kemudian sync Report B (sukses, project sudah ada)
```

---

## 4. Transaction Management

### Sync Transaction Log

Setiap sync operation mendapat transaction record untuk rollback capability:

```dart
Future<void> _createSyncTransaction(
  String txId,
  String entityType,
  String entityId,
) async {
  await _db.into(_db.syncTransactions).insert(
    SyncTransactionsCompanion.insert(
      id: txId,
      entityType: entityType,
      entityId: entityId,
      operation: 'sync', // atau 'create', 'update', 'delete'
      status: 'syncing',
      createdAt: DateTime.now(),
    ),
  );
}

Future<void> _completeSyncTransaction(
  String txId,
  String status, // 'success' atau 'failed'
  [String? errorMessage],
) async {
  await _db.update(_db.syncTransactions)
    ..where((tbl) => tbl.id.equals(txId))
    ..write(SyncTransactionsCompanion(
      status: Value(status),
      errorMessage: Value(errorMessage),
      completedAt: Value(DateTime.now()),
    ));
}
```

### Resume Setelah App Crash

Jika app crash mid-sync, resume saat restart:

```dart
Future<void> resumeIncompleteSyncs() async {
  final incomplete = await _db.select(_db.syncTransactions)
    ..where((tbl) => tbl.status.isIn(['pending', 'syncing']))
    ..orderBy([(tbl) => OrderingTerm.asc(tbl.createdAt)]);

  final transactions = await incomplete.get();

  for (final tx in transactions) {
    print('[Sync] Resuming transaction: ${tx.entityType} ${tx.entityId}');

    try {
      // Retry sync operation
      await _retrySyncTransaction(tx);
    } catch (e) {
      print('[Sync] Retry gagal: ${e.toString()}');
    }
  }
}
```

---

## 5. Conflict Resolution

### Conflict Detection

**Conflict terjadi ketika:** Local dan server keduanya memiliki changes untuk entity yang sama.

**Metode detection:** Bandingkan `local_updated_at` vs server `updated_at`

```dart
Future<void> _syncWithConflictDetection(LocalReport report) async {
  // Fetch server version
  final serverReport = await _supabase
      .from('reports')
      .select()
      .eq('id', report.id)
      .maybeSingle();

  if (serverReport == null) {
    // Belum ada di server → simple upload
    await _supabase.from('reports').insert(report.toJson());
    return;
  }

  // Server version ada → cek timestamps
  final serverUpdatedAt = DateTime.parse(serverReport['updated_at']);
  final localUpdatedAt = report.localUpdatedAt;

  if (serverUpdatedAt.isAfter(localUpdatedAt)) {
    // SERVER MENANG → Overwrite local
    await _overwriteLocalWithServer(report.id, serverReport);
    print('[Conflict] Server version lebih baru, local overwritten');

  } else if (localUpdatedAt.isAfter(serverUpdatedAt)) {
    // LOCAL MENANG → Upload ke server
    await _supabase.from('reports').update(report.toJson()).eq('id', report.id);
    print('[Conflict] Local version lebih baru, uploaded ke server');

  } else {
    // SAME TIMESTAMP → Sudah sync
    print('[Conflict] Same timestamp, sudah sync');
  }
}
```

### Strategi Conflict Resolution

#### MVP Strategy: Last-Write-Wins

**Implementasi:**
- Bandingkan timestamps
- Timestamp yang lebih baru menang
- Yang kalah di-overwrite (data loss mungkin terjadi)

**Dapat diterima untuk MVP karena:**
- Tim kecil (2 sales reps)
- Probabilitas rendah untuk same-entity edits
- User feedback akan menginformasikan Phase 2 improvements

**Code:**
```dart
// Sederhana: timestamp yang lebih baru menang
if (serverUpdatedAt > localUpdatedAt) {
  localData = serverData; // Server menang
} else {
  serverData = localData; // Local menang
}
```

#### Phase 2 Strategy: User-Prompted Resolution

**Implementasi:**
- Detect conflict
- Tampilkan UI dengan kedua versions
- User memilih mana yang di-keep (atau merge)

**Contoh UI:**
```dart
if (conflictDetected) {
  final choice = await showDialog<ConflictChoice>(
    context: context,
    builder: (context) => ConflictResolutionDialog(
      localVersion: localReport,
      serverVersion: serverReport,
    ),
  );

  if (choice == ConflictChoice.keepLocal) {
    await _supabase.from('reports').update(localReport.toJson());
  } else if (choice == ConflictChoice.keepServer) {
    await _db.update(_db.localReports).replace(serverReport);
  } else if (choice == ConflictChoice.merge) {
    final merged = await _mergeReports(localReport, serverReport);
    await _supabase.from('reports').update(merged.toJson());
  }
}
```

---

## 6. Retry Logic

### Strategi Exponential Backoff

**MVP Requirement:** Failed syncs harus retry secara otomatis (acceptable risk table).

```dart
class RetryStrategy {
  static const maxAttempts = 3;
  static const baseDelay = Duration(seconds: 2);

  static Future<T> withRetry<T>({
    required Future<T> Function() operation,
    required String operationName,
  }) async {
    int attempt = 0;
    Duration delay = baseDelay;

    while (attempt < maxAttempts) {
      attempt++;

      try {
        print('[Retry] $operationName - Attempt $attempt/$maxAttempts');
        return await operation();

      } catch (e) {
        if (attempt >= maxAttempts) {
          print('[Retry] $operationName - Max attempts tercapai, giving up');
          rethrow;
        }

        print('[Retry] $operationName gagal, retrying dalam ${delay.inSeconds}s...');
        await Future.delayed(delay);

        // Exponential backoff: 2s → 4s → 8s
        delay *= 2;
      }
    }

    throw Exception('Retry gagal setelah $maxAttempts attempts');
  }
}

// Penggunaan:
await RetryStrategy.withRetry(
  operation: () => _supabase.from('reports').insert(report.toJson()),
  operationName: 'Upload report ${report.id}',
);
```

### Retry Queue Management

```dart
class SyncQueue {
  static final queue = <String>{}; // Set dari entity IDs pending sync

  static Future<void> processQueue() async {
    while (queue.isNotEmpty) {
      final entityId = queue.first;

      try {
        await _syncEntity(entityId);
        queue.remove(entityId); // Success → hapus dari queue

      } catch (e) {
        // Failure → tetap di queue, akan retry di next sync trigger
        print('[Queue] Gagal sync $entityId, akan retry nanti');
        break; // Stop processing queue untuk sekarang
      }
    }
  }
}
```

---

## 7. Photo Upload Strategy

### Per-Photo Sync Status (Production Requirement)

**Problem:** All-or-nothing photo sync menyebabkan data loss (Edge Cases line 19-42)

**Solution:** Lacak setiap photo secara individual

```dart
enum PhotoSyncStatus {
  pending,   // Belum di-upload
  uploading, // Sedang uploading
  success,   // Berhasil di-upload
  failed,    // Upload gagal (cek error)
}

Future<void> uploadPendingPhotos(String reportId) async {
  final pendingPhotos = await _db.select(_db.localAttachments)
    ..where((tbl) => tbl.reportId.equals(reportId))
    ..where((tbl) => tbl.syncStatus.isIn(['pending', 'failed']))
    ..where((tbl) => tbl.uploadAttempts.isSmallerThanValue(3)); // Max 3 retries

  final photos = await pendingPhotos.get();

  // Upload photos secara parallel (max 3 concurrent)
  await Future.wait(
    photos.map((photo) => _uploadSinglePhoto(photo)),
    eagerError: false, // Lanjutkan meskipun beberapa gagal
  );
}

Future<void> _uploadSinglePhoto(LocalAttachment photo) async {
  final txId = Uuid().v4();

  try {
    // Tandai sebagai uploading
    await _db.update(_db.localAttachments)
      ..where((tbl) => tbl.id.equals(photo.id))
      ..write(LocalAttachmentsCompanion(
        syncStatus: Value('uploading'),
        lastAttemptAt: Value(DateTime.now()),
      ));

    // Upload ke Supabase Storage
    final remotePath = '${userId}/${year}/${month}/${reportId}/${photo.id}.jpg';
    await _supabase.storage.from('report-photos').upload(
      remotePath,
      File(photo.localFilePath),
    );

    // Tandai sebagai success
    await _db.update(_db.localAttachments)
      ..where((tbl) => tbl.id.equals(photo.id))
      ..write(LocalAttachmentsCompanion(
        syncStatus: Value('success'),
        remoteFilePath: Value(remotePath),
        syncedAt: Value(DateTime.now()),
      ));

    print('[Photo] Upload sukses: ${photo.id}');

  } catch (e) {
    // Tandai sebagai failed, increment attempts
    await _db.update(_db.localAttachments)
      ..where((tbl) => tbl.id.equals(photo.id))
      ..write(LocalAttachmentsCompanion(
        syncStatus: Value('failed'),
        uploadAttempts: Value(photo.uploadAttempts + 1),
        uploadError: Value(e.toString()),
      ));

    print('[Photo] Upload gagal: ${photo.id} - ${e.toString()}');
  }
}
```

### Photo Upload Resilience

**Idempotent Uploads:** Cek jika photo sudah ada sebelum re-uploading

```dart
Future<bool> _photoExistsOnServer(String remotePath) async {
  try {
    final metadata = await _supabase.storage
        .from('report-photos')
        .info(remotePath);
    return metadata != null;
  } catch (e) {
    return false; // Assume tidak ada jika error
  }
}

// Dalam _uploadSinglePhoto:
if (photo.remoteFilePath != null &&
    await _photoExistsOnServer(photo.remoteFilePath!)) {
  // Sudah di-upload → hanya tandai sebagai success
  await _markPhotoAsSynced(photo.id);
  return;
}
```

---

## 8. Error Handling

### Kategori Error

| Error Type | Penyebab | Action | User Message |
|------------|----------|--------|--------------|
| **Network** | Tidak ada internet, timeout | Retry dengan backoff | "Network error. Akan retry otomatis." |
| **Auth** | Token expired | Refresh token, retry | "Session expired. Re-authenticating..." |
| **Validation** | Invalid data (missing field) | Jangan retry, log error | "Data validation gagal. Hubungi support." |
| **Conflict** | Concurrent edits | Resolve conflict | "Data berubah di server. Resolving..." |
| **Storage** | Quota exceeded | Stop uploads, warn user | "Storage limit tercapai. Silakan upgrade." |
| **Foreign Key** | Dependency belum di-sync | Sync dependency dulu, retry | "Syncing related data terlebih dahulu..." |

### Error Tracking Integration

```dart
Future<void> _syncWithErrorTracking(LocalReport report) async {
  try {
    await _uploadReport(report);

  } on SocketException catch (e) {
    // Network error → silent retry (jangan report ke Sentry)
    print('[Sync] Network error: ${e.toString()}');
    _scheduleRetry(report.id);

  } on PostgrestException catch (e) {
    // Supabase error → report ke Sentry
    await Sentry.captureException(e, stackTrace: StackTrace.current);
    _showErrorToUser('Sync gagal: ${e.message}');

  } catch (e, stackTrace) {
    // Unknown error → pasti report
    await Sentry.captureException(e, stackTrace: stackTrace);
    _showErrorToUser('Unexpected sync error. Silakan coba lagi.');
  }
}
```

---

## 9. Edge Case Handling

### Edge Case 1: Partial Sync Failure

**Scenario:** (Edge Cases doc line 19-42)
- Report sync ✅
- Photos 1-4 sync ✅
- Photo 5 gagal ❌
- Photos 6-10 tidak dicoba ❌

**Solution:**
```dart
// Lanjutkan uploading meskipun satu gagal
await Future.wait(
  photos.map((photo) => _uploadSinglePhoto(photo)),
  eagerError: false, // Jangan stop saat first failure
);

// Cek results
final failed = await _getFailedPhotos(reportId);
if (failed.isNotEmpty) {
  _showPartialSyncWarning(
    'Report synced, tetapi ${failed.length} photos gagal. '
    'Akan retry otomatis.'
  );
}
```

### Edge Case 2: App Crash Mid-Sync

**Scenario:** (Edge Cases doc line 75-91)
- Syncing 20 reports
- Reports 1-10 synced ✅
- App crash
- User restart app
- Reports mana yang telah di-sync?

**Solution:**
```dart
// Saat app startup, cek incomplete syncs
Future<void> onAppStartup() async {
  final incomplete = await _db.getIncompleteSyncTransactions();

  if (incomplete.isNotEmpty) {
    print('[Startup] Ditemukan ${incomplete.length} incomplete syncs, resuming...');
    await _resumeIncompleteSyncs(incomplete);
  }
}

// Resume incomplete syncs
Future<void> _resumeIncompleteSyncs(List<SyncTransaction> txs) async {
  for (final tx in txs) {
    if (tx.status == 'syncing') {
      // Terinterupsi → reset ke pending
      await _db.update(_db.syncTransactions)
        ..where((tbl) => tbl.id.equals(tx.id))
        ..write(SyncTransactionsCompanion(status: Value('pending')));
    }

    // Retry operation
    await _retrySyncTransaction(tx);
  }
}
```

### Edge Case 3: Token Expiry Mid-Sync

**Scenario:** (Edge Cases doc line 363-388)
- User offline selama 7 hari
- Auth token expired
- Mulai sync → 401 Unauthorized
- Semua syncs gagal

**Solution:**
```dart
Future<void> _syncWithTokenRefresh(LocalReport report) async {
  try {
    await _uploadReport(report);

  } on AuthException catch (e) {
    if (e.statusCode == 401) {
      print('[Auth] Token expired, refreshing...');

      // Refresh token
      final session = await _supabase.auth.refreshSession();

      if (session != null) {
        // Retry dengan new token
        await _uploadReport(report);
      } else {
        // Refresh gagal → logout user
        _showAuthErrorAndLogout('Session expired. Silakan login kembali.');
      }
    }
  }
}
```

---

## 10. Testing & Validation

### Critical Test Scenarios

#### Test 1: Offline → Online Transition
```dart
test('Offline ke online sync mempertahankan data integrity', () async {
  // 1. Go offline
  await NetworkSimulator.goOffline();

  // 2. Buat data
  final company = await createCompany('Test Co');
  final project = await createProject(companyId: company.id);
  final report = await createReport(projectId: project.id);

  // 3. Verify local storage
  expect(await db.getReport(report.id), isNotNull);
  expect(report.isSynced, isFalse);

  // 4. Go online
  await NetworkSimulator.goOnline();

  // 5. Sync
  await SyncEngine.instance.syncAll();

  // 6. Verify server
  final serverReport = await supabase.from('reports').select().eq('id', report.id).single();
  expect(serverReport, isNotNull);

  // 7. Verify local ditandai sebagai synced
  final localReport = await db.getReport(report.id);
  expect(localReport.isSynced, isTrue);
});
```

#### Test 2: Partial Sync Recovery
```dart
test('Partial sync failure recover dengan baik', () async {
  // 1. Buat report dengan 10 photos
  final report = await createReportWithPhotos(photoCount: 10);

  // 2. Simulasikan network failure di photo #5
  NetworkSimulator.failOnNthRequest(5);

  // 3. Attempt sync
  await SyncEngine.instance.syncAll();

  // 4. Verify: Report synced, photos 1-4 synced, photo 5 gagal, photos 6-10 pending
  final syncedPhotos = await db.getSyncedPhotos(report.id);
  expect(syncedPhotos.length, 4);

  final failedPhotos = await db.getFailedPhotos(report.id);
  expect(failedPhotos.length, 1);
  expect(failedPhotos.first.uploadAttempts, 1);

  // 5. Retry
  NetworkSimulator.reset();
  await SyncEngine.instance.syncAll();

  // 6. Verify: Semua 10 photos synced
  final allSynced = await db.getSyncedPhotos(report.id);
  expect(allSynced.length, 10);
});
```

#### Test 3: Conflict Resolution
```dart
test('Last-Write-Wins resolve conflicts dengan benar', () async {
  // 1. Buat report di device A
  final reportA = await createReport(projectId: 'proj-123');
  await SyncEngine.instance.syncAll();

  // 2. Edit di device B (simulasikan via direct Supabase call)
  await supabase.from('reports')
      .update({'notes': 'Edited on device B'})
      .eq('id', reportA.id);

  // 3. Edit di device A (local)
  reportA.notes = 'Edited on device A';
  await db.updateReport(reportA);

  // 4. Sync device A
  await SyncEngine.instance.syncAll();

  // 5. Verify: Device A menang (timestamp lebih baru)
  final serverReport = await supabase.from('reports').select().eq('id', reportA.id).single();
  expect(serverReport['notes'], 'Edited on device A');
});
```

---

## 📊 Performance Targets

| Metric | Target | Pengukuran |
|--------|--------|------------|
| Sync latency (10 reports) | <5 detik | Ukur start → completion |
| Photo upload (10 photos @ 500KB each) | <30 detik di 3G | Gunakan network throttling |
| Conflict detection | <100ms per entity | Profile _syncWithConflictDetection() |
| Database query (pending syncs) | <50ms | Profile countPendingSyncs() |
| Memory usage selama sync | <100MB increase | Gunakan Flutter DevTools |

---

## 🚨 Production Checklist

Sebelum deploy sync logic:

- [ ] Semua sync operations menggunakan transaction log
- [ ] Retry logic dengan exponential backoff diimplementasi
- [ ] Per-photo sync status tracking bekerja
- [ ] Token refresh saat 401 errors diimplementasi
- [ ] Conflict resolution ditest dengan multiple devices
- [ ] Error tracking (Sentry) terintegrasi
- [ ] Network simulator tests passing
- [ ] Sync progress visible ke user (badge, toast, loading)
- [ ] Partial sync recovery ditest
- [ ] Foreign key order strictly enforced
- [ ] Idempotent uploads verified (tidak ada duplicates)

---

## 📞 Support & Debugging

**Issue Umum:**

1. **"Sync stuck di X%"**
   - Cek: table `sync_transactions` untuk `status = 'syncing'`
   - Fix: Reset stuck transactions ke `pending` dan retry

2. **"Photos tidak pernah upload"**
   - Cek: `local_attachments.upload_attempts` > 3
   - Fix: Review field `upload_error`, reset attempts ke 0

3. **"Duplicate companies dibuat"**
   - Cek: Idempotent logic tidak bekerja
   - Fix: Verify UUID generation di client side

**Query Debug:**
```sql
-- Cari stuck syncs
SELECT * FROM sync_transactions WHERE status = 'syncing' AND created_at < datetime('now', '-10 minutes');

-- Cari failed photo uploads
SELECT * FROM local_attachments WHERE sync_status = 'failed' AND upload_attempts < 3;

-- Count pending syncs berdasarkan type
SELECT
  CASE
    WHEN is_synced = 0 AND is_draft = 0 THEN 'pending'
    WHEN is_synced = 1 THEN 'synced'
    WHEN is_draft = 1 THEN 'draft'
  END as status,
  COUNT(*) as count
FROM local_reports
GROUP BY status;
```

---

**Terakhir Diperbarui:** Oktober 2025 (v3.0 - Production Ready)
**Next Review:** Setelah MVP pilot deployment

---

## Referensi

- Edge Cases Document (line 19-42: Partial sync failures)
- Edge Cases Document (line 75-91: App crash recovery)
- Edge Cases Document (line 363-388: Token expiry)
- Drift_Schema.md (SyncTransactions table)
- Technical_Spec.md (Non-functional requirements)
