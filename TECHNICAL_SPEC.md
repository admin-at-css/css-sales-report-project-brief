# Technical Specification
## CSS Sales Report - Non-Functional Requirements & Edge Cases

**Versi:** 3.0 (Production Ready)
**Terakhir Diperbarui:** Oktober 2025
**Status:** Siap untuk Implementasi

---

## 1. Overview

Dokumen ini menjelaskan technical requirements, constraints, dan handling edge cases untuk aplikasi CSS Sales Report. Dokumen ini melengkapi Functional_Spec.md dengan mendetailkan BAGAIMANA sistem harus berperilaku di berbagai kondisi.

**Lingkup:** Non-functional requirements, performance targets, edge case handling, data integrity constraints

**Sumber Material:** Dokumen ini mengkonsolidasikan dan memprioritaskan edge cases yang didokumentasikan di `FOR_EXTERNAL_DEVELOPERS/EDGE_CASES_AND_TECHNICAL_CONSIDERATIONS.md`

---

## 2. Non-Functional Requirements

### 2.1 Performance Requirements

| Metric | Target | Priority | Metode Pengukuran |
|--------|--------|----------|-------------------|
| Waktu pembuatan report | <5 detik | P0 | User timing analytics |
| Waktu load list view | <500ms | P0 | Performance profiling |
| Sync latency (10 reports) | <5 detik | P0 | Network monitoring |
| Response query search | <300ms | P1 | Database query profiling |
| Waktu kompresi photo | <2 detik per photo | P1 | Background task timing |
| Query database (indexed) | <200ms | P0 | Drift query profiling |

**Rationale:** User menyatakan "pembuatan report <5 menit" dapat diterima. Ini adalah target teknis untuk memastikan UX yang baik.

---

### 2.2 Availability & Reliability

| Requirement | Target | Priority | Catatan |
|-------------|--------|----------|---------|
| **Offline availability** | 100% | P0 | App harus bekerja tanpa internet (core requirement) |
| **Sync success rate** | >95% | P0 | Diukur selama 30 hari |
| **Data loss rate** | 0% | P0 | **TIDAK DAPAT DITERIMA** per PRD risk table |
| **App crash rate** | <5% sessions | P0 | Diukur via Sentry |
| **Uptime (Supabase)** | >99.9% | N/A | Third-party SLA |

**Critical:** Zero tolerance untuk data loss. Crashes dapat diterima HANYA jika draft auto-save mencegah data loss.

---

### 2.3 Scalability Requirements

| Requirement | MVP Target | Phase 2 Target | Phase 3 Target |
|-------------|-----------|----------------|----------------|
| Concurrent users | 2-5 | 10 | 50 |
| Reports per user | 500 | 2.000 | 10.000 |
| Total database size | <1GB | <10GB | <100GB |
| Photo storage | <5GB | <50GB | <500GB |
| API requests/min | <100 | <500 | <2.000 |

**Pertimbangan Arsitektur:**
- Pagination wajib (20 items/page) untuk handle large datasets
- Database indexes di kolom yang sering di-query
- Photo compression untuk membatasi pertumbuhan storage
- Supabase plan upgrade path telah didefinisikan

---

### 2.4 Security Requirements

| Requirement | Implementation | Priority | Metode Verifikasi |
|-------------|----------------|----------|-------------------|
| Authentication | Supabase Auth (JWT) | P0 | Auth flow testing |
| Authorization | RLS policies di semua tables | P0 | Multi-user RLS testing |
| Data encryption in transit | HTTPS (TLS 1.3) | P0 | SSL cert validation |
| Data encryption at rest | Supabase default encryption | P1 | Platform documentation |
| Session management | Token expiry (7 hari), auto-refresh | P0 | Token expiry testing |
| Password strength | Min 8 chars, complexity | P1 | Input validation testing |

**Model Security:**
- Sales reps HANYA dapat mengakses data mereka sendiri
- Managers memiliki akses READ-ONLY ke semua data tim
- Tidak ada user yang dapat memodifikasi data user lain
- RLS ditegakkan di level database (tidak dapat di-bypass)

**Untuk RLS policies yang detail:** Lihat `database/Supabase_Schema.md`

---

### 2.5 Usability Requirements

| Requirement | Target | Priority | Catatan |
|-------------|--------|----------|---------|
| Material Design 3 compliance | 100% | P0 | Konsistensi design system |
| One-handed mobile usage | Kontrol thumb-reachable | P1 | Bottom navigation, FAB placement |
| Maximum taps to action | ≤3 taps | P1 | Create report, view report, sync |
| Kejelasan error message | Tidak ada jargon teknis | P0 | Error messages yang user-friendly |
| Visibilitas loading state | Semua async operations | P0 | Loading indicators, progress bars |
| Accessibility (WCAG 2.1) | Level AA compliance | P2 | Screen reader support, color contrast |

---

### 2.6 Maintainability Requirements

| Requirement | Standard | Priority |
|-------------|----------|----------|
| Dokumentasi code | Inline comments untuk logic yang kompleks | P1 |
| Dokumentasi API | OpenAPI/Swagger (Phase 2) | P2 |
| Dokumentasi database schema | Comprehensive (selesai) | P0 |
| Git commit messages | Format Conventional Commits | P1 |
| Code review | Required untuk semua merges ke `main` | P0 |
| Test coverage | 60%+ untuk business logic | P0 |

---

## 3. Critical Edge Cases (Priority Zero)

Edge cases ini HARUS ditangani sebelum MVP launch. **Kegagalan mengimplementasi = risiko data loss.**

### 3.1 Sync & Data Integrity

#### Edge Case 3.1.1: Partial Sync Failure
**Source:** `EDGE_CASES_AND_TECHNICAL_CONSIDERATIONS.md` line 19-42

**Scenario:**
```
User membuat report dengan 7 photos offline
→ Goes online, sync dimulai
→ Report metadata sync berhasil ✅
→ Photos 1-4 upload berhasil ✅
→ Network gagal selama photo 5 upload ❌
→ Photos 5-7 tidak pernah dicoba ❌
```

**Problems:**
- Manager melihat report dengan photos yang tidak lengkap
- User tidak tahu photos mana yang gagal
- Report tampak "synced" tetapi data tidak lengkap

**Solution (MVP Requirement):**
1. **Per-photo sync status tracking** (diimplementasikan di Drift_Schema.md line 245-270)
   - Setiap photo memiliki field `sync_status`: pending | uploading | success | failed
   - Melacak `upload_attempts` (max 3 retries)
   - Menyimpan `upload_error` untuk debugging

2. **Photo upload resilience**
   - Gunakan `eagerError: false` di `Future.wait()` untuk melanjutkan meskipun satu gagal
   - Retry failed photos di next sync attempt
   - Tampilkan ke user: "Report synced, tetapi 3 photos gagal. Retrying..."

3. **Implementasi:**
```dart
// Sync_Strategy.md menyediakan full implementation
await Future.wait(
  photos.map((photo) => uploadSinglePhoto(photo)),
  eagerError: false, // Jangan berhenti saat first failure
);

// Cek results dan notify user
final failed = await getFailedPhotos(reportId);
if (failed.isNotEmpty) {
  showPartialSyncWarning('${failed.length} photos gagal, akan retry');
}
```

**Acceptance Test:**
- Buat report dengan 10 photos offline
- Simulasikan network failure di photo #5
- Verify: Photos 1-4 synced, photo 5 gagal, photos 6-10 synced
- Retry sync → Verify: Photo #5 synced dengan sukses

---

#### Edge Case 3.1.2: App Crash Mid-Sync
**Source:** `EDGE_CASES_AND_TECHNICAL_CONSIDERATIONS.md` line 75-91

**Scenario:**
```
User memiliki 20 pending reports
→ Sync dimulai, reports 1-10 upload ✅
→ App crash (battery dies, OS kills app)
→ User restart app
→ Reports mana yang telah di-sync? Mana yang belum?
```

**Problems:**
- Tidak ada transaction log untuk melacak sync progress
- Risiko duplicate uploads jika retry logic tidak idempotent
- User tidak memiliki visibilitas ke apa yang hilang

**Solution (MVP Requirement):**
1. **Sync transaction log table** (diimplementasikan di Drift_Schema.md line 389-420)
   ```dart
   class SyncTransactions extends Table {
     TextColumn get id => text()();
     TextColumn get entityType => text()(); // 'report', 'photo', etc.
     TextColumn get entityId => text()();
     TextColumn get status => text()(); // 'pending', 'syncing', 'success', 'failed'
     IntColumn get attemptCount => integer()();
     DateTimeColumn get lastAttemptAt => dateTime().nullable()();
     // ...
   }
   ```

2. **Resume incomplete syncs saat app restart**
   ```dart
   // Saat app startup
   Future<void> onAppStartup() async {
     final incomplete = await db.getIncompleteSyncTransactions();

     if (incomplete.isNotEmpty) {
       log('Ditemukan ${incomplete.length} incomplete syncs, resuming...');
       await resumeIncompleteSyncs(incomplete);
     }
   }
   ```

3. **Idempotent operations**
   - UUIDs generated di client-side mencegah duplicates
   - Server mengecek jika entity dengan ID sudah ada sebelum inserting

**Acceptance Test:**
- Buat 20 reports offline
- Mulai sync, kill app setelah 10 reports synced
- Restart app
- Verify: 10 reports yang sudah synced tidak re-uploaded
- Verify: Sisa 10 reports resume dan complete

---

#### Edge Case 3.1.3: Currency Precision Loss
**Source:** `EDGE_CASES_AND_TECHNICAL_CONSIDERATIONS.md` line 417-445

**Scenario:**
```
Project dibuat dengan estimated_value = 50.000.000,00 (50 juta rupiah)
→ Disimpan sebagai REAL (floating-point) di old schema
→ Setelah 100 edit operations: 50.000.000,00 → 49.999.999,98 (corrupted!)
```

**Problems:**
- Data finansial HARUS exact
- Audit trails menampilkan nilai yang tidak akurat
- Risiko compliance (inaccurate financial reporting)

**Solution (DIIMPLEMENTASIKAN - Drift_Schema.md line 119-136):**
1. **Simpan currency sebagai INTEGER cents**
   ```dart
   // LAMA (SALAH):
   RealColumn get estimatedValue => real()();

   // BARU (BENAR):
   IntColumn get estimatedValueCents => integer()();

   // Helper functions:
   int currencyToInt(double value) => (value * 100).round();
   double intToCurrency(int cents) => cents / 100.0;

   // Contoh:
   // User input: Rp 50.000.000,00
   // Simpan sebagai: 5.000.000.000 cents (INTEGER)
   // Tampilkan sebagai: Rp 50.000.000,00
   ```

2. **Terapkan ke semua currency fields:**
   - `projects.estimated_value_cents`
   - `projects.actual_value_cents`
   - `project_value_log.old_value_cents`
   - `project_value_log.new_value_cents`

**Acceptance Test:**
- Buat project dengan value Rp 50.000.000,00
- Edit value 100 kali (increase/decrease)
- Verify: Final value adalah TEPAT Rp 50.000.000,00 (tidak ada drift)

**Status:** ✅ FIXED di Drift_Schema.md v3.0

---

### 3.2 Authentication & Session Management

#### Edge Case 3.2.1: Token Expiry Mid-Sync
**Source:** `EDGE_CASES_AND_TECHNICAL_CONSIDERATIONS.md` line 363-388

**Scenario:**
```
User offline selama 7 hari
→ Auth token expired (7-day refresh token limit)
→ Goes online, sync dimulai
→ First API request → 401 Unauthorized
→ Semua syncs gagal
```

**Solution (MVP Requirement):**
```dart
// Wrap sync operations dengan token refresh
Future<void> syncWithTokenRefresh() async {
  try {
    await uploadReport(report);

  } on AuthException catch (e) {
    if (e.statusCode == 401) {
      log('Token expired, refreshing...');

      final session = await supabase.auth.refreshSession();

      if (session != null) {
        // Retry dengan new token
        await uploadReport(report);
      } else {
        // Refresh gagal → logout user
        await showAuthErrorAndLogout('Session expired. Silakan login kembali.');
      }
    }
  }
}
```

**Acceptance Test:**
- Buat reports offline selama >7 hari
- Token expires
- Go online, attempt sync
- Verify: Token auto-refreshes, sync sukses
- Jika refresh gagal: User logged out dengan baik, data preserved locally

---

### 3.3 User Experience Edge Cases

#### Edge Case 3.3.1: Form Data Loss on App Crash
**Source:** `EDGE_CASES_AND_TECHNICAL_CONSIDERATIONS.md` line 479-498

**Scenario:**
```
User mengisi report form selama 10 menit (12 dari 15 fields lengkap)
→ App crash (OS kills karena memory pressure)
→ User restart app
→ Semua form data hilang ❌
```

**Solution (MVP Requirement - US-5.1):**
1. **Draft auto-save setiap 30 detik**
   ```dart
   Timer.periodic(Duration(seconds: 30), (timer) {
     if (formDirty) {
       await db.saveReportDraft(
         reportId: draftId,
         formData: currentFormData,
         isDraft: true,
       );
       log('Draft auto-saved');
     }
   });
   ```

2. **Resume draft saat app restart**
   ```dart
   @override
   void initState() {
     super.initState();
     _checkForDrafts();
   }

   Future<void> _checkForDrafts() async {
     final drafts = await db.getDraftReports();

     if (drafts.isNotEmpty) {
       final resume = await showDialog<bool>(
         context: context,
         builder: (context) => AlertDialog(
           title: Text('Resume Draft?'),
           content: Text('Anda memiliki unsaved report dari 10 menit yang lalu.'),
           actions: [
             TextButton(onPressed: () => Navigator.pop(context, false), child: Text('Buang')),
             TextButton(onPressed: () => Navigator.pop(context, true), child: Text('Lanjutkan')),
           ],
         ),
       );

       if (resume == true) {
         _loadDraft(drafts.first);
       }
     }
   }
   ```

**Acceptance Test:**
- Isi report form 50%
- Tunggu 30 detik (auto-save triggers)
- Kill app (force close)
- Restart app
- Verify: Prompt ditampilkan untuk resume draft
- Verify: Semua filled data intact

---

#### Edge Case 3.3.2: Pagination Performance Degradation
**Source:** Acceptable Risk Table (PRD.md) - **TIDAK DAPAT DITERIMA untuk skip pagination**

**Scenario:**
```
Setelah 6 bulan penggunaan:
  - User memiliki 500 reports
  - View Reports list load semua 500 sekaligus
  - Query memakan 8 detik, UI freeze
  - Poor user experience
```

**Solution (MVP Requirement):**
```dart
// Implementasi pagination untuk semua list views
Future<List<LocalReport>> getReportsPaginated({
  required int page,
  required int pageSize,
}) {
  final offset = page * pageSize;

  return (select(localReports)
    ..where((tbl) => tbl.deletedAt.isNull())
    ..orderBy([(tbl) => OrderingTerm.desc(tbl.visitDate)])
    ..limit(pageSize, offset: offset))
    .get();
}

// Usage di UI:
// Page 1: getReportsPaginated(page: 0, pageSize: 20) → reports 1-20
// User scrolls → load next page
// Page 2: getReportsPaginated(page: 1, pageSize: 20) → reports 21-40
```

**Berlaku untuk:**
- US-2.1: View List Companies
- US-3.1: View List Contacts
- US-4.1: View List Projects
- US-6.1: View My Reports
- US-6.2: View Team Reports (Manager)

**Acceptance Test:**
- Insert 500 reports ke database
- Load reports list
- Verify: Initial load <500ms
- Verify: Hanya 20 reports loaded initially
- Scroll down → Verify: 20 berikutnya loaded dengan smooth

---

## 4. High Priority Edge Cases (Harus Ditangani Sebelum Scale)

Ini dapat ditunda sedikit tetapi harus diimplementasikan sebelum expand melebihi 5 users.

### 4.1 GPS & Location Handling

#### Edge Case 4.1.1: GPS Permission Denied Permanently
**Source:** `EDGE_CASES_AND_TECHNICAL_CONSIDERATIONS.md` line 286-310

**Problem:** App menunggu 10 detik untuk GPS setiap kali, bahkan jika permission denied.

**Solution:**
```dart
Future<Position?> captureGPS() async {
  final permission = await Geolocator.checkPermission();

  if (permission == LocationPermission.deniedForever) {
    // Jangan attempt GPS, show info sekali
    showSnackBar('Location disabled. Enable di Settings untuk auto-capture location.');
    return null;
  }

  // Jika tidak, attempt dengan timeout
  return await Geolocator.getCurrentPosition(
    timeoutDuration: Duration(seconds: 10),
  ).catchError((_) => null);
}
```

---

### 4.2 Network & Connectivity

#### Edge Case 4.2.1: Slow 2G Connection Timeouts
**Source:** `EDGE_CASES_AND_TECHNICAL_CONSIDERATIONS.md` line 553-579

**Problem:** Fixed 30-second timeout terlalu pendek untuk koneksi 2G.

**Solution (Phase 2):**
```dart
// Adaptive timeout berdasarkan connection type
Duration getTimeout(ConnectivityResult connectionType) {
  switch (connectionType) {
    case ConnectivityResult.mobile:
      // Assume 2G/3G, be generous
      return Duration(seconds: 120);
    case ConnectivityResult.wifi:
      return Duration(seconds: 30);
    default:
      return Duration(seconds: 60);
  }
}
```

---

## 5. Medium Priority Edge Cases (Phase 2)

### 5.1 Advanced Conflict Resolution

**Current (MVP):** Last-Write-Wins
**Phase 2:** User-prompted conflict resolution dengan merge UI

### 5.2 Mock GPS Detection

**Current (MVP):** Tidak ada detection
**Phase 2:** Detect `location.isMock` dan warn user

### 5.3 Storage Quota Management

**Current (MVP):** Tidak ada proactive monitoring
**Phase 2:** Alert di 80% storage, implementasi cleanup policies

---

## 6. Data Integrity Constraints

### 6.1 Database Constraints

| Constraint | Implementation | Error Handling |
|------------|----------------|----------------|
| Company name unique | Case-insensitive unique index | "Company sudah ada" |
| Foreign key integrity | ON DELETE RESTRICT | "Tidak dapat hapus: memiliki dependencies" |
| Value > 0 | CHECK constraint (projects) | "Value harus positif" |
| Visit date not future | App-level validation | "Visit date tidak boleh di future" |
| Min 1 attendee | App-level validation | "Pilih minimal 1 attendee" |
| Exactly 1 primary attendee | App-level validation | "Tandai tepat 1 primary attendee" |

### 6.2 Soft Delete Enforcement

Semua deletes adalah soft deletes (timestamp `deleted_at`). Hard deletes TIDAK PERNAH dilakukan di app.

**Cleanup (Phase 2):** Background job untuk hard-delete records yang lebih tua dari 2 tahun.

---

## 7. Error Handling Strategy

### 7.1 Error Categories

| Error Type | User Message | Action | Log ke Sentry? |
|------------|--------------|--------|----------------|
| Network timeout | "Koneksi lambat. Retrying..." | Auto-retry | No (expected) |
| Auth expired | "Session expired. Refreshing..." | Auto-refresh token | No |
| Validation error | "Nama company diperlukan" | Show form error | No |
| Sync failure | "Sync gagal. Akan retry otomatis." | Queue retry | Yes |
| Unknown error | "Terjadi kesalahan. Coba lagi." | Show retry button | Yes |
| Storage quota | "Storage penuh. Silakan hapus old photos." | Block new uploads | Yes |

### 7.2 User-Friendly Error Messages

**Buruk (technical):**
- "PostgrestException: 23505 duplicate key value violates unique constraint"

**Baik (user-friendly):**
- "Nama company ini sudah digunakan. Silakan pilih nama yang berbeda."

**Implementasi:**
```dart
String getFriendlyErrorMessage(Exception e) {
  if (e is PostgrestException) {
    if (e.code == '23505') {
      return 'Item ini sudah ada. Silakan gunakan nama yang berbeda.';
    } else if (e.code == '23503') {
      return 'Tidak dapat hapus: item ini sedang digunakan di tempat lain.';
    }
  }

  return 'Terjadi kesalahan. Silakan coba lagi.';
}
```

---

## 8. Testing Requirements

### 8.1 Critical Edge Case Tests

| Test | Priority | Type | Coverage |
|------|----------|------|----------|
| Partial sync recovery | P0 | Integration | Sync engine |
| App crash selama sync | P0 | Integration | Transaction log |
| Currency precision | P0 | Unit | Currency conversion |
| Token refresh | P0 | Integration | Auth middleware |
| Draft auto-save | P0 | Integration | Report form |
| Pagination performance | P0 | Performance | List queries |
| RLS policy enforcement | P0 | Integration | Database access |

**Untuk complete test plan:** Lihat Test_Plan.md

---

## 9. Monitoring & Alerts

### 9.1 Critical Metrics to Monitor

| Metric | Alert Threshold | Action |
|--------|----------------|--------|
| Sync failure rate | >5% | Investigate sync engine |
| App crash rate | >5% | Cek Sentry logs |
| Data loss incidents | >0 | **SEGERA** investigate |
| Storage usage | >80% | Notify admin, rencanakan upgrade |
| Database query time | >1s | Review query optimization |
| Token refresh failures | >1% | Cek Supabase service status |

**Implementasi:** Sentry + Supabase metrics dashboard

---

## 10. Acceptable Risk Register

Berdasarkan PRD.md risk table, risks ini dapat diterima untuk MVP:

| Risk | Acceptable? | Mitigation |
|------|-------------|------------|
| Occasional app crashes | ✅ Ya | Draft auto-save mencegah data loss |
| Stale manager data (1-2j old) | ✅ Ya | Manager dapat refresh manual |
| Token expiry setelah 7 hari | ✅ Ya | Auto-refresh diimplementasikan |
| Last-Write-Wins conflicts | ✅ Ya (MVP) | Phase 2: Conflict resolution UI |

**Unacceptable Risks:**
- ❌ Data loss (1-2 reports)
- ❌ Currency rounding errors
- ❌ Missing pagination

---

## 11. Implementation Priorities

### Priority 0 (MVP Blockers)
- ✅ Currency sebagai INTEGER (DONE - Drift_Schema v3.0)
- ✅ Sync transaction log (DONE - Drift_Schema v3.0)
- ✅ Per-photo sync status (DONE - Drift_Schema v3.0)
- ⏳ Draft auto-save setiap 30s (US-5.1)
- ⏳ Pagination untuk lists (US-2.1, US-4.1, US-6.1, US-6.2)
- ⏳ Token refresh saat expiry (US-1.1)
- ⏳ RLS policy testing (US-6.2)

### Priority 1 (Sebelum Scale ke 10 Users)
- GPS permission handling
- Adaptive network timeouts
- User-friendly error messages
- Comprehensive E2E tests

### Priority 2 (Phase 2)
- Advanced conflict resolution
- Mock GPS detection
- Storage quota management
- Automated cleanup jobs

---

## 12. Referensi

**Sumber Edge Cases:**
- `FOR_EXTERNAL_DEVELOPERS/EDGE_CASES_AND_TECHNICAL_CONSIDERATIONS.md` (768 lines, 11 categories)

**Referensi Implementasi:**
- `database/Drift_Schema.md` - Currency fix (line 119-149), Sync transactions (line 389-420)
- `database/Sync_Strategy.md` - Complete sync implementation dengan edge case handling
- `Architecture.md` - System design dan patterns
- `Functional_Spec.md` - User stories dengan acceptance criteria
- `Test_Plan.md` - Testing strategy untuk edge cases

---

**Document Status:** ✅ Complete - Edge cases dikonsolidasikan dan diprioritaskan
**Terakhir Diperbarui:** Oktober 2025
**Next Review:** Setelah MVP pilot completion
