# MVP Phase - Rencana Implementasi Teknis

← [Sebelumnya: README](./README.md)

---

**Project:** CSS Sales Report Application
**Phase:** MVP (Minimum Viable Product)
**Versi:** 1.1 (Revised - Edit Included)
**Terakhir Diperbarui:** 2025-10-28
**Target Durasi:** 10-12 Minggu
**Total Story Points:** 77 (73% dari full scope)

---

## 📋 Ringkasan Eksekutif

### Tujuan MVP
Deliver working offline-first mobile app yang memungkinkan **2 sales reps** membuat structured visit reports lebih baik dari WhatsApp, dengan **1 manager** memiliki read-only visibility ke semua team activities.

### Kriteria Sukses
- ✅ 2 sales reps menggunakan app daily selama 30 hari berturut-turut
- ✅ Zero data loss incidents
- ✅ Sync success rate >95%
- ✅ Sales reps prefer app dibanding WhatsApp (survey >4/5)
- ✅ Manager dapat view semua reports secara real-time

### Yang Termasuk vs Tidak Termasuk

**✅ TERMASUK di MVP:**
- Full offline-first capability
- Create companies, contacts, projects, reports
- **Edit companies, contacts, projects** (data quality essential)
- View semua created entities
- Manager dashboard dengan team reports
- Draft auto-save (setiap 30s)
- Pagination untuk semua lists
- Sync transaction log (rollback capability)
- Per-photo sync status tracking
- Token refresh on expiry
- Last-Write-Wins conflict resolution (automatic)

**❌ TIDAK TERMASUK di MVP (Phase 2):**
- Edit reports (defer ke Phase 2)
- Delete functionality (all entities)
- Dashboard statistics/analytics
- User profile settings
- Export ke Excel/PDF
- Advanced conflict resolution UI (user-prompted)

---

## 🎯 Breakdown Scope MVP

### EPIC 1: Authentication (5 Story Points)

#### ✅ US-1.1: Login with Email & Password
**Story Points:** 3
**Prioritas:** P0 (Critical)

**Yang Termasuk:**
- Email/password login via Supabase Auth
- Session persistence (flutter_secure_storage)
- Token refresh on expiry (auto-refresh 1 jam sebelum 7-day expiry)
- Error handling (network errors, invalid credentials)

**Implementasi Teknis:**
```dart
// lib/features/auth/data/repositories/auth_repository_impl.dart
Future<User> login(String email, String password) async {
  try {
    final response = await _supabase.auth.signInWithPassword(
      email: email,
      password: password,
    );

    // Store session token
    await _secureStorage.write(key: 'session_token', value: response.session.accessToken);

    return User.fromSupabase(response.user);
  } on AuthException catch (e) {
    throw LoginFailureException(e.message);
  }
}
```

**Tes Penerimaan:**
- ✅ Valid credentials → Redirect ke home
- ✅ Invalid credentials → Show error message
- ✅ Session persists setelah app restart
- ✅ Token auto-refreshes sebelum expiry

---

#### ✅ US-1.2: Logout from App
**Story Points:** 2
**Prioritas:** P1 (High)

**Yang Termasuk:**
- Logout button di UI
- Confirmation dialog
- Clear session token
- Keep local database (Drift) intact
- Works offline

**Alasan Termasuk:** Essential untuk security, memungkinkan sales rep switch accounts jika diperlukan.

---

### EPIC 2: Manage Companies (8 Story Points)

#### ✅ US-2.1: View List of Companies
**Story Points:** 3
**Prioritas:** P0 (Critical)

**Yang Termasuk:**
- List semua companies yang dibuat oleh current user
- **Pagination: 20 items per page** (P0 requirement)
- Search by company name
- Sort by created date (newest first)
- Pull-to-refresh

**Alasan Pagination adalah P0:**
- Performance requirement untuk prevent UI freeze dengan 100+ companies
- User stated: "Pagination required untuk list views"

**Implementasi Teknis:**
```dart
// lib/features/companies/data/datasources/company_local_datasource.dart
Stream<List<Company>> watchCompanies({
  required int page,
  required int limit,
  String? searchQuery,
}) {
  final query = (select(companies)
    ..where((tbl) => tbl.deletedAt.isNull())
    ..orderBy([(tbl) => OrderingTerm.desc(tbl.createdAt)])
    ..limit(limit, offset: page * limit));

  if (searchQuery != null && searchQuery.isNotEmpty) {
    query.where((tbl) => tbl.name.like('%$searchQuery%'));
  }

  return query.watch();
}
```

**Tes Penerimaan:**
- ✅ Shows first 20 companies
- ✅ Load more on scroll
- ✅ Search filters list
- ✅ Pull-to-refresh updates list

---

#### ✅ US-2.2: Create New Company
**Story Points:** 5
**Prioritas:** P0 (Critical)

**Yang Termasuk:**
- Form fields: Name (required), Address (optional), City (optional)
- Validation: Name required, max 200 chars
- Save ke local DB (Drift) immediately
- Auto-sync ke Supabase ketika online
- Offline capability

**Tes Penerimaan:**
- ✅ Create offline → Saved ke SQLite
- ✅ Go online → Auto-synced ke Supabase
- ✅ Validation errors ditunjukkan dengan jelas

---

#### ✅ US-2.3: Edit Company
**Story Points:** 2
**Prioritas:** P1 (High)

**Yang Termasuk:**
- Edit name, address, city
- Reuse Create Company form (pre-populated dengan existing data)
- Update local DB + sync ke Supabase
- **Last-Write-Wins conflict resolution** (automatic, no UI)
- Works offline

**Alasan Termasuk di MVP:**
- **Data quality critical:** Typos di company names membuat confusion untuk manager
- **Prevents duplicate records:** Tanpa edit, users membuat "PT ABC Typo" + "PT ABC Correct" = dirty data
- **User expectation:** Basic CRUD expected (Create + Edit + View)
- **Low complexity:** Reuse create form, UPDATE query instead of INSERT (1-2 hari kerja)

**Implementasi Teknis:**
```dart
Future<void> updateCompany(Company updated) async {
  // 1. Update local DB
  await _localDataSource.updateCompany(updated.copyWith(
    updatedAt: DateTime.now(),
  ));

  // 2. Queue for sync
  await _syncQueue.enqueue(SyncOperation(
    entityType: 'company',
    entityId: updated.id,
    operation: 'update',
  ));

  // 3. Sync if online
  if (await _connectivity.isOnline) {
    _syncService.triggerSync();
  }
}

// Last-Write-Wins conflict resolution (automatic)
Future<void> syncCompanyUpdate(Company local) async {
  final remote = await supabase
    .from('companies')
    .select()
    .eq('id', local.id)
    .single();

  if (DateTime.parse(remote['updated_at']).isAfter(local.updatedAt)) {
    // Server is newer, overwrite local
    await db.updateCompany(Company.fromJson(remote));
  } else {
    // Local is newer, upload to server
    await supabase
      .from('companies')
      .update(local.toJson())
      .eq('id', local.id);
  }
}
```

**Tes Penerimaan:**
- ✅ Edit company offline → Saved locally
- ✅ Go online → Syncs ke Supabase
- ✅ Two users edit same company → Latest timestamp wins (no errors)
- ✅ Form validation sama seperti create

---

#### ❌ US-2.4: Delete Company (DEFERRED ke Phase 2)
**Story Points:** 2
**Rationale:** Delete memerlukan cascade logic (warn about contacts/projects), soft delete complexity, accidental delete risk. Edit solves 80% dari data correction needs.

---

### EPIC 3: Manage Contacts (10 Story Points)

#### ✅ US-3.1: View List of Contacts per Company
**Story Points:** 3
**Prioritas:** P0 (Critical)

**Yang Termasuk:**
- List contacts untuk selected company
- **Pagination: 20 items per page**
- Search by name/position/phone
- Show primary contact badge

**Tes Penerimaan:**
- ✅ Lists contacts untuk company
- ✅ Pagination works dengan 20+ contacts
- ✅ Search filters list

---

#### ✅ US-3.2: Create New Contact
**Story Points:** 5
**Prioritas:** P0 (Critical)

**Yang Termasuk:**
- Form fields: Name (required), Position, Phone, Email, Is Primary
- Validation: Name required, email format, phone format
- Linked ke company (foreign key)
- Save offline, sync online

**Tes Penerimaan:**
- ✅ Create contact untuk company
- ✅ Primary contact flag works
- ✅ Syncs ke Supabase ketika online

---

#### ✅ US-3.3: Edit Contact
**Story Points:** 2
**Prioritas:** P1 (High)

**Yang Termasuk:**
- Edit name, position, phone, email, is_primary
- Reuse Create Contact form (pre-populated)
- Update local DB + sync ke Supabase
- **Last-Write-Wins conflict resolution** (automatic)
- Works offline

**Alasan Termasuk di MVP:**
- **Contact details change frequently:** Phone numbers, email addresses, job titles (promotion)
- **Same rationale as Edit Company:** Data quality, prevent duplicates, user expectation
- **Low complexity:** Reuse create form, UPDATE query (1-2 hari kerja)

**Tes Penerimaan:**
- ✅ Edit contact offline → Saved locally
- ✅ Go online → Syncs ke Supabase
- ✅ Two users edit same contact → Latest timestamp wins
- ✅ Can toggle is_primary flag

---

#### ❌ US-3.4: Delete Contact (DEFERRED ke Phase 2)
**Story Points:** 2
**Rationale:** Delete memerlukan cascade checks (contact is project primary? report attendee?), complexity not justified untuk MVP.

---

### EPIC 4: Manage Projects (11 Story Points)

#### ✅ US-4.1: View List of Projects per Company
**Story Points:** 3
**Prioritas:** P0 (Critical)

**Yang Termasuk:**
- List projects untuk selected company
- **Pagination: 20 items per page**
- Show project status (Active, Won, Lost, On Hold)
- Show estimated value
- Filter by status

**Tes Penerimaan:**
- ✅ Lists projects untuk company
- ✅ Pagination dengan 20+ projects
- ✅ Filter by status works

---

#### ✅ US-4.2: Create New Project
**Story Points:** 8
**Prioritas:** P0 (Critical)

**Yang Termasuk:**
- Form fields: Project Name, Type, Segmentation, Source, Status, Estimated Value, Expected Close Date, Primary Contact
- **Currency as INTEGER cents** (P0 requirement - already in schema)
- Validation: Name required, value >0, date not in past
- Linked ke company + primary contact
- Save offline, sync online

**Technical Implementation (Currency):**
```dart
// Helper functions in lib/core/utils/currency_helpers.dart
int currencyToInt(double value) => (value * 100).round();
double intToCurrency(int cents) => cents / 100.0;

// Usage
final inputValue = 50000000.00; // Rp 50 million
final storedValue = currencyToInt(inputValue); // 5000000000 cents (INTEGER)

// Display
final displayValue = intToCurrency(storedValue); // 50000000.00
```

**Tes Penerimaan:**
- ✅ Create project dengan Rp 50,000,000 → Stored as 5,000,000,000 cents
- ✅ Edit 100 kali → Value masih exact (no precision loss)
- ✅ Syncs ke Supabase dengan INTEGER value

---

#### ✅ US-4.3: Edit Project
**Story Points:** 3 (INCLUDED in MVP - Exception to edit rule)
**Prioritas:** P0 (Critical)

**Why Include Edit untuk Projects (tapi tidak companies/contacts):**
- Projects memiliki **Estimated Value** yang sering berubah (price negotiations)
- Projects memiliki **Status** yang berubah (Active → Won/Lost)
- Value changes trigger audit log (project_value_log table)
- Core untuk tracking sales pipeline

**Yang Termasuk:**
- Edit semua fields (sama seperti create)
- **Value change logging** (audit trail)
- Status updates
- Works offline, syncs online

**Technical Implementation (Value Change Log):**
```dart
// When project value changes
Future<void> updateProjectValue(String projectId, int newValueCents) async {
  final oldValue = await db.getProject(projectId).estimatedValueCents;

  // Update project
  await db.updateProject(projectId, estimatedValueCents: newValueCents);

  // Log value change for audit trail
  await db.insertProjectValueLog(
    ProjectValueLogCompanion(
      id: Value(uuid.v4()),
      projectId: Value(projectId),
      oldValueCents: Value(oldValue),
      newValueCents: Value(newValueCents),
      changedAt: Value(DateTime.now()),
      changedBy: Value(currentUserId),
    ),
  );
}
```

**Tes Penerimaan:**
- ✅ Edit project value → Log entry created
- ✅ Edit status Active → Won → Log created
- ✅ Works offline, syncs later

---

#### ❌ US-4.4: Delete Project (DEFERRED ke Phase 2)
**Story Points:** 1

---

### EPIC 5: Create Report (16 Story Points)

#### ✅ US-5.1: Create Visit Report
**Story Points:** 13
**Prioritas:** P0 (Critical - CORE VALUE PROPOSITION)

**Yang Termasuk:**
- Form fields: Project (dropdown), Report Type (dropdown), Visit Date, Attendees (multi-select), Notes, Next Action, Outcome, Photos (max 10), GPS (auto-capture, optional)
- **Draft auto-save setiap 30 detik** (P0 requirement)
- Photo compression (<500KB per photo)
- **Per-photo sync status tracking** (P0 requirement - already in schema)
- GPS auto-capture dengan 10s timeout (non-blocking jika denied)
- Save offline, sync online

**Draft Auto-Save Implementation:**
```dart
// In Report Form BLoC
Timer? _autoSaveTimer;

void _startAutoSave() {
  _autoSaveTimer?.cancel();
  _autoSaveTimer = Timer.periodic(
    const Duration(seconds: 30),
    (_) => add(SaveDraftEvent()),
  );
}

Future<void> _onSaveDraft(SaveDraftEvent event, Emitter<ReportFormState> emit) async {
  // Save to local DB with is_draft = true
  await _reportRepository.saveDraft(event.report.copyWith(isDraft: true));
  emit(state.copyWith(draftSavedAt: DateTime.now()));
}
```

**Per-Photo Sync Status Implementation:**
```dart
// Photo upload with individual status tracking
Future<void> uploadReportPhotos(String reportId, List<LocalAttachment> photos) async {
  await Future.wait(
    photos.map((photo) async {
      try {
        // Mark as uploading
        await db.updatePhotoSyncStatus(photo.id, 'uploading');

        // Compress
        final compressed = await FlutterImageCompress.compressWithFile(
          photo.localPath,
          quality: 85,
          minWidth: 1920,
        );

        // Upload
        final remotePath = '${userId}/${year}/${month}/${reportId}/${photo.id}.jpg';
        await supabase.storage.from('attachments').uploadBinary(
          remotePath,
          compressed,
        );

        // Mark as success
        await db.updatePhotoSyncStatus(photo.id, 'success');
      } catch (e) {
        // Mark as failed, increment attempts
        await db.updatePhotoSyncStatus(photo.id, 'failed', error: e.toString());
        await db.incrementPhotoUploadAttempts(photo.id);
      }
    }),
    eagerError: false, // Don't stop if one photo fails
  );
}
```

**Tes Penerimaan:**
- ✅ Form auto-saves draft setiap 30s
- ✅ Create report dengan 10 photos offline
- ✅ Go online → Report syncs, photos upload individually
- ✅ Simulate network failure pada photo 5 → Photos 1-4 success, 5 failed, 6-10 success
- ✅ Retry sync → Photo 5 sekarang succeeds
- ✅ GPS captured jika permission granted (non-blocking jika denied)

---

#### ✅ US-5.2: View Report Detail
**Story Points:** 3
**Prioritas:** P0 (Critical)

**Yang Termasuk:**
- Display semua report fields (read-only)
- Show attendees list dengan primary contact badge
- Photo gallery (tap untuk full screen)
- GPS location on map (jika available)
- Sync status indicator

**Tes Penerimaan:**
- ✅ View report dengan semua details
- ✅ Photos load dari local cache (offline)
- ✅ GPS shows on map jika available

---

#### ❌ US-5.3: Edit Report (DEFERRED ke Phase 2)
**Story Points:** 3
**Rationale:** Untuk MVP pilot, membuat new report acceptable jika mistake. Edit adalah enhancement.

#### ❌ US-5.4: Delete Report (DEFERRED ke Phase 2)
**Story Points:** 1

---

### EPIC 6: View Reports & Dashboard (10 Story Points)

#### ✅ US-6.1: View My Reports (Sales Rep)
**Story Points:** 5
**Prioritas:** P0 (Critical)

**Yang Termasuk:**
- List semua reports yang dibuat oleh current user
- **Pagination: 20 items per page** (P0 requirement)
- Filter by date range
- Search by company/project name
- Sort by visit date (newest first)
- Show sync status per report

**Tes Penerimaan:**
- ✅ Shows first 20 reports
- ✅ Pagination loads more
- ✅ Filter by date works
- ✅ Search filters list

---

#### ✅ US-6.2: View Team Reports (Manager)
**Story Points:** 5
**Prioritas:** P0 (Critical)

**Yang Termasuk:**
- Manager sees ALL reports dari all sales reps (read-only)
- **RLS policy enforcement** (P0 requirement - must test thoroughly)
- Pagination (20 items per page)
- Filter by sales rep, date range, company
- Search by company/project name

**RLS Policy Testing (Critical):**
```sql
-- Supabase RLS policy (must be tested with 2+ users)
CREATE POLICY "Users see own reports OR manager sees all"
ON reports
FOR SELECT
USING (
  auth.uid() = created_by
  OR
  auth.uid() IN (SELECT id FROM users WHERE role = 'manager')
);
```

**Tes Penerimaan:**
- ✅ Sales Rep A sees only Rep A's reports
- ✅ Sales Rep B sees only Rep B's reports
- ✅ Manager sees BOTH Rep A and Rep B reports
- ✅ Sales rep CANNOT see other rep's reports (security test)

---

#### ❌ US-6.3: Simple Dashboard Statistics (DEFERRED ke Phase 2)
**Story Points:** 5
**Rationale:** Core value adalah structured reporting. Analytics adalah enhancement.

---

### EPIC 7: Offline & Sync (15 Story Points)

#### ✅ US-7.1: Offline Create/Edit
**Story Points:** 5
**Prioritas:** P0 (Critical - CORE REQUIREMENT)

**Yang Termasuk:**
- Semua create operations work offline
- Data saved ke Drift (SQLite) immediately
- No blocking pada network checks
- Offline indicator di UI

**Implementasi Teknis:**
```dart
// Repository pattern - always write to local first
@override
Future<void> createReport(Report report) async {
  // 1. Save to local DB (always succeeds, even offline)
  await _localDataSource.insertReport(report);

  // 2. Queue for sync
  await _syncQueue.enqueue(SyncOperation(
    entityType: 'report',
    entityId: report.id,
    operation: 'create',
  ));

  // 3. Trigger sync if online (non-blocking)
  if (await _connectivity.isOnline) {
    _syncService.triggerSync(); // Fire and forget
  }
}
```

**Tes Penerimaan:**
- ✅ Turn off WiFi/data → Create company → Success
- ✅ Create contact offline → Success
- ✅ Create project offline → Success
- ✅ Create report dengan photos offline → Success
- ✅ Turn on WiFi → All entities auto-sync

---

#### ✅ US-7.2: Auto Sync When Online
**Story Points:** 8
**Prioritas:** P0 (Critical - CORE REQUIREMENT)

**Yang Termasuk:**
- Background auto-sync setiap 5 menit ketika online
- **Sync transaction log** (P0 requirement - already in schema)
- Retry logic dengan exponential backoff (2s → 4s → 8s, max 3 attempts)
- Sync order: Companies → Contacts → Projects → Reports → Attachments
- Resume incomplete syncs on app restart
- Idempotent operations (UUIDs prevent duplicates)

**Sync Order (Referential Integrity):**
```dart
Future<void> performFullSync() async {
  // Sync in dependency order to maintain foreign key constraints
  await _syncCompanies();     // No dependencies
  await _syncContacts();      // Depends on companies
  await _syncProjects();      // Depends on companies + contacts
  await _syncReports();       // Depends on projects
  await _syncAttachments();   // Depends on reports
}
```

**Transaction Log for Rollback:**
```dart
// Create sync transaction record
Future<void> syncReport(Report report) async {
  final txnId = uuid.v4();

  try {
    // 1. Log sync start
    await db.insertSyncTransaction(SyncTransactionCompanion(
      id: Value(txnId),
      entityType: Value('report'),
      entityId: Value(report.id),
      status: Value('syncing'),
      attemptCount: Value(1),
      createdAt: Value(DateTime.now()),
    ));

    // 2. Upload to Supabase
    await supabase.from('reports').insert(report.toJson());

    // 3. Mark success
    await db.updateSyncTransaction(txnId, status: 'success');

  } catch (e) {
    // 4. Mark failed, increment attempt count
    await db.updateSyncTransaction(
      txnId,
      status: 'failed',
      errorMessage: e.toString(),
    );

    // 5. Schedule retry if attempts < 3
    final attempts = await db.getSyncTransactionAttempts(txnId);
    if (attempts < 3) {
      await scheduleRetry(txnId, attempts);
    }
  }
}
```

**Resume on App Restart:**
```dart
// On app startup
Future<void> onAppStartup() async {
  // Find incomplete sync transactions
  final incomplete = await db
    .select(db.syncTransactions)
    .where((tbl) => tbl.status.isNotIn(['success']))
    .get();

  if (incomplete.isNotEmpty) {
    log('Found ${incomplete.length} incomplete syncs, resuming...');
    await resumeIncompleteSyncs(incomplete);
  }
}
```

**Tes Penerimaan:**
- ✅ Create 20 entities offline → Go online → All sync in correct order
- ✅ Kill app mid-sync → Restart → Resumes dari last successful sync
- ✅ Network fails on entity #10 → Retry dengan exponential backoff
- ✅ Setelah 3 failed attempts → User notified, manual retry available
- ✅ Duplicate prevention: Create report offline dengan UUID → Sync twice → Only 1 record di Supabase

---

#### ✅ US-7.3: Manual Sync Button
**Story Points:** 2
**Prioritas:** P1 (High)

**Yang Termasuk:**
- Manual sync button di UI (force sync now)
- Shows sync progress (X/Y entities synced)
- Shows last sync time
- Pull-to-refresh pada list views triggers sync

**Alasan Termasuk:** Memberi users control, builds trust dalam offline-first system.

**Tes Penerimaan:**
- ✅ Tap sync button → Immediate sync attempt
- ✅ Shows progress (e.g., "Syncing 5/10 reports...")
- ✅ Pull-to-refresh pada reports list → Sync + refresh

---

#### ❌ US-7.4: Download Team Data (Manager) (DEFERRED ke Phase 2)
**Story Points:** 2
**Rationale:** Manager always online, can view reports di app. Download adalah enhancement.

---

### EPIC 8: User Profile & Settings (0 Story Points - DEFERRED)

#### ❌ US-8.1: View User Profile (DEFERRED ke Phase 2)
**Story Points:** 2

#### ❌ US-8.2: App Settings (DEFERRED ke Phase 2)
**Story Points:** 3

**Rationale:** Not critical untuk MVP pilot. Hard-coded settings acceptable untuk 2 users.

---

## 📊 MVP Summary

### Breakdown Story Points

| Epic | Stories Included | Stories Deferred | Points (MVP) | Points (Deferred) |
|------|------------------|------------------|--------------|-------------------|
| EPIC 1: Authentication | 2/2 | 0 | 5 | 0 |
| EPIC 2: Companies | 3/4 | 1 | 10 | 2 |
| EPIC 3: Contacts | 3/4 | 1 | 10 | 2 |
| EPIC 4: Projects | 3/4 | 1 | 11 | 1 |
| EPIC 5: Reports | 2/4 | 2 | 16 | 4 |
| EPIC 6: Dashboard | 2/3 | 1 | 10 | 5 |
| EPIC 7: Offline/Sync | 3/4 | 1 | 15 | 2 |
| EPIC 8: Profile | 0/2 | 2 | 0 | 5 |
| **TOTAL** | **18/27** | **9** | **77** | **21** |

**MVP = 77 story points (79% dari 98 story points across 8 epics)**
**Deferred = 21 story points (21%)**

Note: Total adalah 98 instead of 106 karena 8 points sudah dialokasikan ke Epic 8 (deferred).

### Apa yang Membuat Ini MVP?

**Core Value Delivered:**
1. ✅ Sales reps dapat create **dan edit** structured data offline (lebih baik dari WhatsApp)
2. ✅ Reports include company context, project tracking, photos, GPS
3. ✅ Manager memiliki visibility ke semua team activities secara real-time
4. ✅ Zero data loss (draft auto-save, sync transaction log, per-photo tracking)
5. ✅ Works offline, auto-syncs ketika online
6. ✅ Data quality maintained (edit functionality prevents duplicate/incorrect records)

**MVP Principles Applied:**
- **Edit untuk master data:** Companies, Contacts, Projects editable (prevents duplicate records, typo corrections)
- **No delete:** Delete deferred (requires cascade logic, soft delete complexity, accidental delete risk)
- **Last-Write-Wins:** Automatic conflict resolution untuk 2 users (advanced UI deferred ke Phase 2)
- **Pagination mandatory:** Performance requirement (list views freeze dengan 100+ items)
- **Security non-negotiable:** RLS policies tested thoroughly
- **Data integrity non-negotiable:** Currency as INTEGER, transaction log, per-photo status

---

## 🏗️ Implementation Order (Sprint Planning)

### Sprint 1: Foundation (Week 1-2)
**Goal:** Setup architecture, authentication, database schema

**Tasks:**
1. Project setup (Flutter, Supabase, Drift)
2. Folder structure (Clean Architecture: presentation/domain/data)
3. Database migrations (Supabase + Drift)
4. US-1.1: Login implementation
5. US-1.2: Logout implementation

**Deliverable:** Working login/logout, app shell

---

### Sprint 2: Companies & Contacts (Week 3-4)
**Goal:** Full CRUD untuk master data (Create, View, Edit)

**Tasks:**
1. US-2.1: View Companies (dengan pagination)
2. US-2.2: Create Company
3. US-2.3: Edit Company (Last-Write-Wins conflict resolution)
4. US-3.1: View Contacts (dengan pagination)
5. US-3.2: Create Contact
6. US-3.3: Edit Contact (Last-Write-Wins conflict resolution)
7. Offline capability untuk companies/contacts (create + edit)
8. Basic sync (companies/contacts only, dengan UPDATE support)

**Deliverable:** Dapat create dan edit companies/contacts offline, sync online dengan conflict resolution

---

### Sprint 3: Projects (Week 5-6)
**Goal:** Project management dengan currency precision

**Tasks:**
1. US-4.1: View Projects (dengan pagination)
2. US-4.2: Create Project (dengan INTEGER currency)
3. US-4.3: Edit Project (dengan value change logging)
4. Currency helper functions (currencyToInt, intToCurrency)
5. Project value log audit trail
6. Sync projects + value logs

**Deliverable:** Dapat create dan edit projects dengan exact currency values

---

### Sprint 4: Reports - Core Feature (Week 7-8)
**Goal:** Create visit reports dengan draft auto-save

**Tasks:**
1. US-5.1: Create Report (form, validation)
2. Draft auto-save implementation (setiap 30s)
3. Photo picker integration
4. Photo compression (<500KB)
5. GPS auto-capture (dengan timeout, optional)
6. US-5.2: View Report Detail

**Deliverable:** Dapat create reports offline dengan photos dan GPS

---

### Sprint 5: Sync Engine (Week 9)
**Goal:** Robust sync dengan transaction log dan photo tracking

**Tasks:**
1. US-7.1: Offline create/edit (complete implementation)
2. US-7.2: Auto sync (full implementation)
   - Sync transaction log
   - Per-photo sync status
   - Retry logic dengan exponential backoff
   - Resume on app restart
   - Sync order (companies → contacts → projects → reports → attachments)
3. US-7.3: Manual sync button

**Deliverable:** Bulletproof sync dengan no data loss

---

### Sprint 6: Manager Features & Reports List (Week 10)
**Goal:** Manager dashboard dan reports viewing

**Tasks:**
1. US-6.1: View My Reports (sales rep, dengan pagination)
2. US-6.2: View Team Reports (manager, dengan RLS)
3. RLS policy testing (multi-user scenarios)
4. Filter/search implementation
5. Sync status indicators di UI

**Deliverable:** Manager dapat see semua team reports, reps see only their own

---

### Sprint 7: Polish & Testing (Week 11-12)
**Goal:** Production-ready quality

**Tasks:**
1. **Critical edge case testing:**
   - Partial sync failure recovery
   - App crash mid-sync recovery
   - Currency precision (100 edits test)
   - Token refresh on expiry
   - RLS policy enforcement
2. **Performance testing:**
   - Pagination dengan 100+ items
   - Photo compression speed
   - Sync speed (10 reports dengan photos)
3. **E2E testing:**
   - Complete user flow (create company → contact → project → report)
   - Offline → online sync flow
   - Manager viewing reports
4. **UI polish:**
   - Error messages user-friendly
   - Loading indicators consistent
   - Material Design 3 compliance
5. **Documentation:**
   - User guide untuk 2 sales reps
   - Training materials

**Deliverable:** Production-ready MVP untuk pilot launch

---

## ✅ Definition of Done (MVP)

### Per User Story
- [ ] Acceptance criteria met 100%
- [ ] Unit tests written (60%+ coverage untuk business logic)
- [ ] Integration tests untuk critical paths
- [ ] Code reviewed dan approved
- [ ] Works offline (jika applicable)
- [ ] Syncs ke Supabase correctly
- [ ] No memory leaks (tested dengan 1000 records)
- [ ] Error handling complete (network, validation, permissions)

### Per Sprint
- [ ] Semua stories di sprint completed
- [ ] Demo-able ke Product Owner
- [ ] No critical bugs
- [ ] Performance targets met

### MVP Selesai
- [ ] Semua 18 user stories delivered (77 points)
- [ ] Semua P0 edge cases tested (partial sync, crash recovery, currency precision, RLS)
- [ ] RLS policies tested dengan 2 sales reps + 1 manager
- [ ] Sync success rate >95% (tested dengan 100 entities)
- [ ] App crash rate <5% (tested dengan Sentry)
- [ ] Works offline → online → offline transitions
- [ ] User guide written
- [ ] Training session delivered
- [ ] Deployed ke production (Google Play internal testing or APK)

---

## 📦 DELIVERABLES CHECKLIST

**Apa Yang Akan Anda Hand Over ke Product Owner**

Checklist ini memastikan semua deliverables complete sebelum final payment/acceptance. Check off setiap item saat Anda menyelesaikannya.

### Deliverable Code

- [ ] **Complete Flutter Codebase**
  - Semua source code di folder `lib/` (organized by Clean Architecture layers)
  - Semua tests di folder `test/`
  - `pubspec.yaml` dengan semua dependencies listed
  - Tidak ada hardcoded credentials (semua di `.env`)
  - Tidak ada debug print statements di production code
  - Code mengikuti Flutter style guide (verified dengan `flutter analyze`)

- [ ] **Semua 18 User Stories Implemented**
  - [ ] US-1.1: Login with Email & Password
  - [ ] US-1.2: Logout from App
  - [ ] US-2.1: View List of Companies (dengan pagination)
  - [ ] US-2.2: Create New Company
  - [ ] US-2.3: Edit Company
  - [ ] US-3.1: View List of Contacts per Company (dengan pagination)
  - [ ] US-3.2: Create New Contact
  - [ ] US-3.3: Edit Contact
  - [ ] US-4.1: View List of Projects per Company (dengan pagination)
  - [ ] US-4.2: Create New Project (dengan INTEGER currency)
  - [ ] US-4.3: Edit Project (dengan value change logging)
  - [ ] US-5.1: Create Visit Report (dengan draft auto-save, photos, GPS)
  - [ ] US-5.2: View Report Detail
  - [ ] US-6.1: View My Reports (Sales Rep) dengan pagination
  - [ ] US-6.2: View Team Reports (Manager) dengan RLS
  - [ ] US-7.1: Offline Create/Edit
  - [ ] US-7.2: Auto Sync When Online (dengan transaction log, retry logic)
  - [ ] US-7.3: Manual Sync Button

- [ ] **Unit Tests untuk Repositories**
  - 60%+ code coverage untuk business logic (verified dengan `flutter test --coverage`)
  - Semua repository methods tested (create, read, update operations)
  - Mock objects digunakan untuk Supabase dan Drift (no real network calls di tests)
  - Coverage report included: `coverage/lcov.info`

- [ ] **Integration Tests untuk Critical Paths**
  - [ ] Complete report creation flow (offline → online sync)
  - [ ] Sync engine (create 20 entities offline → sync online → verify di Supabase)
  - [ ] RLS policy enforcement (2 sales reps cannot see each other's data)
  - [ ] Currency precision (edit project value 100 kali, no rounding errors)
  - [ ] Pagination (load 100+ companies, verify performance)
  - [ ] Photo upload (10 photos, verify compression <500KB, individual status tracking)

- [ ] **Signed Release APK**
  - `app-arm64-v8a-release.apk` (primary, untuk modern phones)
  - `app-armeabi-v7a-release.apk` (optional, untuk older phones)
  - APK signed dengan proper keystore (not debug key)
  - Version code dan name set di `pubspec.yaml` (e.g., version: 1.0.0+1)
  - App icon configured
  - App name: "CSS Sales Report"
  - Min SDK: 21 (Android 5.0+)

### Deliverable Dokumentasi

- [ ] **SETUP.md** (Environment Setup untuk New Developers)
  - Flutter SDK installation steps
  - Supabase project setup guide
  - Environment variables configuration
  - First build instructions
  - Troubleshooting common issues
  - Already provided di `production_requirements/SETUP.md`

- [ ] **DEPLOYMENT.md** (Cara Build dan Release APK)
  - Keystore generation instructions
  - Build commands untuk debug dan release
  - APK installation steps (adb commands)
  - Version numbering strategy
  - Google Play Store upload guide (jika applicable)

- [ ] **USER_GUIDE.md** (Untuk Sales Reps, dengan Screenshots)
  - Login instructions
  - Cara create company/contact/project
  - Cara create report (step-by-step dengan screenshots)
  - Cara view reports
  - Cara sync manually
  - Troubleshooting: "What if login fails?" "What if photo won't upload?"
  - FAQ section

- [ ] **TESTING.md** (Cara Run Tests, Test Accounts)
  - Cara run unit tests: `flutter test`
  - Cara run integration tests: `flutter test integration_test/`
  - Test account credentials (salesrep1@css.test, salesrep2@css.test, manager@css.test)
  - Cara reset test data
  - Cara generate coverage report

- [ ] **HANDOVER_NOTES.md** (Known Issues, Incomplete Items, Technical Debt)
  - List of known bugs (jika ada) dengan workarounds
  - Features yang belum implemented (deferred ke Phase 2)
  - Technical debt incurred (e.g., "Currency conversion helpers di utils/, should be moved ke domain layer")
  - Recommendations untuk Phase 2
  - Any assumptions made selama development
  - Performance bottlenecks (jika ada)

### Deliverable Infrastruktur

- [ ] **Supabase Project (Production-Ready)**
  - Semua 10 tables created (user_profiles, companies, contacts, projects, etc.)
  - Semua RLS policies enabled dan tested
  - Storage bucket `attachments` configured dengan policies
  - Test accounts created (2 sales reps, 1 manager)
  - Sample data seeded (20-30 companies, 50 contacts, 30 projects, 50 reports)
  - Database backups enabled (automatic di Supabase)
  - Product Owner memiliki admin access ke Supabase dashboard

- [ ] **Sentry Project (Crash Reporting Configured)**
  - Sentry project created: "css-sales-report"
  - DSN configured di `.env`
  - Test crash sent dan visible di Sentry dashboard
  - Product Owner added sebagai team member di Sentry
  - Alerts configured (email on critical errors)

- [ ] **3 Test Accounts dengan Sample Data**
  - **Sales Rep 1:** salesrep1@css.test / Test123!
    - 10 companies created
    - 20 contacts created
    - 10 projects created
    - 25 reports created (dengan photos dan GPS)
  - **Sales Rep 2:** salesrep2@css.test / Test123!
    - 10 companies created
    - 20 contacts created
    - 10 projects created
    - 25 reports created
  - **Manager:** manager@css.test / Test123!
    - Dapat view semua 50 reports dari both reps
    - Cannot create/edit (read-only)

### Transfer Pengetahuan

- [ ] **2-Hour Handover Session** (Live Video Call)
  - Walkthrough of codebase structure (folders `lib/`)
  - Explanation of Clean Architecture layers (presentation → domain → data)
  - Demo of BLoC state management pattern
  - Walkthrough of sync engine logic (`lib/core/sync/`)
  - Demo of running tests
  - Demo of building release APK
  - Q&A session (answer semua Product Owner questions)
  - Screen recording of session provided

- [ ] **Answer Clarification Questions** (Up to 5 Hours Post-Delivery)
  - Available untuk bug fixes (critical only) selama 2 minggu setelah handover
  - Available untuk clarification questions via email (response dalam 48 jam)
  - Total: Up to 5 jam post-delivery support included

### Kriteria Penerimaan (Definition of Done)

**Sebelum marking MVP as complete, semua yang berikut harus true:**

**Functional:**
- [ ] Semua 18 user stories work as described di acceptance criteria
- [ ] Login/logout works
- [ ] Create companies/contacts/projects works offline
- [ ] Edit companies/contacts/projects works offline
- [ ] Create reports dengan photos works offline (up to 10 photos)
- [ ] Draft auto-save saves setiap 30 detik
- [ ] Sync works: Go offline → create 20 entities → go online → all sync successfully
- [ ] Pagination works: View 100+ companies tanpa lag
- [ ] RLS works: Sales Rep A cannot see Sales Rep B's reports
- [ ] Manager dapat see semua reports dari semua sales reps

**Quality:**
- [ ] App crash rate <5% (tested dengan Sentry selama 1 minggu)
- [ ] Sync success rate >95% (checked di sync transaction logs)
- [ ] Currency precision: Edit project value 100 kali → no rounding errors
- [ ] Report creation time <5 menit (timed dengan stopwatch)
- [ ] Photo compression: Semua photos <500KB setelah compression
- [ ] No console errors atau warnings di release build
- [ ] `flutter analyze` shows 0 errors, 0 warnings
- [ ] Unit test coverage >60% (verified dengan coverage report)

**Performance:**
- [ ] List views load first 20 items di <200ms
- [ ] Sync 10 reports dengan photos completes di <5 detik (on good connection)
- [ ] App launches di <3 detik pada mid-range device
- [ ] APK size <50MB (arm64-v8a variant)

**Security:**
- [ ] No credentials hardcoded di source code
- [ ] `.env` file not committed ke Git
- [ ] RLS policies tested dengan 3 accounts simultaneously
- [ ] Service role key never exposed di app code (only anon key used)
- [ ] Semua API calls menggunakan HTTPS (Supabase enforces this)

**Documentation:**
- [ ] Semua 5 markdown documents complete dan up-to-date
- [ ] USER_GUIDE.md memiliki screenshots (minimum 10 screenshots)
- [ ] Tidak ada placeholder text seperti "TODO" atau "TBD" di docs

**Handover:**
- [ ] 2-hour handover session completed
- [ ] Product Owner dapat build APK from source (verified selama handover)
- [ ] Product Owner dapat run tests (verified selama handover)
- [ ] Product Owner memiliki admin access ke Supabase dan Sentry

---

## 🚀 Deployment Strategy

### Environment Setup
1. **Supabase Production Database**
   - Run migrations secara berurutan (001_create_tables.sql → 004_enable_rls_and_policies.sql)
   - Create 2 sales rep accounts + 1 manager account
   - Test RLS policies dengan semua 3 accounts

2. **Supabase Storage**
   - Create `attachments` bucket
   - Setup RLS policies untuk storage

3. **Sentry**
   - Create project untuk crash reporting
   - Configure DSN di app

### APK Build
```bash
# Build signed release APK
flutter build apk --release --split-per-abi

# Output: build/app/outputs/flutter-apk/app-armeabi-v7a-release.apk
```

### Pilot Launch (2 Sales Reps + 1 Manager)
**Week 1:**
- Training session (2 jam in person)
- Install APK di 3 devices
- Create 5 sample companies/contacts/projects bersama
- Create 1 test report bersama

**Week 2-4:**
- Daily check-ins (15 menit) - any blockers?
- Monitor Sentry untuk crashes
- Monitor Supabase metrics (sync success rate)
- Gather feedback (what's frustrating? what's working?)

**Week 5:**
- Success assessment:
  - Apakah mereka menggunakannya daily?
  - Apakah mereka prefer it dibanding WhatsApp?
  - Apakah manager visibility improved?
  - Ada data loss incidents?

---

## 📏 Success Metrics (MVP Pilot)

| Metric | Target | How to Measure |
|--------|--------|----------------|
| Daily active usage | 90%+ (2/2 reps use daily) | Login analytics, report creation count |
| Reports created per day | Avg 3-5 per rep | Database query |
| Sync success rate | >95% | Sync transaction log analysis |
| Data loss incidents | 0 | User reports + database audit |
| App crashes | <5% sessions | Sentry dashboard |
| User satisfaction | >4/5 rating | End-of-pilot survey |
| Manager time savings | 80% reduction (5h → 1h/week) | Manager survey |

---

## 🔧 Technical Debt & Known Limitations

### Limitasi yang Dapat Diterima untuk MVP
1. **No edit/delete untuk reports**
   - Workaround: Create new record jika mistake
   - Fixed di Phase 2

2. **No dashboard statistics**
   - Workaround: Manager counts reports manually
   - Fixed di Phase 2

3. **No user profile settings**
   - Workaround: Settings hard-coded
   - Fixed di Phase 2

4. **Last-Write-Wins conflict resolution**
   - Acceptable untuk 2 users (low conflict probability)
   - Advanced conflict resolution di Phase 2

5. **No export ke Excel/PDF**
   - Workaround: Manager views di app
   - Fixed di Phase 2

### Technical Debt yang Harus Ditangani di Phase 2
- Improve error messages (more context-specific)
- Add offline indicator di UI (more prominent)
- Optimize photo compression (parallel processing)
- Add batch operations (select multiple reports)
- Improve sync feedback (progress bar dengan details)

---

## 🎯 Next Steps Setelah MVP

1. **Collect Pilot Feedback** (Week 1-4 of pilot)
   - User interviews (what do they love? what's frustrating?)
   - Feature requests (what's missing?)
   - Bug reports (what's broken?)

2. **Analyze Usage Data** (Week 4)
   - Berapa banyak reports created?
   - Features mana yang paling digunakan?
   - Di mana users struggle? (analytics)

3. **Decide Phase 2 Scope** (Week 5)
   - Prioritize berdasarkan feedback + usage data
   - Plan next 6-8 minggu development

4. **Plan Scale to 5-10 Users** (jika successful)
   - Kapan expand?
   - Additional training needed?
   - Infrastructure capacity (Supabase limits?)

---

**Status Dokumen:** ✅ Complete - Ready untuk Development
**Terakhir Diperbarui:** 2025-10-28

---

## 📍 Navigasi

**Selesai membaca MVP Scope?**
- ✅ [Lanjut ke: User Stories →](./USER_STORIES.md)

**Atau kembali ke:**
- ← [README](./README.md)
