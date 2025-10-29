# Phase 2 - Enhancements & Scale

**Project:** CSS Sales Report Application
**Phase:** Phase 2 (Post-MVP Enhancements)
**Versi:** 1.1 (Revised - Edit Dipindahkan ke MVP)
**Last Updated:** 2025-10-28
**Target Duration:** 5-7 Minggu
**Total Story Points:** 29
**User Scale:** 5-10 Users

---

## 📋 Overview Phase 2

### Goals
- Tambahkan delete functionality (soft delete dengan cascade warnings)
- Tambahkan edit functionality untuk reports
- Scale dari 2 → 5-10 users
- Implement Priority 1 edge cases
- Tambahkan dashboard statistics untuk management
- Improve user experience berdasarkan pilot learnings

**Note:** Edit functionality untuk Companies, Contacts, dan Projects dipindahkan ke MVP (v1.1) untuk alasan data quality.

### Prerequisites
✅ MVP successfully deployed selama 30 hari
✅ 2 sales reps actively using daily
✅ Feedback collected dari pilot users
✅ Tidak ada critical bugs di MVP

---

## 🎯 Scope Phase 2

**CHANGE NOTE:** Edit functionality untuk Companies dan Contacts dipindahkan ke MVP. Phase 2 sekarang fokus pada delete functionality dan advanced features.

---

### EPIC 2: Companies - Delete (2 Story Points)

#### US-2.4: Delete Company (Soft Delete)
**Story Points:** 2
**Priority:** P2

**Included:**
- Confirmation dialog ("Delete XYZ Company?")
- Soft delete (set `deleted_at` timestamp)
- Cascade check (warn jika company memiliki projects/contacts)
- Sync ke Supabase
- Bekerja offline

**Cascade Warning:**
```dart
Future<bool> canDeleteCompany(String companyId) async {
  final projectCount = await db.countProjectsByCompany(companyId);
  final contactCount = await db.countContactsByCompany(companyId);

  if (projectCount > 0 || contactCount > 0) {
    return await showDialog<bool>(
      context: context,
      builder: (context) => AlertDialog(
        title: Text('Delete Company?'),
        content: Text(
          'This company has $projectCount projects and $contactCount contacts. '
          'They will also be deleted. Continue?',
        ),
        actions: [
          TextButton(onPressed: () => Navigator.pop(context, false), child: Text('Cancel')),
          TextButton(onPressed: () => Navigator.pop(context, true), child: Text('Delete')),
        ],
      ),
    ) ?? false;
  }

  return true;
}
```

---

### EPIC 3: Contacts - Delete (2 Story Points)

#### US-3.4: Delete Contact (Soft Delete)
**Story Points:** 2
**Priority:** P2

**Included:**
- Soft delete dengan confirmation
- Cascade check (warn jika contact adalah project primary atau report attendee)
- Bekerja offline

---

### EPIC 4: Projects - Delete (1 Story Point)

#### US-4.4: Delete Project (Soft Delete)
**Story Points:** 1
**Priority:** P2

**Included:**
- Soft delete dengan confirmation
- Cascade check (warn jika project memiliki reports)
- Bekerja offline

---

### EPIC 5: Reports - Edit & Delete (4 Story Points)

#### US-5.3: Edit Report
**Story Points:** 3
**Priority:** P1

**User Feedback Trigger:**
"Lupa menambahkan foto, harus buat report baru" → Perlu edit capability

**Included:**
- Edit semua fields (sama seperti create)
- Add/remove photos
- Update attendees
- Conflict resolution (Last-Write-Wins)
- Draft auto-save continues selama edit
- Bekerja offline

**Photo Management:**
```dart
// Remove photo
Future<void> removePhotoFromReport(String photoId) async {
  // 1. Soft delete locally
  await db.updateAttachment(photoId, deletedAt: DateTime.now());

  // 2. Queue delete untuk sync (delete dari Supabase Storage)
  await _syncQueue.enqueue(SyncOperation(
    entityType: 'attachment',
    entityId: photoId,
    operation: 'delete',
  ));
}

// Add photo ke existing report
Future<void> addPhotoToReport(String reportId, File photo) async {
  // Logic sama seperti create report
  final attachment = LocalAttachment(
    id: uuid.v4(),
    reportId: reportId,
    localPath: photo.path,
    syncStatus: 'pending',
  );

  await db.insertAttachment(attachment);
  await _syncQueue.enqueue(/* ... */);
}
```

---

#### US-5.4: Delete Report (Soft Delete)
**Story Points:** 1
**Priority:** P2

**Included:**
- Soft delete dengan confirmation
- Delete associated photos (soft delete)
- Bekerja offline

---

### EPIC 6: Dashboard Statistics (5 Story Points)

#### US-6.3: Simple Dashboard Statistics
**Story Points:** 5
**Priority:** P1

**User Feedback Trigger:**
Manager: "Saya ingin lihat trends: Berapa banyak visits bulan ini? Berapa banyak active projects?"

**Included:**
- **Untuk Sales Rep:**
  - My reports bulan ini (count)
  - My active projects (count + total value)
  - Recent activity (last 5 reports)

- **Untuk Manager:**
  - Team reports bulan ini (count, by rep)
  - Team active projects (count + total value)
  - Top performing rep (most visits)
  - Pipeline by status (Active, Won, Lost, On Hold)

**Technical Implementation:**
```dart
// Dashboard queries
class DashboardRepository {
  Future<DashboardStats> getMyStats(String userId) async {
    final now = DateTime.now();
    final monthStart = DateTime(now.year, now.month, 1);

    final reportsThisMonth = await db
      .countReports()
      .where((tbl) => tbl.createdBy.equals(userId))
      .where((tbl) => tbl.visitDate.isBiggerOrEqualValue(monthStart))
      .getSingle();

    final activeProjects = await db
      .customSelect('''
        SELECT COUNT(*) as count, SUM(estimated_value_cents) as total_value
        FROM projects
        WHERE created_by = ? AND status = 'Active' AND deleted_at IS NULL
      ''', variables: [Variable(userId)]).getSingle();

    return DashboardStats(
      reportsThisMonth: reportsThisMonth,
      activeProjectCount: activeProjects['count'],
      activeProjectValue: intToCurrency(activeProjects['total_value']),
    );
  }

  Future<ManagerDashboardStats> getTeamStats() async {
    // Queries similar aggregated across all users
    // ...
  }
}
```

**UI Implementation:**
- Cards dengan big numbers (Material Design 3)
- Charts: Bar chart untuk reports by rep, Pie chart untuk pipeline status
- Gunakan `fl_chart` package

---

### EPIC 7: Offline/Sync - Enhancements (2 Story Points)

#### US-7.4: Download Team Data (Manager)
**Story Points:** 2
**Priority:** P2

**User Feedback Trigger:**
Manager: "Saya ingin bekerja pada reports selama penerbangan (offline)"

**Included:**
- Manager dapat download semua team data ke local device
- Button: "Download All Team Reports"
- Progress indicator (X% downloaded)
- Periodic refresh (daily atau manual)

**Technical Implementation:**
```dart
Future<void> downloadAllTeamData() async {
  emit(DownloadingState(progress: 0.0));

  // 1. Fetch semua team reports
  final reports = await supabase
    .from('reports')
    .select('*, companies(*), projects(*)')
    .order('visit_date', ascending: false);

  emit(DownloadingState(progress: 0.3));

  // 2. Insert ke local DB
  for (var report in reports) {
    await db.insertOrUpdateReport(Report.fromJson(report));
  }

  emit(DownloadingState(progress: 0.6));

  // 3. Download photos untuk reports
  final photos = await supabase
    .from('attachments')
    .select()
    .in_('report_id', reports.map((r) => r['id']).toList());

  for (var photo in photos) {
    await downloadPhoto(photo);
  }

  emit(DownloadedState(reportCount: reports.length));
}
```

---

### EPIC 8: User Profile & Settings (5 Story Points)

#### US-8.1: View User Profile
**Story Points:** 2
**Priority:** P1

**Included:**
- Display user info (name, email, role)
- Show stats (total reports created, member since)
- Change password button

---

#### US-8.2: App Settings
**Story Points:** 3
**Priority:** P2

**Included:**
- Auto-sync toggle (enable/disable background sync)
- Photo quality setting (High/Medium/Low)
- Notification settings (sync success/failure)
- App version + build number
- Clear local cache button (dengan confirmation)

**Settings Persistence:**
```dart
// Gunakan SharedPreferences
class AppSettings {
  Future<void> setAutoSyncEnabled(bool enabled) async {
    await _prefs.setBool('auto_sync_enabled', enabled);
    if (enabled) {
      _syncService.startPeriodicSync();
    } else {
      _syncService.stopPeriodicSync();
    }
  }

  Future<void> setPhotoQuality(PhotoQuality quality) async {
    await _prefs.setString('photo_quality', quality.name);
  }
}
```

---

## 🔧 Priority 1 Edge Cases (Phase 2)

### Edge Case: GPS Permission Denied
**Priority:** P1
**Effort:** 1 Story Point

**MVP Saat Ini:** GPS auto-capture dengan timeout, tapi tidak ada permission handling UI

**Phase 2 Enhancement:**
- Detect jika GPS permission denied permanently
- Tampilkan user-friendly message: "Izin lokasi diperlukan untuk reports akurat. Aktifkan di Settings?"
- Button untuk buka app settings
- Allow proceeding tanpa GPS (optional)

**Implementation:**
```dart
Future<Position?> captureGPS() async {
  final permission = await Geolocator.checkPermission();

  if (permission == LocationPermission.deniedForever) {
    final openSettings = await showDialog<bool>(
      context: context,
      builder: (context) => AlertDialog(
        title: Text('Location Permission'),
        content: Text('Lokasi diperlukan untuk reports akurat. Buka settings?'),
        actions: [
          TextButton(onPressed: () => Navigator.pop(context, false), child: Text('Skip')),
          TextButton(onPressed: () => Navigator.pop(context, true), child: Text('Open Settings')),
        ],
      ),
    );

    if (openSettings == true) {
      await Geolocator.openAppSettings();
    }

    return null; // Proceed tanpa GPS
  }

  // ... rest of GPS capture logic
}
```

---

### Edge Case: Network Timeout Selama Sync
**Priority:** P1
**Effort:** 2 Story Points

**MVP Saat Ini:** Fixed timeout (30s), retry 3 kali

**Phase 2 Enhancement:**
- Adaptive timeout berdasarkan network type (WiFi: 30s, 4G: 60s, 3G: 90s)
- Show user: "Jaringan lambat terdeteksi, sync mungkin memerlukan waktu lebih lama"
- Allow user untuk cancel long-running sync

**Implementation:**
```dart
Future<Duration> getAdaptiveTimeout() async {
  final connectivityResult = await Connectivity().checkConnectivity();

  switch (connectivityResult) {
    case ConnectivityResult.wifi:
      return Duration(seconds: 30);
    case ConnectivityResult.mobile:
      final speed = await estimateNetworkSpeed(); // Ping test
      if (speed > 5) return Duration(seconds: 45); // 4G
      if (speed > 1) return Duration(seconds: 60); // 3G
      return Duration(seconds: 90); // 2G
    default:
      return Duration(seconds: 30);
  }
}
```

---

### Edge Case: User-Friendly Error Messages
**Priority:** P1
**Effort:** 2 Story Points

**MVP Saat Ini:** Technical error messages (contoh: "NetworkException: SocketException")

**Phase 2 Enhancement:**
- Map technical errors ke user-friendly messages
- Berikan actionable guidance

**Error Message Mapping:**
```dart
String getUserFriendlyError(Exception e) {
  if (e is SocketException) {
    return 'Tidak ada koneksi internet. Perubahan Anda disimpan dan akan sync saat online.';
  }
  if (e is TimeoutException) {
    return 'Jaringan lambat. Sync akan berlanjut di background.';
  }
  if (e is AuthException) {
    return 'Sesi Anda telah berakhir. Silakan login lagi.';
  }
  if (e is PostgrestException && e.code == '23505') {
    return 'Record ini sudah ada. Silakan refresh data Anda.';
  }
  // Default
  return 'Terjadi kesalahan. Silakan coba lagi atau hubungi support.';
}
```

---

## 📊 Ringkasan Phase 2

### Breakdown Story Points

| Epic | Stories | Points |
|------|---------|--------|
| EPIC 2: Companies Delete | 1 | 2 |
| EPIC 3: Contacts Delete | 1 | 2 |
| EPIC 4: Projects Delete | 1 | 1 |
| EPIC 5: Reports Edit/Delete | 2 | 4 |
| EPIC 6: Dashboard Statistics | 1 | 5 |
| EPIC 7: Download Team Data | 1 | 2 |
| EPIC 8: Profile & Settings | 2 | 5 |
| **Priority 1 Edge Cases** | 3 | 5 |
| **TOTAL** | **12** | **26** |

**REVISED:** US-2.3 (Edit Company) dan US-3.3 (Edit Contact) dipindahkan ke MVP.
**New Total:** 26 story points (rounded ke 29 untuk buffer = 5-7 minggu)

---

## 🏗️ Implementation Order (Sprint Planning)

**NOTE:** Edit functionality untuk Companies dan Contacts sekarang completed di MVP Sprint 2.

### Sprint 1: Report Edit & All Delete Functionality (Week 1-2)
- US-5.3: Edit Report (dengan photo add/remove)
- US-5.4: Delete Report (soft delete)
- US-2.4: Delete Company (dengan cascade warnings)
- US-3.4: Delete Contact (dengan cascade warnings)
- US-4.4: Delete Project (dengan cascade warnings)
- Cascade warning dialogs implementation
- Testing dengan multiple users

### Sprint 2: Dashboard & Analytics (Week 3-4)
- US-6.3: Dashboard Statistics
- Chart implementation (fl_chart)
- Performance optimization (aggregate queries)
- Manager dashboard

### Sprint 3: Profile & Settings (Week 5)
- US-8.1: User Profile
- US-8.2: App Settings
- US-7.4: Download Team Data (manager)

### Sprint 4: Priority 1 Edge Cases (Week 6)
- GPS permission handling
- Adaptive network timeouts
- User-friendly error messages

### Sprint 5: Testing & Polish (Week 7)
- E2E testing dengan 5-10 users
- Performance testing dengan larger datasets
- UI polish berdasarkan feedback
- Documentation updates

---

## 🎯 Success Criteria (Phase 2)

| Metrik | Target |
|--------|--------|
| User count | 5-10 active users |
| Feature adoption | >80% users menggunakan edit functionality |
| Dashboard usage | Manager checks dashboard daily |
| User satisfaction | >4.5/5 rating |
| Tidak ada critical bugs | 0 data loss incidents |
| Performance | <1s response time untuk dashboard queries |

---

## 🚀 Strategi Deployment

### Gradual Rollout
1. **Week 1:** Deploy ke 2 pilot users dulu (test edit/delete)
2. **Week 2:** Jika stable, deploy ke 3 new users
3. **Week 3:** Deploy ke remaining users (total 5-10)

### Training
- 30-minute session untuk new features (edit/delete, dashboard)
- Video tutorials untuk async learning

---

**Status Dokumen:** ✅ Complete - Siap untuk Planning
**Last Updated:** 2025-10-28
**Next Review:** Setelah MVP completion (Week 12)
# Phase 3 - Scale & Advanced Features

**Project:** CSS Sales Report Application
**Phase:** Phase 3 (Production Scale)
**Versi:** 1.0
**Last Updated:** 2025-10-28
**Target Duration:** 8-12 Minggu
**User Scale:** 10-50+ Users
**Status:** Future Planning (Execute Setelah Phase 2 Success)

---

## 📋 Overview Phase 3

### Goals
- Scale dari 10 → 50+ users
- Tambahkan advanced features berdasarkan Phase 2 learnings
- Improve performance untuk large datasets
- Tambahkan export & reporting capabilities
- iOS version (jika ada demand)
- Advanced analytics untuk management

### Prerequisites
✅ Phase 2 deployed dan stable selama 60 hari
✅ 5-10 users actively using daily
✅ Feedback mengindikasikan kebutuhan untuk advanced features
✅ Business case validated (ROI positive)
✅ Budget allocated untuk Phase 3

---

## 🎯 Scope Phase 3

### 1. Export & Reporting (8 Story Points)

#### Feature: Export Reports ke Excel/PDF
**Story Points:** 5
**Priority:** P1

**User Feedback Trigger:**
Manager: "Saya perlu present reports di monthly management meetings. Perlu Excel/PDF export."

**Included:**
- Export single report ke PDF (formatted, dengan photos)
- Export multiple reports ke Excel (tabular, dengan filters)
- Export pipeline summary ke Excel
- Share via email/WhatsApp dari app

**Technical Implementation:**
```dart
// PDF generation (package: pdf)
Future<File> exportReportToPDF(Report report) async {
  final pdf = pw.Document();

  pdf.addPage(
    pw.Page(
      build: (pw.Context context) {
        return pw.Column(
          crossAxisAlignment: pw.CrossAxisAlignment.start,
          children: [
            pw.Header(level: 0, child: pw.Text('Visit Report')),
            pw.Text('Company: ${report.company.name}'),
            pw.Text('Project: ${report.project.name}'),
            pw.Text('Visit Date: ${formatDate(report.visitDate)}'),
            pw.Text('Type: ${report.reportType}'),
            pw.SizedBox(height: 20),
            pw.Header(level: 1, child: pw.Text('Notes')),
            pw.Text(report.notes ?? 'N/A'),
            // ... photos, attendees, etc.
          ],
        );
      },
    ),
  );

  final output = await getTemporaryDirectory();
  final file = File('${output.path}/report_${report.id}.pdf');
  await file.writeAsBytes(await pdf.save());

  return file;
}

// Excel generation (package: excel)
Future<File> exportReportsToExcel(List<Report> reports) async {
  var excel = Excel.createExcel();
  Sheet sheet = excel['Reports'];

  // Headers
  sheet.appendRow(['Visit Date', 'Company', 'Project', 'Type', 'Attendees', 'Outcome', 'Notes']);

  // Data rows
  for (var report in reports) {
    sheet.appendRow([
      formatDate(report.visitDate),
      report.company.name,
      report.project.name,
      report.reportType,
      report.attendees.map((a) => a.name).join(', '),
      report.outcome ?? '',
      report.notes ?? '',
    ]);
  }

  final output = await getTemporaryDirectory();
  final file = File('${output.path}/reports_export.xlsx');
  await file.writeAsBytes(excel.encode()!);

  return file;
}
```

---

#### Feature: Scheduled Email Reports (Manager)
**Story Points:** 3
**Priority:** P2

**Included:**
- Manager subscribe ke weekly email summary
- Email berisi: Total reports minggu ini, by rep, top projects
- Automated email via Supabase Edge Functions

**Technical Implementation:**
```typescript
// Supabase Edge Function (Deno)
import { serve } from 'https://deno.land/std@0.168.0/http/server.ts'

serve(async (req) => {
  // Runs setiap Senin 9am (via pg_cron)
  const managers = await supabase
    .from('users')
    .select('email')
    .eq('role', 'manager')
    .eq('email_reports_enabled', true)

  for (const manager of managers) {
    const weeklyStats = await getWeeklyStats(manager.id)
    await sendEmail(manager.email, weeklyStats)
  }

  return new Response('OK', { status: 200 })
})
```

---

### 2. Advanced Conflict Resolution (5 Story Points)

#### Feature: User-Prompted Conflict Resolution
**Story Points:** 5
**Priority:** P1

**Phase 2 Saat Ini:** Last-Write-Wins (automatic)

**Phase 3 Enhancement:**
Ketika sync detect conflict, tunjukkan user kedua versions dan biarkan mereka pilih:

**UI Flow:**
```
┌─────────────────────────────────────┐
│   Conflict Detected                 │
├─────────────────────────────────────┤
│ Project "ABC Corp Deal" was edited │
│ by you and another user.            │
│                                     │
│ Your Version:                       │
│ - Value: Rp 60,000,000              │
│ - Status: Active                    │
│ - Updated: 2025-10-28 14:30         │
│                                     │
│ Server Version:                     │
│ - Value: Rp 55,000,000              │
│ - Status: On Hold                   │
│ - Updated: 2025-10-28 14:32         │
│                                     │
│ [ Keep Mine ] [ Use Server ] [ Merge ]
└─────────────────────────────────────┘
```

**Technical Implementation:**
```dart
Future<void> resolveConflict(String entityId, EntityType type) async {
  final local = await db.getEntity(entityId, type);
  final remote = await supabase.from(type.tableName).select().eq('id', entityId).single();

  final resolution = await showConflictDialog(local, remote);

  switch (resolution.action) {
    case ConflictAction.keepLocal:
      await supabase.from(type.tableName).update(local.toJson()).eq('id', entityId);
      break;
    case ConflictAction.useServer:
      await db.updateEntity(Entity.fromJson(remote));
      break;
    case ConflictAction.merge:
      final merged = await showMergeDialog(local, remote);
      await db.updateEntity(merged);
      await supabase.from(type.tableName).update(merged.toJson()).eq('id', entityId);
      break;
  }
}
```

---

### 3. Performance Optimization untuk Scale (8 Story Points)

#### Feature: Database Query Optimization
**Story Points:** 3
**Priority:** P0 (Critical untuk 50+ users)

**Issue Saat Ini:** N+1 query problem saat loading reports dengan companies/projects

**Phase 3 Optimization:**
```dart
// BEFORE (N+1 queries)
for (var report in reports) {
  report.company = await db.getCompany(report.companyId); // N queries
  report.project = await db.getProject(report.projectId); // N queries
}

// AFTER (1 query dengan JOIN)
final reportsWithRelations = await db
  .customSelect('''
    SELECT
      r.*,
      c.name as company_name,
      p.name as project_name,
      p.estimated_value_cents
    FROM reports r
    JOIN companies c ON r.company_id = c.id
    JOIN projects p ON r.project_id = p.id
    WHERE r.deleted_at IS NULL
    ORDER BY r.visit_date DESC
    LIMIT 20 OFFSET ?
  ''', variables: [Variable(page * 20)])
  .get();
```

---

#### Feature: Photo Lazy Loading & Caching
**Story Points:** 3
**Priority:** P1

**Issue Saat Ini:** Photos loaded semuanya sekaligus, scrolling lambat dengan 100+ reports

**Phase 3 Enhancement:**
- Lazy load photos hanya saat visible (IntersectionObserver)
- Cache photos di LRU cache (max 100MB)
- Thumbnail generation di server (Supabase Image Transformation)

**Technical Implementation:**
```dart
// Gunakan cached_network_image package
CachedNetworkImage(
  imageUrl: supabase.storage.from('attachments').getPublicUrl(
    photoPath,
    transform: TransformOptions(width: 400, height: 300), // Thumbnail
  ),
  memCacheWidth: 400,
  memCacheHeight: 300,
  placeholder: (context, url) => CircularProgressIndicator(),
  errorWidget: (context, url, error) => Icon(Icons.error),
)
```

---

#### Feature: Background Sync Optimization
**Story Points:** 2
**Priority:** P1

**Issue Saat Ini:** Sync all entities setiap 5 menit (wasteful jika tidak ada changes)

**Phase 3 Enhancement:**
- Delta sync (hanya entities changed sejak last sync)
- Incremental sync (batch size: 20 entities at a time)
- Sync priority (reports > projects > companies)

**Technical Implementation:**
```dart
Future<void> performDeltaSync() async {
  final lastSyncTime = await getLastSyncTime();

  // Hanya sync entities modified setelah last sync
  final modifiedReports = await db
    .select(db.reports)
    .where((tbl) => tbl.updatedAt.isBiggerThanValue(lastSyncTime))
    .get();

  // Batch upload (20 at a time)
  for (var i = 0; i < modifiedReports.length; i += 20) {
    final batch = modifiedReports.skip(i).take(20);
    await supabase.from('reports').upsert(batch.map((r) => r.toJson()).toList());
  }

  await saveLastSyncTime(DateTime.now());
}
```

---

### 4. iOS Version (13 Story Points)

#### Feature: iOS App (Flutter)
**Story Points:** 13
**Priority:** P2 (Jika ada demand)

**Trigger:** Sales team expands, beberapa reps memiliki iPhones

**Included:**
- iOS app dengan features sama seperti Android MVP
- iOS-specific UI adjustments (Cupertino widgets)
- iOS permissions (Photos, Location)
- TestFlight distribution

**Technical Considerations:**
- Drift works di iOS (SQLite built-in)
- Supabase SDK supports iOS
- Photo picker: image_picker supports iOS
- GPS: geolocator supports iOS

**Additional Work:**
- iOS app store listing (jika public)
- iOS code signing & certificates
- iOS-specific testing

---

### 5. Advanced Analytics & Business Intelligence (8 Story Points)

#### Feature: Manager Analytics Dashboard
**Story Points:** 5
**Priority:** P1

**Included:**
- Monthly trends (reports created, projects won)
- Sales funnel visualization (Initial Visit → Quotation → Closing → Won)
- Rep performance comparison (reports, win rate, avg deal size)
- Geographic heatmap (di mana most visits?)
- Time analysis (avg time dari first visit ke closing)

**Technical Implementation:**
```dart
// Analytics queries
class AnalyticsRepository {
  Future<SalesFunnelData> getSalesFunnel(String userId, DateRange range) async {
    final result = await db.customSelect('''
      SELECT
        report_type,
        COUNT(*) as count
      FROM reports
      WHERE created_by = ?
        AND visit_date BETWEEN ? AND ?
        AND deleted_at IS NULL
      GROUP BY report_type
      ORDER BY
        CASE report_type
          WHEN 'Initial Visit' THEN 1
          WHEN 'Follow-up Meeting' THEN 2
          WHEN 'Technical Presentation' THEN 3
          WHEN 'Price Quotation' THEN 4
          WHEN 'Closing Visit' THEN 5
          ELSE 6
        END
    ''', variables: [
      Variable(userId),
      Variable(range.start),
      Variable(range.end),
    ]).get();

    return SalesFunnelData.fromQueryResult(result);
  }
}
```

**UI Visualization:**
- Gunakan `fl_chart` untuk advanced charts (funnel, heatmap)
- Interactive filters (date range, rep selection)
- Export analytics ke PDF

---

#### Feature: Predictive Analytics (AI/ML)
**Story Points:** 3
**Priority:** P3 (Future)

**Included:**
- Win probability prediction (berdasarkan report frequency, outcome trends)
- Expected close date prediction (ML model trained di historical data)
- Rep performance scoring

**Technical Approach:**
- Train TensorFlow Lite model di historical data
- Features: report count, days since first visit, outcome sentiment, deal size
- On-device inference (tidak perlu server)

---

### 6. Integrations (8 Story Points)

#### Feature: WhatsApp Integration (Send Reports)
**Story Points:** 3
**Priority:** P2

**User Feedback Trigger:**
Sales rep: "Saya masih perlu kirim reports ke clients via WhatsApp, bisa dari app?"

**Included:**
- "Share Report" button → Opens WhatsApp dengan pre-formatted message + PDF
- Template: "Dear [Client Name], Attached is our visit report from [Date]. [Summary]"

**Technical Implementation:**
```dart
Future<void> shareReportViaWhatsApp(Report report) async {
  // 1. Generate PDF
  final pdfFile = await exportReportToPDF(report);

  // 2. Share via WhatsApp
  await Share.shareXFiles(
    [XFile(pdfFile.path)],
    text: '''
Dear ${report.company.name},

Attached is our visit report from ${formatDate(report.visitDate)}.

Summary:
- Type: ${report.reportType}
- Outcome: ${report.outcome}
- Next Action: ${report.nextAction}

Best regards,
${currentUser.name}
    '''.trim(),
  );
}
```

---

#### Feature: Email Integration (Send Reports)
**Story Points:** 2
**Priority:** P2

**Included:**
- "Email Report" button → Opens email client dengan PDF attachment
- Pre-filled subject & body

---

#### Feature: CRM Integration (Salesforce, HubSpot)
**Story Points:** 3
**Priority:** P3 (Jika enterprise customers)

**Included:**
- Sync companies/contacts ke CRM
- Sync projects sebagai "Opportunities"
- Sync reports sebagai "Activities"

---

### 7. Advanced Security & Compliance (5 Story Points)

#### Feature: Audit Trail Viewer
**Story Points:** 2
**Priority:** P1

**Included:**
- Manager dapat view audit trail untuk any entity (siapa mengubah apa, kapan)
- Berguna untuk compliance & dispute resolution

**UI:**
```
Project: ABC Corp Deal - Change History
┌─────────────────────────────────────────────────┐
│ 2025-10-28 14:30 - John Doe                     │
│   Value: Rp 50,000,000 → Rp 60,000,000         │
│   Status: Active → On Hold                      │
├─────────────────────────────────────────────────┤
│ 2025-10-25 10:15 - Jane Smith                  │
│   Created project                                │
│   Initial value: Rp 50,000,000                  │
└─────────────────────────────────────────────────┘
```

---

#### Feature: Data Encryption at Rest
**Story Points:** 2
**Priority:** P1 (Jika handling sensitive data)

**Included:**
- Encrypt local SQLite database (sqlcipher)
- Encrypted key disimpan di secure storage

---

#### Feature: Two-Factor Authentication (2FA)
**Story Points:** 1
**Priority:** P2

**Included:**
- Optional 2FA via SMS atau authenticator app
- Supabase Auth supports 2FA natively

---

### 8. User Experience Enhancements (5 Story Points)

#### Feature: Voice-to-Text untuk Notes
**Story Points:** 2
**Priority:** P2

**User Feedback Trigger:**
Sales rep: "Mengetik notes panjang lambat, bisa dictate?"

**Included:**
- Microphone button di Notes field
- Speech-to-text menggunakan Google Speech API
- Supports Indonesian + English

---

#### Feature: Offline Maps untuk GPS
**Story Points:** 2
**Priority:** P2

**Included:**
- Download map tiles untuk offline viewing
- Show visit locations di offline map

---

#### Feature: Dark Mode
**Story Points:** 1
**Priority:** P3

**Included:**
- System theme detection
- Manual toggle di settings

---

## 📊 Ringkasan Phase 3

### Breakdown Story Points

| Feature Category | Story Points |
|------------------|--------------|
| Export & Reporting | 8 |
| Advanced Conflict Resolution | 5 |
| Performance Optimization | 8 |
| iOS Version | 13 |
| Advanced Analytics | 8 |
| Integrations | 8 |
| Security & Compliance | 5 |
| UX Enhancements | 5 |
| **TOTAL** | **60** |

---

## 🏗️ Strategi Implementation

### Option A: Incremental (Recommended)
Tambahkan features incrementally berdasarkan user demand:
- Bulan 1-2: Export & Reporting + Performance Optimization
- Bulan 3-4: iOS Version (jika needed)
- Bulan 5-6: Advanced Analytics
- Ongoing: Integrations & UX enhancements as needed

### Option B: Parallel Teams
Jika budget allows, jalankan 2 parallel teams:
- Team 1: Android enhancements (export, analytics, integrations)
- Team 2: iOS version

---

## 🎯 Success Criteria (Phase 3)

| Metrik | Target |
|--------|--------|
| User count | 50+ active users |
| Scalability | <2s page load dengan 10,000 reports |
| Feature adoption | >60% users export reports monthly |
| Manager satisfaction | >4.5/5 untuk analytics dashboard |
| iOS adoption | 20%+ dari tim uses iOS app (jika launched) |

---

## 💰 Pertimbangan Cost

### Infrastructure Scale (50+ Users)
- **Supabase:** ~$25-50/bulan (Database + Storage + Auth)
- **Sentry:** ~$26/bulan (Team plan untuk 50k events)
- **Total:** ~$50-100/bulan hosting

### Development Cost (Estimate)
- 60 story points × $1,500/point = ~$90,000 USD (~Rp 1,350,000,000)
- Atau 6-8 bulan in-house developer

---

## 🚦 Go/No-Go Decision Points

**Proceed ke Phase 3 JIKA:**
✅ Phase 2 deployed successfully selama 60+ hari
✅ 10+ active users dengan high satisfaction (>4/5)
✅ Clear demand untuk advanced features (export, analytics, iOS)
✅ Positive ROI dari Phase 1+2
✅ Budget allocated

**Postpone Phase 3 JIKA:**
❌ User count plateaued di <10 users
❌ Low feature adoption di Phase 2
❌ High churn rate (users stop using app)
❌ Negative atau neutral ROI

---

**Status Dokumen:** ✅ Complete - Future Planning
**Last Updated:** 2025-10-28
**Next Review:** Setelah Phase 2 completion (6-8 bulan)
