# Screen Inventory - Daftar Complete Screens

← [Sebelumnya: CSS Logo Guidelines.pdf](./CSS%20Logo%20Guidelines.pdf)

---

## 📋 Overview

Dokumen ini berisi daftar lengkap **31 screens** yang harus Anda design untuk MVP Phase.

**Total Screens:** 31 main screens + 15-20 states = ~50 designs

---

## 🎯 Priority Screens (Design First)

Ini adalah 5 screens yang HARUS di-design di Week 1 untuk review:

| Priority | Screen Name | Complexity | User Story | Estimated Time |
|----------|-------------|------------|------------|----------------|
| **P0** | Create Report Flow | Very High | US-5.1 | 8-10 hours |
| **P0** | Companies List + Create/Edit | Medium | US-2.1, US-2.2, US-2.3 | 4-5 hours |
| **P0** | Contacts List + Create/Edit | Medium | US-3.1, US-3.2, US-3.3 | 4-5 hours |
| **P0** | Projects List + Create/Edit | Medium | US-4.1, US-4.2, US-4.3 | 4-5 hours |
| **P0** | Report Detail View | Medium | US-5.2 | 3-4 hours |

**Total Priority Screens:** ~7-8 actual screen designs (some flows have multiple screens)

---

## 📱 Complete Screen List (31 Screens)

### EPIC 1: Authentication (2 Screens)

#### 1. Login Screen
- **User Story:** US-1.1
- **User:** All users (sales rep & manager)
- **Key Elements:**
  - Email input field
  - Password input field (dengan show/hide toggle)
  - "Masuk" button (primary)
  - Error message area (jika login failed)
  - CSS logo
  - Background image (optional, brand-appropriate)
- **States to Design:**
  - Empty (belum isi)
  - Filled (email + password filled)
  - Error (invalid credentials)
  - Loading (submitting)

#### 2. Splash Screen
- **User:** All users
- **Key Elements:**
  - CSS logo (centered)
  - App name "CSS Sales Report"
  - Loading indicator (optional)
- **Notes:** Shown saat app first launch, max 2 seconds

---

### EPIC 2: Manage Companies (5 Screens)

#### 3. Companies List
- **User Story:** US-2.1
- **User:** Sales rep (owned companies), Manager (all companies)
- **Key Elements:**
  - Top App Bar: "Perusahaan" + search icon + filter icon
  - Search bar (expandable)
  - List of company cards:
    - Company name (title)
    - Address (subtitle, truncated)
    - Jumlah contacts + projects (metadata)
  - FAB "+" button (create company)
  - Pagination (load more button / infinite scroll)
  - Pull-to-refresh
- **States:**
  - Empty (belum ada companies)
  - Loaded (20 companies)
  - Loading (shimmer placeholders)
  - Search results (0 results vs found results)

#### 4. Company Detail
- **User Story:** US-2.1
- **User:** Sales rep, Manager
- **Key Elements:**
  - Top App Bar: Company name + edit icon (sales rep only) + back button
  - Company info section:
    - Name
    - Address
    - City
    - Created by (nama sales rep)
    - Created date
  - Tabs: "Kontak" | "Proyek"
  - Tab content (list of contacts OR projects)
  - FAB "+" (add contact or project, depending on active tab)

#### 5. Create Company Form
- **User Story:** US-2.2
- **User:** Sales rep only
- **Key Elements:**
  - Top App Bar: "Tambah Perusahaan" + back + save icon
  - Form fields:
    - Nama Perusahaan* (required, text input)
    - Alamat (optional, text input multiline)
    - Kota (optional, text input)
  - Bottom action bar:
    - "Batal" button (secondary)
    - "Simpan" button (primary)
  - Draft auto-save indicator (kecil, di bottom: "Terakhir disimpan 1 menit yang lalu")
- **States:**
  - Empty form
  - Partially filled
  - Validation error (nama empty → show red text)
  - Saving (button shows spinner)
  - Success (redirect to company detail)

#### 6. Edit Company Form
- **User Story:** US-2.3
- **User:** Sales rep only (own companies)
- **Key Elements:**
  - Same as Create Form, tapi pre-filled dengan existing data
  - Top App Bar: "Edit Perusahaan"
- **Notes:** Reuse Create Company design, just pre-populate fields

#### 7. Company Search Results
- **User:** Sales rep, Manager
- **Key Elements:**
  - Search bar (active, dengan X button to clear)
  - Results count: "3 perusahaan ditemukan"
  - Filtered list (same card design as Companies List)
  - Empty state jika 0 results

---

### EPIC 3: Manage Contacts (5 Screens)

#### 8. Contacts List (per Company)
- **User Story:** US-3.1
- **User:** Sales rep (own contacts), Manager (all contacts)
- **Key Elements:**
  - Shown di Company Detail page (dalam tab "Kontak")
  - List of contact cards:
    - Name + "Primary Contact" badge (jika is_primary = true)
    - Position
    - Phone number
    - Email (jika ada)
  - FAB "+" (add contact)
  - Pagination
- **States:**
  - Empty (belum ada contacts untuk company ini)
  - Loaded (10 contacts)

#### 9. Contact Detail
- **User Story:** US-3.1
- **User:** Sales rep, Manager
- **Key Elements:**
  - Top App Bar: Contact name + edit icon + back
  - Contact info:
    - Name
    - Position
    - Phone (dengan icon untuk call/WhatsApp)
    - Email (dengan icon untuk email)
    - Is Primary Contact (checkbox or badge)
    - Created by
    - Created date
  - Action buttons:
    - Call (open phone dialer)
    - WhatsApp (open WhatsApp)
    - Email (open email app)

#### 10. Create Contact Form
- **User Story:** US-3.2
- **User:** Sales rep only
- **Key Elements:**
  - Top App Bar: "Tambah Kontak" + back + save
  - Form fields:
    - Nama* (required)
    - Posisi (optional)
    - No. Telepon (optional, phone number format)
    - Email (optional, email format validation)
    - Kontak Utama? (checkbox)
  - Bottom actions: Batal | Simpan
  - Draft auto-save indicator
- **States:**
  - Empty, filled, validation errors, saving, success

#### 11. Edit Contact Form
- **User Story:** US-3.3
- **User:** Sales rep only
- **Key Elements:**
  - Same as Create Contact, pre-filled

#### 12. Contact Search Results
- **User:** Sales rep, Manager
- **Key Elements:**
  - Search by name, position, or phone
  - Filtered list

---

### EPIC 4: Manage Projects (5 Screens)

#### 13. Projects List (per Company)
- **User Story:** US-4.1
- **User:** Sales rep (own projects), Manager (all projects)
- **Key Elements:**
  - Shown di Company Detail page (dalam tab "Proyek")
  - List of project cards:
    - Project name (title)
    - Status badge (Active, Won, Lost, On Hold) - color coded
    - Estimated value (Rp 50,000,000)
    - Expected close date
  - FAB "+"
  - Filter by status (dropdown: All | Active | Won | Lost | On Hold)
  - Pagination
- **States:**
  - Empty, loaded

#### 14. Project Detail
- **User Story:** US-4.1
- **User:** Sales rep, Manager
- **Key Elements:**
  - Top App Bar: Project name + edit icon + back
  - Project info:
    - Project Name
    - Type (Architectural | Industrial | Infrastructure | Marine)
    - Segmentation (chips: Decorative, Protective Coating, etc)
    - Source (Canvassing | Referral | etc)
    - Status (badge)
    - Estimated Value (Rp formatted)
    - Expected Close Date
    - Primary Contact (nama + link to contact detail)
    - Created by
    - Created date
  - Value change log (accordion, collapsed by default):
    - History of value changes (old value → new value, date, changed by)

#### 15. Create Project Form
- **User Story:** US-4.2
- **User:** Sales rep only
- **Key Elements:**
  - Top App Bar: "Tambah Proyek" + back + save
  - Form fields:
    - Nama Proyek* (text input)
    - Tipe Proyek* (dropdown: Architectural, Industrial, Infrastructure, Marine)
    - Segmentasi* (multi-select chips: Decorative, Protective Coating, Floor Coating, Marine Coating)
    - Sumber* (dropdown: Canvassing, Referral from Customer, etc)
    - Status* (dropdown: Active, Won, Lost, On Hold - default: Active)
    - Nilai Estimasi* (currency input, Rp format)
    - Tanggal Penutupan (date picker, optional)
    - Kontak Utama* (dropdown list dari contacts perusahaan ini)
  - Bottom actions: Batal | Simpan
  - Draft auto-save
- **States:**
  - Empty, filled, validation errors, saving, success
- **Special Notes:**
  - Currency input harus jelas format: "Rp 50,000,000" (dengan thousand separators)

#### 16. Edit Project Form
- **User Story:** US-4.3
- **User:** Sales rep only
- **Key Elements:**
  - Same as Create Project, pre-filled
  - Jika estimated value di-edit → trigger value change log entry (background, tidak perlu UI confirmation)

#### 17. Project Search/Filter Results
- **User:** Sales rep, Manager
- **Key Elements:**
  - Filter by status
  - Filtered list

---

### EPIC 5: Create & View Reports (6 Screens)

#### 18. Create Report Screen (UPDATED - Nested Inline Creation)
- **User Story:** US-5.1 (Enhanced dengan +18 SP untuk nested inline creation)
- **User:** Sales rep only
- **Purpose:** Single-screen report creation dengan ability create project/company/contact inline
- **Key Elements:**
  - Top App Bar: "Buat Laporan Kunjungan" + back
  - **Project Selection (Primary Field):**
    - Dropdown dengan existing projects OR "[+ Buat Project Baru]" at bottom
    - Auto-fills company/contact jika existing project selected
  - **Inline Project Creation Card (Expandable):**
    - Company dropdown dengan "[+ Buat Company Baru]" nested option
    - Nested company creation form (1 field: Company Name)
    - Primary Contact dropdown dengan "[+ Buat Contact Baru]" nested option
    - Nested contact creation form (4 fields: Name, Position, Phone, Email)
    - Project fields (Name, Type, Segmentation, Estimated Value)
    - Visual depth: Light gray background, 5-10dp left indent
  - **Report Fields:**
    - Report Type, Visit Date, Attendees (multi-select), Notes, Next Action, Outcome, Photos (1-10), GPS (optional)
  - **State Management:**
    - Auto-save draft every 30s (includes draft project/company/contact)
    - Real-time validation untuk all nested forms
    - Success feedback: Green checkmark + toast after each nested save
  - Bottom actions: "Batalkan" (secondary) | "Simpan Laporan" (primary)
- **States:**
  - Default (no project selected)
  - Project dropdown open (with existing + create new option)
  - Inline project creation expanded
  - Nested company creation expanded (Level 3)
  - Nested contact creation expanded (Level 3)
  - After nested save - success (checkmarks, toast)
  - Validation errors (real-time, per field)
  - Submitting (spinner + disabled)
- **Interactions:**
  - Tap "[+ Buat Project Baru]" → Inline project form expands (300ms animation)
  - Tap "[+ Buat Company Baru]" → Nested company form expands within project form
  - After nested save → Form collapses, entity auto-selected in parent dropdown
  - User can collapse without saving (show confirmation: "Buang perubahan?")
- **Wireframes:** See NESTED_INLINE_CREATION_WIREFRAMES.md for complete UI states
- **Priority:** P0 (Critical - matches real sales rep workflow)

#### 19. Report Detail View
- **User Story:** US-5.2
- **User:** Sales rep (own reports), Manager (all reports)
- **Key Elements:**
  - Top App Bar: "Detail Laporan" + back + share icon (optional)
  - Sync status badge (top right):
    - "Tersinkronisasi" (green check)
    - "Menyinkronkan..." (blue spinner)
    - "Gagal sync" (red X + retry button)
  - Report info sections:
    - Proyek (company name + project name, link to project detail)
    - Detail Laporan:
      - Tipe
      - Tanggal kunjungan
      - Peserta (list of names, primary contact ada badge)
      - Catatan (full text)
      - Tindakan selanjutnya
      - Hasil/outcome
    - Foto Gallery (scroll horizontal, tap untuk full screen)
    - Lokasi (map view jika GPS available, atau text "Lokasi tidak tersedia")
    - Metadata:
      - Dibuat oleh (nama sales rep)
      - Dibuat pada (date + time)
      - Status sync
- **States:**
  - Synced (green badge)
  - Syncing (loading)
  - Failed (red badge + retry)
  - Offline (gray badge)

#### 23. Photo Full Screen View
- **User:** Sales rep, Manager
- **Key Elements:**
  - Full screen photo (pinch to zoom)
  - Top App Bar (transparent): back + download icon
  - Bottom: photo counter "3 / 10"
  - Swipe left/right untuk next/prev photo

---

### EPIC 6: Dashboard & Reports List (4 Screens)

#### 24. Home/Dashboard (Sales Rep View)
- **User Story:** US-6.1
- **User:** Sales rep only
- **Key Elements:**
  - Top App Bar: "Beranda" + profile icon + sync icon
  - Sync status banner (jika ada pending sync):
    - "Menyinkronkan 5 dari 10 item..." (loading bar)
    - Atau "Offline - 10 item menunggu sync" (tap untuk see details)
  - Quick stats cards:
    - "Laporan Minggu Ini" (count)
    - "Laporan Pending Sync" (count)
    - "Perusahaan Aktif" (count)
  - Quick actions (button grid):
    - "Buat Laporan" (primary, largest)
    - "Lihat Laporan"
    - "Kelola Perusahaan"
    - "Kelola Kontak"
  - Recent reports (list, max 5):
    - Report preview (company, project, date, sync status badge)
    - "Lihat Semua" link
  - Bottom Navigation Bar:
    - Beranda (active)
    - Laporan
    - Perusahaan
    - Profil

#### 25. My Reports List (Sales Rep)
- **User Story:** US-6.1
- **User:** Sales rep only
- **Key Elements:**
  - Top App Bar: "Laporan Saya" + search icon + filter icon
  - Filter chips:
    - Tanggal (date range picker)
    - Perusahaan (multi-select dropdown)
    - Status Sync (All | Synced | Pending | Failed)
  - Sort dropdown: "Terbaru" | "Terlama" | "Perusahaan (A-Z)"
  - List of report cards:
    - Company name + Project name (title)
    - Tanggal kunjungan (subtitle)
    - Tipe laporan (badge)
    - Sync status (icon + badge)
    - Thumbnail foto pertama (jika ada)
  - Pagination (20 reports per page)
  - FAB "+" (create report)
- **States:**
  - Empty (belum ada reports)
  - Loaded
  - Filtered (show "X laporan ditemukan")
  - Loading

#### 26. Home/Dashboard (Manager View)
- **User Story:** US-6.2
- **User:** Manager only
- **Key Elements:**
  - Top App Bar: "Dashboard Tim" + filter icon + refresh icon
  - Summary cards (4 cards horizontal scroll):
    - "Total Laporan Minggu Ini" (all sales reps)
    - "Sales Rep Aktif" (count)
    - "Proyek Aktif" (count)
    - "Perusahaan Baru Bulan Ini" (count)
  - Filter by Sales Rep (dropdown: All | Sales Rep A | Sales Rep B)
  - Recent team reports (list, max 10):
    - Sales rep name (avatar + name)
    - Company + Project
    - Tanggal
    - Tipe laporan
    - Tap untuk detail
  - "Lihat Semua Laporan" button
  - Bottom Nav (different dari sales rep):
    - Dashboard (active)
    - Laporan Tim
    - Perusahaan
    - Profil

#### 27. Team Reports List (Manager)
- **User Story:** US-6.2
- **User:** Manager only
- **Key Elements:**
  - Top App Bar: "Laporan Tim" + search + filter
  - Filters:
    - Sales Rep (multi-select)
    - Tanggal (date range)
    - Perusahaan
    - Tipe Laporan
  - Sort options
  - List of reports (grouped by sales rep OR grouped by date):
    - Section header: "Sales Rep A - 5 laporan"
    - Report cards (same design as My Reports List + sales rep name tag)
  - Pagination
  - NO FAB (manager cannot create reports)
- **States:**
  - Empty (tidak ada reports dari tim - shouldn't happen, tapi design anyway)
  - Loaded
  - Filtered

---

### EPIC 7: Sync & Settings (3 Screens)

#### 28. Sync Status Screen
- **User Story:** US-7.2, US-7.3
- **User:** Sales rep (own data), Manager (team data sync)
- **Key Elements:**
  - Top App Bar: "Status Sinkronisasi" + back + refresh icon
  - Overall sync status card:
    - "Semua data tersinkronisasi" (green) OR
    - "10 item pending sync" (amber) OR
    - "5 item gagal sync" (red)
    - Last sync time: "Terakhir disinkronisasi 5 menit yang lalu"
  - Manual sync button: "Sinkronkan Sekarang" (primary button)
  - Sync details (accordion, collapsible):
    - Perusahaan: 0 pending, 2 failed (expand untuk see which ones)
    - Kontak: 3 pending, 0 failed
    - Proyek: 1 pending, 0 failed
    - Laporan: 5 pending, 3 failed
    - Foto: 10 pending (with upload progress per photo)
  - Network status indicator: "Online" (green) | "Offline" (gray)
  - Transaction log (optional, collapsible):
    - Last 20 sync transactions (timestamp, entity type, status)
- **States:**
  - All synced (green, nothing pending)
  - Syncing in progress (show progress bar + "Menyinkronkan 3/10...")
  - Partial failed (show which entities failed + retry button)
  - Offline (show "Anda offline, sync akan dilanjutkan saat online")

#### 29. Settings Screen
- **User Story:** Not in MVP user stories, but required for app structure
- **User:** All users
- **Key Elements:**
  - Top App Bar: "Pengaturan" + back
  - Settings sections (list):
    - Akun:
      - Email (read-only)
      - Role (sales rep / manager, read-only)
    - Sinkronisasi:
      - Auto-sync (toggle on/off)
      - Sync via WiFi only (toggle)
      - Sync interval (dropdown: Every 5 min | 15 min | 30 min | Manual only)
    - Aplikasi:
      - Bahasa (dropdown: Bahasa Indonesia | English - default: ID)
      - Tentang Aplikasi (link to About screen)
    - Akun:
      - Logout button (destructive, red text)
- **States:**
  - Default settings
  - Toggled settings

#### 30. Profile Screen
- **User:** All users
- **Key Elements:**
  - Top App Bar: "Profil" + back + edit icon (jika ada profile edit di MVP - NOT in MVP, so no edit icon)
  - Profile header:
    - Avatar (placeholder circle dengan initial)
    - Full name
    - Role badge (Sales Representative | Manager)
    - Email
  - Stats cards (if sales rep):
    - Total laporan dibuat
    - Total perusahaan
    - Total proyek
  - Quick actions:
    - Pengaturan (link to Settings)
    - Bantuan/Help
    - Tentang Aplikasi
  - Logout button (bottom, secondary/outlined)

---

### ADDITIONAL SCREENS (1 Screen)

#### 31. Onboarding/First-Time User Guide
- **User:** New users (first launch setelah login)
- **Key Elements:**
  - 3-4 slides (swipeable):
    - Slide 1: "Selamat datang di CSS Sales Report!" + illustration
    - Slide 2: "Buat laporan kunjungan dengan mudah" + illustration
    - Slide 3: "Bekerja offline, sync otomatis saat online" + illustration
    - Slide 4 (optional): "Mari mulai!" + CTA button
  - Skip button (top right, semua slides)
  - Dot indicators (bottom, show current slide)
  - "Mulai" button (last slide, primary)
- **Notes:**
  - Shown hanya sekali, setelah first login
  - User can skip kapan saja

---

## 📊 Screen Count Summary

| Epic | Screen Count | Complexity |
|------|--------------|------------|
| EPIC 1: Authentication | 2 | Low |
| EPIC 2: Companies | 5 | Medium |
| EPIC 3: Contacts | 5 | Medium |
| EPIC 4: Projects | 5 | Medium-High |
| EPIC 5: Reports | 6 | Very High |
| EPIC 6: Dashboard | 4 | Medium |
| EPIC 7: Sync & Settings | 3 | Medium |
| Additional | 1 | Low |
| **TOTAL** | **31** | **Mixed** |

---

## 🎨 States Summary (Not Counted as Separate Screens)

Ini akan di-design di Figma yang sama, tapi sebagai variants/states:

### Empty States (8 designs)
1. Empty Companies List
2. Empty Contacts List (per company)
3. Empty Projects List (per company)
4. Empty Reports List
5. Empty Search Results
6. Empty Notifications (future)
7. No Internet + Empty List
8. Manager Empty Dashboard

### Error States (5 designs)
1. Network Error
2. Sync Failed (with details)
3. Form Validation Error
4. Photo Upload Failed
5. Generic Error

### Loading States (4 designs)
1. List Loading (shimmer placeholders)
2. Form Submitting
3. Photo Uploading (progress per photo)
4. Sync in Progress

### Success States (3 designs)
1. Success Snackbar
2. Sync Success
3. Report Submitted Success

**Total States:** 20 additional designs

---

## 🚀 Design Order Recommendation

### Week 1: Priority Screens
1. Create Report Flow (Screens 18-21) - 8-10 hours
2. Companies CRUD (Screens 3, 5, 6) - 4-5 hours
3. Contacts CRUD (Screens 8, 10, 11) - 4-5 hours
4. Projects CRUD (Screens 13, 15, 16) - 4-5 hours
5. Report Detail (Screen 22) - 3-4 hours

### Week 2: Dashboard & Lists
6. Login (Screen 1) - 2 hours
7. Home Dashboard - Sales Rep (Screen 24) - 4 hours
8. My Reports List (Screen 25) - 3 hours
9. Manager Dashboard (Screen 26) - 4 hours
10. Team Reports List (Screen 27) - 3 hours

### Week 3: Details & Supporting Screens
11. Company Detail (Screen 4) - 3 hours
12. Contact Detail (Screen 9) - 2 hours
13. Project Detail (Screen 14) - 4 hours
14. Search Results (Screens 7, 12, 17) - 3 hours
15. Photo Full Screen (Screen 23) - 2 hours

### Week 4: Sync, Settings, Onboarding
16. Sync Status (Screen 28) - 4 hours
17. Settings (Screen 29) - 3 hours
18. Profile (Screen 30) - 2 hours
19. Splash (Screen 2) - 1 hour
20. Onboarding (Screen 31) - 3 hours

### Week 5: States & Polish
21. All Empty States (8 designs) - 4 hours
22. All Error States (5 designs) - 3 hours
23. All Loading States (4 designs) - 2 hours
24. All Success States (3 designs) - 2 hours
25. Design System finalization - 4 hours
26. Interactive Prototypes - 6 hours

---

## 📍 Navigasi

**Sudah paham daftar screens yang harus di-design?**

➡️ **[Lanjut ke: TECHNICAL_CONSTRAINTS.md →](./TECHNICAL_CONSTRAINTS.md)**

Dokumen selanjutnya akan menjelaskan:
- Offline UX patterns yang harus Anda implement
- Pagination requirements
- Technical constraints yang affect design decisions

**Atau kembali ke:**
← [USER_PERSONAS](./USER_PERSONAS.md)
