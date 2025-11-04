# Spesifikasi Fungsional
## CSS Sales Report - User Stories & Requirements

← [Sebelumnya: MVP Scope](./MVP_SCOPE.md)

---

**Versi:** 3.0 (Production Ready)
**Terakhir Diperbarui:** Oktober 2025
**Total Story Points:** 106
**Total User Stories:** 22 (across 8 epics)

---

## 1. Pendahuluan

### 1.1 Tujuan

Dokumen ini menjelaskan functional requirements untuk aplikasi mobile CSS Sales Report. Dokumen ini mengikuti standar IEEE 830 untuk Software Requirements Specifications dan menyediakan user stories yang detail dengan acceptance criteria untuk implementasi.

### 1.2 Lingkup

Aplikasi ini memungkinkan sales representatives PT Cepat Service Station untuk membuat laporan kunjungan terstruktur secara offline dan menyinkronkannya ke database pusat. Manager dapat melihat aktivitas tim secara real-time.

**Dalam Lingkup:** 22 user stories across 8 functional epics
**Di Luar Lingkup:** Lihat PRD.md Section "What's Excluded"

### 1.3 Definisi

| Istilah | Definisi |
|---------|----------|
| Sales Rep | Sales representative lapangan yang membuat laporan |
| Manager | Team leader dengan akses read-only ke semua data tim |
| Offline-First | Data disimpan ke local terlebih dahulu, di-sync ke server saat online |
| Soft Delete | Record ditandai sebagai dihapus (`deleted_at` di-set) tetapi tidak dihapus secara fisik |
| RLS | Row-Level Security - kontrol akses di level database |
| Draft | Laporan yang belum selesai, auto-saved secara lokal |

### 1.4 Referensi

- PRD.md - Product Requirements Document
- Architecture.md - System design dan technical architecture
- Technical_Spec.md - Non-functional requirements dan edge cases
- database/Drift_Schema.md - Local database schema
- database/Supabase_Schema.md - Cloud database schema

---

## 2. Deskripsi Umum

### 2.1 User Classes

**Primary User: Sales Representative**
- Membuat laporan kunjungan di lapangan
- Mengelola companies, contacts, projects
- Bekerja terutama secara offline
- Membutuhkan UI yang sederhana dan cepat
- Jumlah: 2 users (MVP), berkembang ke 5-10

**Secondary User: Manager**
- Melihat semua laporan tim (read-only)
- Memantau pipeline dan performance
- Selalu online
- Membutuhkan dashboard dan filters
- Jumlah: 1 user

### 2.2 Lingkungan Operasi

- Platform: Android mobile (minimum SDK 21 / Android 5.0)
- Network: Bekerja offline, sync saat online
- Storage: ~50-100MB local database + photos
- Connectivity: 2G/3G/4G/WiFi (adaptive)

### 2.3 Design and Implementation Constraints

- Harus bekerja offline (core requirement)
- Data tidak boleh hilang (transaction log diperlukan)
- Nilai currency harus exact (penyimpanan INTEGER, bukan floating-point)
- Photos di-compress ke <500KB masing-masing
- Pagination wajib untuk lists (performance requirement)
- RLS policies menegakkan isolasi data

---

## 3. Functional Requirements

Bagian ini menjelaskan semua 22 user stories yang diorganisir dalam 8 epics.

---

### EPIC 1: Authentication (5 Story Points)

#### US-1.1: Login dengan Email & Password
**Story Points:** 3
**Priority:** P0 (Critical)

**User Story:**
Sebagai sales rep atau manager
Saya ingin login menggunakan email dan password
Sehingga saya dapat mengakses app secara aman

**Acceptance Criteria:**
1. Login screen menampilkan input fields untuk email dan password
2. Validasi email menegakkan format yang valid (example@domain.com)
3. Password harus minimal 8 karakter
4. Tersedia tombol toggle show/hide password
5. Error messages yang jelas untuk kredensial yang tidak valid
6. Loading indicator ditampilkan selama authentication
7. Auto-redirect ke home screen setelah login sukses
8. Session tetap aktif (tidak perlu re-login saat app restart)

**Technical Notes:**
- Gunakan Supabase Auth untuk authentication
- Simpan session token di flutter_secure_storage
- Handle network errors dengan baik dengan opsi retry
- Token expires setelah 7 hari, auto-refresh sebelum expiry

**Dependencies:** None (fitur pertama untuk diimplementasi)

---

#### US-1.2: Logout dari App
**Story Points:** 2
**Priority:** P1 (High)

**User Story:**
Sebagai user
Saya ingin logout dari app
Sehingga akun saya aman jika orang lain menggunakan phone saya

**Acceptance Criteria:**
1. Tombol logout terlihat di profile/settings screen
2. Confirmation dialog ditampilkan sebelum logout
3. Session token dihapus setelah logout
4. User di-redirect ke login screen
5. Local database (SQLite) TIDAK dihapus (mempertahankan data offline)

**Technical Notes:**
- Panggil `supabase.auth.signOut()`
- Hapus flutter_secure_storage
- Pertahankan Drift database
- Logout harus bekerja offline

**Dependencies:** US-1.1 (Login)

---

### EPIC 2: Manage Companies (12 Story Points)

#### US-2.1: Lihat List Companies
**Story Points:** 3
**Priority:** P0 (Critical)

**User Story:**
Sebagai sales rep
Saya ingin melihat list semua companies yang telah saya buat
Sehingga saya dapat mengelola customers saya

**Acceptance Criteria:**
1. Tampilkan list companies yang dibuat oleh current user
2. Tampilkan nama company untuk setiap entry
3. Fungsi search berdasarkan nama company
4. Opsi sort: Most Recent, Name (A-Z)
5. Empty state message jika tidak ada companies
6. Pull-to-refresh memicu background sync
7. Manager dapat melihat SEMUA companies dari semua sales reps
8. **Pagination:** Load 20 companies per page (MVP performance requirement)

**Technical Notes:**
- Query dari Drift (SQLite) terlebih dahulu (offline-first)
- Background sync ke Supabase
- Filter berdasarkan `created_by = current_user.id` (kecuali managers)
- Gunakan indexed query untuk performance

**Dependencies:** US-1.1 (Login diperlukan)

---

#### US-2.2: Buat Company Baru
**Story Points:** 5
**Priority:** P0 (Critical)

**User Story:**
Sebagai sales rep
Saya ingin menambahkan company baru
Sehingga saya dapat melacak customers baru

**Acceptance Criteria:**
1. Form dengan required field: Company Name
2. Validasi: Nama company tidak boleh kosong
3. Validasi: Nama company tidak boleh duplicate (case-insensitive)
4. Simpan ke SQLite segera (dukungan offline)
5. Auto-sync ke Supabase saat online
6. Success toast message ditampilkan
7. Redirect ke company detail screen setelah pembuatan

**Technical Notes:**
- Cek duplicate di local DB terlebih dahulu, kemudian server saat sync
- Set `created_by = current_user.id`
- Generate UUID di client-side
- Handle sync conflict jika duplicate ditemukan di server

**Dependencies:** US-2.1 (View list)

---

#### US-2.3: Edit Company
**Story Points:** 2
**Priority:** P1 (High)

**User Story:**
Sebagai sales rep
Saya ingin mengedit nama company
Sehingga saya dapat memperbaiki typos atau memperbarui nama resmi

**Acceptance Criteria:**
1. Edit field nama company
2. Validasi yang sama dengan create (tidak duplicate, tidak kosong)
3. Simpan ke SQLite dan trigger sync
4. Success toast message
5. User hanya dapat mengedit companies yang mereka buat

**Technical Notes:**
- Cek permission `created_by`
- Update timestamp `updated_at` secara otomatis
- Conflict resolution via Last-Write-Wins (MVP)

**Dependencies:** US-2.2 (Create company)

---

#### US-2.4: Hapus Company (Soft Delete)
**Story Points:** 2
**Priority:** P2 (Medium)

**User Story:**
Sebagai sales rep
Saya ingin menghapus company yang dibuat secara salah
Sehingga list company saya tetap bersih

**Acceptance Criteria:**
1. Confirmation dialog sebelum delete
2. Soft delete (set timestamp `deleted_at`)
3. Company tidak lagi muncul di list
4. User hanya dapat menghapus companies yang mereka buat
5. Tidak dapat menghapus company dengan active projects (validation error ditampilkan)

**Technical Notes:**
- Soft delete: `deleted_at = NOW()`
- Validasi tidak ada active projects: `SELECT COUNT(*) FROM projects WHERE company_id = ? AND deleted_at IS NULL`
- Database constraint: ON DELETE RESTRICT

**Dependencies:** US-2.2 (Create company)

---

### EPIC 3: Manage Contacts (12 Story Points)

#### US-3.1: Lihat List Contacts per Company
**Story Points:** 3
**Priority:** P0 (Critical)

**User Story:**
Sebagai sales rep
Saya ingin melihat semua contact persons untuk sebuah company
Sehingga saya tahu siapa yang harus dihubungi

**Acceptance Criteria:**
1. List contacts yang difilter berdasarkan company_id
2. Tampilkan: Contact Name, Position, Phone
3. Sort berdasarkan: Most Recent, Name (A-Z)
4. Empty state jika tidak ada contacts
5. Search berdasarkan nama contact atau phone
6. Badge/indicator untuk inactive contacts (`is_active = false`)
7. **Pagination:** Load 20 contacts per page

**Technical Notes:**
- Query: `SELECT * FROM contacts WHERE company_id = ? AND deleted_at IS NULL`
- Offline-first (Drift query)

**Dependencies:** US-2.1 (View companies)

---

#### US-3.2: Buat Contact Baru
**Story Points:** 5
**Priority:** P0 (Critical)

**User Story:**
Sebagai sales rep
Saya ingin menambahkan contact person baru ke sebuah company
Sehingga saya dapat melacak project stakeholders

**Acceptance Criteria:**
1. Form fields:
   - Contact Name (required)
   - Position (required)
   - Phone (required, format Indonesia valid)
   - Email (optional, validasi format jika diisi)
   - Is Active (default: true, checkbox)
2. Validasi phone: format 08xx atau +62xxx
3. Simpan ke SQLite dan auto-sync
4. Success toast message
5. `company_id` secara otomatis di-link dari context

**Technical Notes:**
- Set `created_by = current_user.id`
- Validasi phone regex: `^(08[0-9]{8,11}|\\+62[0-9]{9,12})$`
- Validasi email: standard email regex jika diisi

**Dependencies:** US-2.2 (Create company)

---

#### US-3.3: Edit Contact
**Story Points:** 2
**Priority:** P1 (High)

**User Story:**
Sebagai sales rep
Saya ingin mengedit informasi contact
Sehingga data contact tetap up-to-date

**Acceptance Criteria:**
1. Edit semua fields (name, position, phone, email, is_active)
2. Validasi yang sama dengan create
3. Dapat menandai contact sebagai inactive (`is_active = false`) jika resign/pindah
4. Simpan dan sync
5. User hanya dapat mengedit contacts yang mereka buat

**Technical Notes:**
- Cek permission `created_by`
- Update timestamp `updated_at`

**Dependencies:** US-3.2 (Create contact)

---

#### US-3.4: Hapus Contact (Soft Delete)
**Story Points:** 2
**Priority:** P2 (Medium)

**User Story:**
Sebagai sales rep
Saya ingin menghapus contact yang diinput secara salah
Sehingga list contact saya tetap bersih

**Acceptance Criteria:**
1. Confirmation dialog sebelum delete
2. Soft delete (set `deleted_at`)
3. Contact tidak lagi muncul di list
4. User hanya dapat menghapus contacts yang mereka buat
5. Tidak dapat menghapus contact jika mereka adalah primary contact di active project (validation error)

**Technical Notes:**
- Validasi: `NOT IN (SELECT primary_contact_id FROM projects WHERE deleted_at IS NULL)`

**Dependencies:** US-3.2 (Create contact)

---

### EPIC 4: Manage Projects (15 Story Points)

#### US-4.1: Lihat List Projects per Company
**Story Points:** 3
**Priority:** P0 (Critical)

**User Story:**
Sebagai sales rep
Saya ingin melihat semua projects untuk sebuah company
Sehingga saya dapat melacak sales pipeline saya

**Acceptance Criteria:**
1. List projects yang difilter berdasarkan company_id
2. Tampilkan: Project Name, Type, Status, Estimated Value
3. Sort berdasarkan: Most Recent, Expected Close Date, Value (Highest)
4. Filter berdasarkan: Status (Active, Won, Lost, On Hold), Project Type
5. Empty state jika tidak ada projects
6. Badge berwarna untuk status
7. **Pagination:** Load 20 projects per page

**Technical Notes:**
- Format estimated value: `Rp 50.000.000` (thousand separator)
- Warna status: Active (blue), Won (green), Lost (red), On Hold (gray)
- Value disimpan sebagai INTEGER cents, ditampilkan sebagai currency

**Dependencies:** US-2.1 (View companies)

---

#### US-4.2: Buat Project Baru
**Story Points:** 8
**Priority:** P0 (Critical)

**User Story:**
Sebagai sales rep
Saya ingin membuat project baru untuk sebuah company
Sehingga saya dapat melacak sales opportunities

**Acceptance Criteria:**
1. Form fields:
   - Project Name (required, text)
   - Project Type (required, dropdown): Architectural | Industrial | Infrastructure | Marine
   - Project Segmentation (required, multi-select): Decorative | Protective Coating | Floor Coating | Marine Coating
   - Project Source (required, dropdown): Canvassing | Referral from Customer | Referral from Principal | Website Inquiry | Exhibition | Repeat Customer
   - Primary Contact (required, dropdown dari company contacts)
   - Estimated Value (required, numeric, IDR)
   - Expected Close Date (optional, date picker)
   - Status (default: 'active', tersembunyi saat create)
2. Validasi: Nama project tidak boleh kosong
3. Validasi: Estimated value harus > 0
4. Validasi: Project segmentation array tidak boleh kosong
5. Validasi: Expected close date tidak boleh di masa lalu
6. Simpan ke SQLite dan sync
7. Success toast message
8. `company_id` dan `created_by` di-set secara otomatis

**Technical Notes:**
- Multi-select: Simpan sebagai JSON array di SQLite, PostgreSQL array di Supabase
- Currency: Simpan sebagai INTEGER cents (contoh: Rp 50.000.000 = 5.000.000.000 cents)
- Input uang dengan thousand separator untuk UX

**Dependencies:** US-2.2 (Create company), US-3.2 (Create contact)

---

#### US-4.3: Edit Project
**Story Points:** 3
**Priority:** P0 (Critical)

**User Story:**
Sebagai sales rep
Saya ingin memperbarui informasi project (value, status, close date)
Sehingga data project tetap akurat

**Acceptance Criteria:**
1. Edit semua fields dari create
2. **Field tambahan ketika status = 'won':**
   - Actual Value (required, numeric)
   - Won Date (required, date)
3. **Field tambahan ketika status = 'lost':**
   - Lost Reason (required, text area)
4. Validasi yang sama dengan create
5. Perubahan value secara otomatis di-log ke `project_value_log` (via database trigger)
6. User hanya dapat mengedit projects yang mereka buat

**Technical Notes:**
- Tampilan field conditional berdasarkan pilihan status
- Server trigger melog perubahan value ke audit table
- Update timestamp `updated_at`

**Dependencies:** US-4.2 (Create project)

---

#### US-4.4: Hapus Project (Soft Delete)
**Story Points:** 1
**Priority:** P2 (Medium)

**User Story:**
Sebagai sales rep
Saya ingin menghapus project yang diinput secara salah
Sehingga pipeline saya tetap bersih

**Acceptance Criteria:**
1. Confirmation dialog sebelum delete
2. Soft delete (set `deleted_at`)
3. User hanya dapat menghapus projects yang mereka buat
4. Tidak dapat menghapus project dengan existing reports (validation error)

**Technical Notes:**
- Validasi: `NOT IN (SELECT project_id FROM reports WHERE deleted_at IS NULL)`

**Dependencies:** US-4.2 (Create project)

---

### EPIC 5: Create Report (38 Story Points - Updated)

#### US-5.1: Buat Visit Report
**Story Points:** 31 (+18 untuk nested inline creation)
**Priority:** P0 (Critical - Core Feature)

**User Story:**
Sebagai sales rep
Saya ingin membuat visit report
Sehingga manager saya dapat melacak aktivitas saya

**Acceptance Criteria:**
1. Form fields:
   - Project (required, dropdown dari user's projects ATAU create new inline)
     - Dropdown menampilkan existing projects
     - Opsi "[+ Buat Project Baru]" di bottom dropdown
     - Jika dipilih → Inline project creation form expand
     - **Nested Company Creation:**
       - Company dropdown menampilkan existing companies
       - Opsi "[+ Buat Company Baru]" di bottom
       - Jika dipilih → Nested company form expand (1 field: Nama Company)
     - **Nested Contact Creation:**
       - Primary Contact dropdown menampilkan contacts dari selected company
       - Opsi "[+ Buat Contact Baru]" di bottom
       - Jika dipilih → Nested contact form expand (Nama, Posisi, Phone, Email)
     - Semua project required fields harus diisi (US-4.2)
   - Report Type (required, dropdown): Initial Visit | Follow-up Meeting | Technical Presentation | Price Quotation | Closing Visit | After Sales Visit
   - Visit Date (required, date picker, default: today, tidak boleh future)
   - Attendees (required, multi-select dari company contacts, minimum 1, dengan penandaan primary contact)
   - Notes (optional, text area, max 5000 karakter)
   - Next Action (optional, text area, max 5000 karakter)
   - Outcome (optional, dropdown): Positive | Neutral | Negative
   - Photos (optional, max 10, dari camera atau gallery)
   - GPS (auto-capture saat form dibuka, OPTIONAL - tidak blocking jika ditolak/tidak ada signal)
2. Validasi: Visit date tidak boleh di future
3. Validasi: Minimal 1 attendee dipilih
4. Validasi: Tepat 1 attendee ditandai sebagai primary
5. **Draft auto-save setiap 30 detik** (`is_draft = true` di local DB)
6. Simpan ke SQLite dan sync ke Supabase
7. Success toast message
8. **Nested Inline Creation (BARU - Critical untuk real-world workflow):**
   - User dapat create new project tanpa leave report form
   - Di dalam project form, user dapat create new company inline (jika belum ada)
   - Di dalam project form, user dapat create new contact inline (jika belum ada)
   - Semua nested forms menggunakan "bottom of dropdown" pattern untuk discoverability
   - Setelah creating nested entity → Auto-selected di parent dropdown
   - User dapat collapse inline forms tanpa save (dengan confirmation)
   - Visual hierarchy: Report (white) → Project (light gray) → Company/Contact (light blue, indented)
   - Success feedback: Green checkmark + toast message setelah setiap save
   - Validation: Real-time untuk semua nested forms, disable save sampai valid
9. Photos di-compress sebelum upload (target: <500KB masing-masing)
10. Photos di-upload ke: `{user_id}/{year}/{month}/{report_id}/{photo_id}.jpg`

**Technical Notes:**
- GPS auto-capture dengan timeout 10s (set NULL jika gagal)
- Draft disimpan dengan `is_draft = true` (local only, tidak di-sync)
- Kompresi photo: library flutter_image_compress
- Upload photos setelah report dibuat (batch upload)
- Handle offline: Queue photo uploads untuk nanti
- Set `created_by = current_user.id`
- **Nested inline creation flow:**
  - Save order: Company → Contact → Project → Report (maintain referential integrity)
  - Semua entities disimpan ke SQLite immediately dengan is_synced=false
  - Auto-link IDs: Company.id → Project.company_id, Contact.id → Project.primary_contact_id
  - Validate semua nested entities sebelum allow report submission
  - Jika user abandon inline creation → Draft entities disimpan, linked ke draft report
  - Dropdown pattern: "[+ Buat [Entity] Baru]" selalu di bottom, blue/green text, ➕ icon
- **UX specifications:** Lihat designer-brief/NESTED_INLINE_CREATION_WIREFRAMES.md

**Dependencies:** US-4.2 (Create project), US-3.2 (Create contact)

---

#### US-5.2: Lihat Report Detail
**Story Points:** 3
**Priority:** P0 (Critical)

**User Story:**
Sebagai sales rep atau manager
Saya ingin melihat informasi detail report
Sehingga saya dapat mereview aktivitas kunjungan

**Acceptance Criteria:**
1. Tampilkan semua field report (read-only)
2. Tampilkan list attendees dengan primary contact yang di-highlight
3. Tampilkan photos di gallery (tap untuk full screen)
4. Tampilkan lokasi GPS di map (jika tersedia)
5. Tampilkan sync status (synced / pending sync)
6. Tombol edit (jika user adalah creator)
7. Tombol delete (jika user adalah creator)

**Technical Notes:**
- Gunakan cached images untuk viewing offline
- Map widget: Google Maps atau OpenStreetMap
- Manager memiliki akses read-only (tidak ada tombol edit/delete)

**Dependencies:** US-5.1 (Create report)

---

#### US-5.3: Edit Report
**Story Points:** 3
**Priority:** P1 (High)

**User Story:**
Sebagai sales rep
Saya ingin mengedit report yang saya buat
Sehingga saya dapat memperbaiki kesalahan

**Acceptance Criteria:**
1. Edit semua fields (sama dengan create)
2. Dapat menambah/menghapus photos
3. Dapat menambah/menghapus attendees
4. User hanya dapat mengedit reports yang mereka buat
5. Simpan dan sync

**Technical Notes:**
- Handle photo deletion: Tandai untuk cleanup di storage
- Update timestamp `updated_at`
- Draft auto-save berlaku untuk edits

**Dependencies:** US-5.1 (Create report)

---

#### US-5.4: Hapus Report (Soft Delete)
**Story Points:** 1
**Priority:** P2 (Medium)

**User Story:**
Sebagai sales rep
Saya ingin menghapus report yang diinput secara salah
Sehingga data saya tetap bersih

**Acceptance Criteria:**
1. Confirmation dialog sebelum delete
2. Soft delete (set `deleted_at`)
3. User hanya dapat menghapus reports yang mereka buat
4. Photos di storage juga ditandai untuk cleanup

**Technical Notes:**
- Soft delete report dan attachments
- Cleanup job menghapus orphaned photos (Phase 2)

**Dependencies:** US-5.1 (Create report)

---

### EPIC 6: View Reports & Dashboard (15 Story Points)

#### US-6.1: Lihat My Reports (Sales Rep)
**Story Points:** 5
**Priority:** P0 (Critical)

**User Story:**
Sebagai sales rep
Saya ingin melihat semua reports yang telah saya buat
Sehingga saya dapat mereview aktivitas saya

**Acceptance Criteria:**
1. List semua reports oleh current user (`created_by = current_user.id`)
2. Tampilkan: Company Name, Project Name, Visit Date, Report Type, Sync Status
3. Sort berdasarkan: Visit Date (newest first), Created At
4. Filter berdasarkan: Report Type, Date Range (This Week, This Month, Custom)
5. Search berdasarkan: Company Name, Project Name
6. Badge/indicator untuk sync status (synced vs pending)
7. Pull-to-refresh memicu sync
8. Empty state jika tidak ada reports
9. **Pagination:** Load 20 reports per page (MVP performance requirement)

**Technical Notes:**
- Offline-first: Query dari SQLite
- Sync indicator dari field `is_synced`
- Tampilkan count pending syncs di atas
- Efficient indexed queries

**Dependencies:** US-5.1 (Create report)

---

#### US-6.2: Lihat Team Reports (Manager)
**Story Points:** 5
**Priority:** P0 (Critical)

**User Story:**
Sebagai manager
Saya ingin melihat semua team reports
Sehingga saya dapat memantau aktivitas tim

**Acceptance Criteria:**
1. List SEMUA reports dari semua sales reps (jika user role = manager)
2. Tampilkan: Sales Rep Name, Company, Project, Visit Date, Report Type
3. Sort berdasarkan: Visit Date, Sales Rep Name
4. Filter berdasarkan: Sales Rep, Report Type, Date Range
5. Search berdasarkan: Company, Project, Sales Rep
6. Tap report untuk melihat detail (read-only)
7. **Pagination:** Load 20 reports per page

**Technical Notes:**
- RLS policy memungkinkan managers membaca semua reports
- Fetch dari Supabase (managers selalu online)
- Manager tidak dapat mengedit/menghapus reports (read-only)

**Dependencies:** US-5.1 (Create report), US-1.1 (Login with role)

---

#### US-6.3: Simple Dashboard Statistics
**Story Points:** 5
**Priority:** P1 (High)

**User Story:**
Sebagai sales rep atau manager
Saya ingin melihat summary statistics
Sehingga saya dapat melacak performance

**Acceptance Criteria:**
1. **Sales Rep Dashboard menampilkan:**
   - Total Companies (dibuat oleh saya)
   - Total Projects berdasarkan status: Active, Won, Lost
   - Total Reports (bulan ini)
   - Total Pipeline Value (sum dari estimated values dari active projects)
2. **Manager Dashboard menampilkan:**
   - Total Team Members
   - Total Reports (bulan ini, seluruh tim)
   - Total Pipeline Value (seluruh tim)
   - Top Performers (berdasarkan report count bulan ini)
3. Tombol refresh untuk memperbarui statistics

**Technical Notes:**
- Query agregasi sederhana
- Cache stats untuk offline (sales rep)
- Manager selalu fetch fresh (online)
- Auto-refresh saat pull-down gesture

**Dependencies:** US-4.1 (View projects), US-6.1/6.2 (View reports)

---

### EPIC 7: Offline & Sync (17 Story Points)

#### US-7.1: Offline Create/Edit
**Story Points:** 5
**Priority:** P0 (Critical - Core Feature)

**User Story:**
Sebagai sales rep
Saya ingin membuat/mengedit reports tanpa internet
Sehingga saya tetap produktif di lapangan

**Acceptance Criteria:**
1. App sepenuhnya fungsional offline (create, edit companies, contacts, projects, reports)
2. Data disimpan ke SQLite dengan flag `is_synced = false`
3. Indikator UI yang jelas: badge "Offline Mode" atau "No Connection"
4. Actions di-queue untuk sync saat online

**Technical Notes:**
- Gunakan connectivity_plus untuk mendeteksi status koneksi
- Semua writes ke SQLite terlebih dahulu
- Background sync queue untuk pending operations
- Offline indicator di app bar

**Dependencies:** Semua create/edit user stories (US-2.2, US-3.2, US-4.2, US-5.1)

---

#### US-7.2: Auto Sync Saat Online
**Story Points:** 8
**Priority:** P0 (Critical - Core Feature)

**User Story:**
Sebagai sales rep
Saya ingin data saya auto-sync saat koneksi kembali
Sehingga data saya aman di-backup ke cloud

**Acceptance Criteria:**
1. Auto-detect saat koneksi internet tersedia
2. Upload pending data dari SQLite ke Supabase (companies, contacts, projects, reports, photos)
3. **Urutan sync:** Companies → Contacts → Projects → Reports → Attendees → Value Log → Photos (mempertahankan referential integrity)
4. Progress indicator selama sync ("Syncing 5 items...")
5. Success toast setelah sync selesai
6. Update `is_synced = true` dan `synced_at = NOW()` di local DB
7. Handle sync errors dengan baik dengan retry logic
8. **Conflict resolution:** Last-Write-Wins (MVP strategy)

**Technical Notes:**
- Batch upload untuk efisiensi
- Retry logic: Max 3 attempts dengan exponential backoff
- Photo upload: Max 3 concurrent uploads
- Jika server memiliki `updated_at` yang lebih baru, server menang (overwrite local)
- Transaction log untuk rollback capability (lihat Technical_Spec.md)

**Dependencies:** US-7.1 (Offline operation), database/Sync_Strategy.md

---

#### US-7.3: Tombol Manual Sync
**Story Points:** 2
**Priority:** P1 (High)

**User Story:**
Sebagai sales rep
Saya ingin memicu sync secara manual
Sehingga saya dapat mengontrol kapan mengupload data (contoh: saat di WiFi)

**Acceptance Criteria:**
1. Tombol sync di home screen atau profile screen
2. Tampilkan badge count pending sync ("5 pending")
3. Trigger manual sync saat di-tap
4. Tombol disabled saat offline atau sedang syncing
5. Success/error toast message

**Technical Notes:**
- Logic sync yang sama dengan auto-sync (US-7.2)
- Cek connectivity sebelum mencoba
- Tampilkan loading indicator selama sync

**Dependencies:** US-7.2 (Auto sync logic)

---

#### US-7.4: Download Team Data (Manager)
**Story Points:** 2
**Priority:** P2 (Medium)

**User Story:**
Sebagai manager
Saya ingin mendownload semua team data untuk viewing offline
Sehingga saya dapat mereview data tanpa internet

**Acceptance Criteria:**
1. Tombol "Download All Data" di manager dashboard
2. Download companies, projects, reports dari seluruh tim
3. Simpan ke local SQLite
4. Progress indicator selama download
5. Success toast message

**Technical Notes:**
- Paginated download untuk large datasets
- Periodic refresh (contoh: daily)
- Fitur khusus manager (sales reps tidak memerlukan ini)

**Dependencies:** US-6.2 (View team reports)

---

### EPIC 8: User Profile & Settings (5 Story Points)

#### US-8.1: Lihat User Profile
**Story Points:** 2
**Priority:** P1 (High)

**User Story:**
Sebagai user
Saya ingin melihat informasi profile saya
Sehingga saya dapat mengonfirmasi akun mana yang saya gunakan

**Acceptance Criteria:**
1. Profile screen menampilkan:
   - Full Name
   - Email
   - Role (Sales Rep / Manager)
   - Account Status (Active / Inactive)
2. Tombol logout dapat diakses dari profile

**Technical Notes:**
- Fetch dari table `user_profiles`
- Read-only (profile editing di luar lingkup MVP)
- Cache profile data secara lokal

**Dependencies:** US-1.1 (Login)

---

#### US-8.2: App Settings
**Story Points:** 3
**Priority:** P2 (Medium)

**User Story:**
Sebagai user
Saya ingin mengkonfigurasi preferensi app
Sehingga app bekerja sesuai kebutuhan saya

**Acceptance Criteria:**
1. Opsi settings:
   - Auto-sync On/Off (default: On)
   - WiFi-only sync On/Off (default: Off)
   - Photo quality Low/Medium/High (default: Medium)
   - Tombol clear cache
   - Info versi app
2. Settings disimpan ke local storage
3. Settings diterapkan segera

**Technical Notes:**
- Gunakan shared_preferences untuk persistence settings
- Photo quality mempengaruhi compression ratio (Low=100KB, Medium=500KB, High=1MB)
- Clear cache menghapus cached photos, bukan database

**Dependencies:** None (fitur independent)

---

## 4. Non-Functional Requirements

### 4.1 Performance

- Pembuatan report: <5 detik untuk save
- Load list view: <500ms
- Sync (10 reports): <5 detik di koneksi bagus
- Query search: <300ms
- **Pagination wajib:** Lists harus load 20 items per page (tidak dapat diterima untuk skip - lihat PRD acceptable risk table)

### 4.2 Usability

- Kepatuhan Material Design 3
- Optimasi penggunaan mobile satu tangan
- Maksimum 3 taps untuk actions umum
- Error messages yang jelas (tidak ada jargon teknis)
- Loading states untuk semua async operations

### 4.3 Reliability

- **Zero data loss** (tidak dapat diterima - lihat PRD acceptable risk table)
- **Sync success rate >95%**
- Draft auto-save setiap 30 detik
- Sync transaction log untuk rollback
- App crashes dapat diterima JIKA draft auto-save mencegah data loss

### 4.4 Data Integrity

- **Currency disimpan sebagai INTEGER** (tidak dapat diterima menggunakan floating-point)
- Foreign key constraints ditegakkan
- Soft deletes (tidak ada hard deletes)
- Audit trail untuk perubahan value

**Untuk handling edge case yang detail:** Lihat Technical_Spec.md

---

## 5. Definition of Done

Setiap user story dianggap SELESAI ketika:

- [ ] Semua acceptance criteria terpenuhi dan ditest
- [ ] Code passes unit tests (60%+ coverage untuk business logic)
- [ ] Bekerja offline (save ke Drift SQLite)
- [ ] Bekerja online (sync ke Supabase)
- [ ] UI mengikuti guideline Material Design 3
- [ ] Error handling diimplementasi
- [ ] Loading states diimplementasi
- [ ] Code direview dan di-merge ke branch `develop`
- [ ] Tidak ada critical bugs
- [ ] Database migrations ditest (jika applicable)
- [ ] Target performance terpenuhi

---

## 6. Story Point Summary

| Epic | Story Points | User Stories | Priority |
|------|--------------|--------------|----------|
| EPIC 1: Authentication | 5 | 2 | P0 |
| EPIC 2: Manage Companies | 12 | 4 | P0 |
| EPIC 3: Manage Contacts | 12 | 4 | P0 |
| EPIC 4: Manage Projects | 15 | 4 | P0 |
| EPIC 5: Create Report | **38** (+18 nested inline creation) | 4 | P0 |
| EPIC 6: View Reports & Dashboard | 15 | 3 | P0 |
| EPIC 7: Offline & Sync | 17 | 4 | P0 |
| EPIC 8: User Profile & Settings | 5 | 2 | P1-P2 |
| **TOTAL** | **124** (+18 dari baseline) | **22** | |

### Priority Levels

- **P0 (Critical):** Harus ada untuk MVP, memblokir fitur lain
- **P1 (High):** Penting untuk MVP, tetapi dapat ditunda jika diperlukan
- **P2 (Medium):** Nice to have, dapat dipindahkan ke Phase 2

---

## 7. Development Timeline Estimate

**Total Story Points:** 124 (+18 untuk nested inline creation)

**Asumsi:**
- Solo developer: 10-15 story points/minggu
- Estimasi durasi: 8-12 minggu
- Rekomendasi: 11.5 minggu dengan buffer (+1.5 minggu untuk nested creation complexity)

**Breakdown per Fase:**
- Week 1-2: Setup + Authentication + Companies (17 pts)
- Week 3-4: Contacts + Projects (27 pts)
- Week 5-8: Reports dengan Nested Inline Creation (38 pts)
- Week 9-10: Dashboard + Sync (32 pts)
- Week 11-11.5: Profile + Testing + Bug fixes (10 pts)

**Critical Path:** EPIC 7 (Offline & Sync) - Diperlukan untuk production readiness

---

## 8. Traceability Matrix

| User Story | Database Tables | API Endpoints | UI Screens |
|------------|----------------|---------------|------------|
| US-1.1 | user_profiles | auth/login | LoginScreen |
| US-2.1 | companies | GET /companies | CompaniesListScreen |
| US-2.2 | companies | POST /companies | CreateCompanyScreen |
| US-3.1 | contacts | GET /contacts | ContactsListScreen |
| US-3.2 | contacts | POST /contacts | CreateContactScreen |
| US-4.1 | projects | GET /projects | ProjectsListScreen |
| US-4.2 | projects, contacts | POST /projects | CreateProjectScreen |
| US-5.1 | reports, attachments, report_attendees | POST /reports, POST /storage | CreateReportScreen |
| US-6.1 | reports, projects, companies | GET /reports | MyReportsListScreen |
| US-6.3 | All tables | GET /reports, /projects | DashboardScreen |
| US-7.2 | sync_transactions | Multiple (batch) | SyncService (background) |

---

## 9. Revision History

| Version | Date | Changes | Author |
|---------|------|---------|--------|
| 1.0 | Oct 2025 | Initial MVP requirements | A Kevin Zakaria |
| 2.0 | Oct 2025 | Added edge case handling notes | A Kevin Zakaria |
| 3.0 | Oct 2025 | Production-ready spec dengan pagination, currency fixes | Development Team |

---

**Document Status:** ✅ Complete - Ready untuk development
**Terakhir Diperbarui:** Oktober 2025

---

## 📍 Navigasi

**Selesai membaca User Stories?**
- ✅ [Lanjut ke: Database Schema →](./DATABASE_SCHEMA.md)

**Atau kembali ke:**
- ← [MVP Scope](./MVP_SCOPE.md)
- 🏠 [README](./README.md)
