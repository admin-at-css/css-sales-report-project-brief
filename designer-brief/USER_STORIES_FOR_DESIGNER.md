# User Stories - Requirements Per Feature (Designer Version)

← [Sebelumnya: TECHNICAL_CONSTRAINTS](./TECHNICAL_CONSTRAINTS.md)

---

## 📋 Overview

Dokumen ini adalah **simplified version** dari developer user stories, focused on **UX requirements** yang relevant untuk designer.

**Developer user stories lengkap ada di:** `../USER_STORIES.md` (optional reading - 30 pages, very technical)

**This document:** Extracted UX-relevant requirements only (10 min read)

---

## 🎯 How to Use This Document

**Untuk each screen yang Anda design:**
1. Find the relevant user story di document ini
2. Read "What User Wants" (main goal)
3. Read "Key UI Elements" (what must be on screen)
4. Read "UX Requirements" (how it should behave)
5. Check "Edge Cases" (don't forget these!)

**Note:** Acceptance criteria technical (database, API) sudah di-filter out. You only see UX requirements here.

---

## 📱 EPIC 1: Authentication

### US-1.1: Login with Email & Password

**What User Wants:**
Sales rep atau manager ingin login menggunakan email dan password sehingga dapat mengakses app secara aman.

**Key UI Elements:**
- Email input field (with email validation)
- Password input field (dengan show/hide toggle)
- "Masuk" button (primary)
- CSS logo & branding
- Error message area

**UX Requirements:**
- Email harus valid format (example@domain.com)
- Password minimum 8 characters
- Show loading indicator saat authenticating
- Clear error messages:
  - "Email atau password salah" (invalid credentials)
  - "Tidak ada koneksi internet" (network error)
- Auto-redirect ke home setelah login success

**Edge Cases:**
- User offline saat login → show error, cannot login (login requires internet)
- Session tetap aktif setelah app restart (auto-login)

---

### US-1.2: Logout from App

**What User Wants:**
User ingin logout dari app sehingga akun aman jika orang lain pakai phone.

**Key UI Elements:**
- Logout button di profile/settings screen
- Confirmation dialog: "Apakah Anda yakin ingin logout?"

**UX Requirements:**
- Logout harus work offline
- Local data (SQLite) TIDAK dihapus saat logout
- Redirect ke login screen setelah logout

---

## 📱 EPIC 2: Manage Companies

### US-2.1: View List of Companies

**What User Wants:**
Sales rep ingin melihat list semua companies yang telah dibuat sehingga dapat mengelola customers.

**Key UI Elements:**
- Search bar (by company name)
- Filter button (future: by city, created date)
- Sort dropdown (Most Recent, Name A-Z)
- Company cards:
  - Company name (title, bold, 16sp)
  - Address (subtitle, truncated to 2 lines)
  - Metadata: X contacts, Y projects (small text, 12sp)
  - Sync status badge (synced/pending/failed)
- FAB "+" button (create new company)
- Pagination (20 companies per page, "Load More" button)

**UX Requirements:**
- Sales rep sees only own companies
- Manager sees ALL companies dari all sales reps
- Pull-to-refresh triggers sync
- Empty state jika belum ada companies
- Search filters list real-time (as user types)

---

### US-2.2: Create New Company

**What User Wants:**
Sales rep ingin menambahkan company baru sehingga dapat track customers baru.

**Key UI Elements:**
- Form fields:
  - Nama Perusahaan* (required, max 200 chars)
  - Alamat (optional, multiline)
  - Kota (optional)
- "Batal" button (secondary)
- "Simpan" button (primary)
- Draft auto-save indicator (bottom: "Terakhir disimpan 1 menit yang lalu")

**UX Requirements:**
- Validation:
  - Nama perusahaan required (show error if empty)
  - Duplicate nama not allowed (show error: "Perusahaan dengan nama ini sudah ada")
- Auto-save draft every 30 seconds
- Works offline (save ke local, sync later)
- Success feedback: "Perusahaan berhasil ditambahkan" (snackbar)
- Redirect ke company detail setelah save

**Edge Cases:**
- User closes app mid-form → draft auto-saved, restore on return
- Duplicate check: "PT ABC" = "PT ABC " (trim whitespace)

---

### US-2.3: Edit Company

**What User Wants:**
Sales rep ingin edit company info sehingga data tetap accurate (fix typos, update address).

**Key UI Elements:**
- Same form as Create Company, tapi pre-filled
- "Simpan Perubahan" button

**UX Requirements:**
- Sales rep can only edit OWN companies (not other sales rep's companies)
- Manager CANNOT edit (read-only)
- Validation sama seperti create
- Works offline (save local, sync later)
- If conflict (2 users edit sama company simultaneously) → Last-Write-Wins (no user prompt, automatic)

---

## 📱 EPIC 3: Manage Contacts

### US-3.1: View List of Contacts per Company

**What User Wants:**
Sales rep ingin melihat list contacts untuk selected company sehingga tahu siapa PIC di company tersebut.

**Key UI Elements:**
- Shown dalam Company Detail page, tab "Kontak"
- Contact cards:
  - Name (title)
  - Position (subtitle)
  - Phone number
  - "Primary Contact" badge (jika is_primary = true)
- Search bar (by name, position, phone)
- FAB "+" (add contact)

**UX Requirements:**
- Sales rep sees contacts dari own companies
- Manager sees all contacts
- Primary contact highlighted (badge, different color)
- Empty state jika belum ada contacts

---

### US-3.2: Create New Contact

**What User Wants:**
Sales rep ingin add contact baru untuk company sehingga bisa track PIC.

**Key UI Elements:**
- Form:
  - Nama* (required)
  - Posisi (optional)
  - No. Telepon (optional, phone format)
  - Email (optional, email format)
  - "Kontak Utama?" (checkbox)
- Auto-save draft indicator

**UX Requirements:**
- Validation:
  - Nama required
  - Email harus valid format jika diisi
  - Phone harus numeric jika diisi
- Only 1 primary contact per company (if user checks "Kontak Utama", other contacts di-uncheck automatically)
- Works offline

---

### US-3.3: Edit Contact

**What User Wants:**
Sales rep ingin edit contact info sehingga data up-to-date (phone change, promotion ke position baru).

**Key UI Elements:**
- Same form as Create Contact, pre-filled

**UX Requirements:**
- Can toggle "Kontak Utama" status
- Works offline

---

## 📱 EPIC 4: Manage Projects

### US-4.1: View List of Projects per Company

**What User Wants:**
Sales rep ingin melihat list projects untuk company sehingga dapat track sales opportunities.

**Key UI Elements:**
- Shown dalam Company Detail page, tab "Proyek"
- Project cards:
  - Project name (title)
  - Status badge (Active, Won, Lost, On Hold) - color coded
  - Estimated value (Rp 50,000,000)
  - Expected close date
- Filter by status (dropdown)
- FAB "+"

**UX Requirements:**
- Status color coding:
  - Active = Blue
  - Won = Green
  - Lost = Red
  - On Hold = Gray
- Currency format: Rp 50.000.000 (thousand separators)
- Sort by: Expected close date (nearest first) OR created date (newest first)

---

### US-4.2: Create New Project

**What User Wants:**
Sales rep ingin create project baru sehingga dapat track sales opportunity.

**Key UI Elements:**
- Form:
  - Nama Proyek* (required)
  - Tipe Proyek* (dropdown: Architectural, Industrial, Infrastructure, Marine)
  - Segmentasi* (multi-select chips: Decorative, Protective Coating, Floor Coating, Marine Coating)
  - Sumber* (dropdown: Canvassing, Referral from Customer, Referral from Principal, Website Inquiry, Exhibition, Repeat Customer)
  - Status* (dropdown: Active, Won, Lost, On Hold - default: Active)
  - Nilai Estimasi* (currency input, Rp format)
  - Tanggal Penutupan (date picker, optional)
  - Kontak Utama* (dropdown list dari contacts perusahaan ini)

**UX Requirements:**
- All fields required except Tanggal Penutupan
- Currency input: auto-format dengan thousand separators as user types
- Segmentasi: allow multiple selections (chips)
- If company belum punya contacts → disable Kontak Utama dropdown, show message: "Tambahkan kontak terlebih dahulu"
- Works offline

---

### US-4.3: Edit Project

**What User Wants:**
Sales rep ingin edit project (especially estimated value dan status) sehingga data reflect current sales pipeline.

**Key UI Elements:**
- Same form as Create Project, pre-filled

**UX Requirements:**
- If user changes Estimated Value → log value change untuk audit (background, no UI needed)
- Common flow: Status change from Active → Won (when deal closes)
- Works offline

---

## 📱 EPIC 5: Create & View Reports (MOST COMPLEX)

### US-5.1: Create Visit Report

**What User Wants:**
Sales rep ingin create visit report secara offline dengan photos sehingga report tidak hilang dan dapat di-sync nanti.

**Key UI Elements:**
Multi-step flow (4 steps):

**Step 1: Select Project**
- Search bar
- List of projects (grouped by company)
- Tap project → proceed to Step 2

**Step 2: Report Form**
- Selected project info (card at top, cannot edit, ada "Ganti Proyek" link)
- Form:
  - Tipe Laporan* (dropdown: Meeting, Site Visit, Quotation, Follow-up, Other)
  - Tanggal Kunjungan* (date picker, default: today)
  - Peserta* (multi-select dari contacts perusahaan ini, primary contact auto-selected)
  - Catatan (multiline text area, optional)
  - Tindakan Selanjutnya (text input, optional)
  - Hasil/Outcome (text input, optional)
- GPS auto-capture indicator: "Lokasi: [Lat, Long] ✓" atau "Lokasi tidak tersedia"
- Draft auto-save: "Terakhir disimpan 30 detik yang lalu"

**Step 3: Add Photos**
- Photo grid (max 10 photos)
- "Ambil Foto" button (camera icon)
- "Pilih dari Galeri" button (gallery icon)
- Photo counter: "3 dari 10 foto"
- Each photo has X button (remove)
- Can skip (photos optional)

**Step 4: Review & Submit**
- Review all fields (accordion, collapsed)
- Edit buttons per section
- "Simpan sebagai Draft" button (secondary)
- "Submit Laporan" button (primary)

**UX Requirements:**
- **MOST CRITICAL:** Draft auto-save every 30 seconds (user might exit app accidentally)
- Progress indicator (1/4, 2/4, 3/4, 4/4)
- Can navigate back to edit any step
- Photos compressed automatically (<500KB each)
- GPS auto-captured (optional, no error if failed)
- Works 100% offline
- Success feedback: "Laporan berhasil disimpan" + redirect to report detail

**Edge Cases:**
- User exits app on Step 2 → draft saved → on return, show: "Lanjutkan draft dari 5 menit lalu?"
- Max 10 photos → disable upload buttons when limit reached
- GPS permission denied → just show "Lokasi tidak tersedia" (no popup)
- Network fails during step → no problem, save local

---

### US-5.2: View Report Detail

**What User Wants:**
Sales rep/manager ingin view report detail sehingga dapat review visit information.

**Key UI Elements:**
- Top app bar: "Detail Laporan" + back + share icon (optional)
- Sync status badge (top right):
  - "Tersinkronisasi" (green check)
  - "Menyinkronkan..." (blue spinner)
  - "Gagal sync" (red X + retry button)
- Report sections:
  - Proyek (company + project name, link to project detail)
  - Detail Laporan (type, date, attendees, notes, next action, outcome)
  - Foto Gallery (horizontal scroll, tap untuk full screen)
  - Lokasi (map view jika GPS available, atau "Lokasi tidak tersedia")
  - Metadata (created by, created date, sync status)

**UX Requirements:**
- Manager has read-only access (NO edit button)
- Sales rep has edit button (jika own report) - but edit NOT in MVP, so NO edit button for now
- Photos open in full screen gallery on tap
- GPS location shown on static map (not interactive)

---

## 📱 EPIC 6: Dashboard & Reports List

### US-6.1: View My Reports (Sales Rep)

**What User Wants:**
Sales rep ingin melihat list semua reports yang telah dibuat sehingga dapat review past visits.

**Key UI Elements:**
- Filter chips:
  - Tanggal (date range picker)
  - Perusahaan (multi-select dropdown)
  - Status Sync (All | Synced | Pending | Failed)
- Sort dropdown (Terbaru | Terlama | Perusahaan A-Z)
- Report cards:
  - Company + Project name (title)
  - Tanggal kunjungan (subtitle)
  - Tipe laporan (badge)
  - Sync status (icon + badge)
  - Thumbnail foto pertama (jika ada)
- FAB "+"
- Pagination (20 reports per page)

**UX Requirements:**
- Default sort: Newest first
- Empty state jika belum ada reports
- Filter/search updates list immediately
- Pull-to-refresh triggers sync

---

### US-6.2: View Team Reports (Manager)

**What User Wants:**
Manager ingin melihat ALL reports dari ALL sales reps sehingga dapat monitor team activities.

**Key UI Elements:**
- Filter:
  - Sales Rep (multi-select: All | Rep A | Rep B)
  - Date range
  - Company
  - Report type
- List grouped by sales rep atau by date
- Report cards (sama seperti My Reports List + sales rep name tag)
- NO FAB (manager cannot create reports)

**UX Requirements:**
- Manager sees ALL team reports (read-only)
- Default: show all sales reps
- Can filter by specific sales rep
- Empty state (shouldn't happen, but design anyway)

---

## 📱 EPIC 7: Sync & Settings

### US-7.1: Offline Create/Edit

**What User Wants:**
Sales rep ingin create/edit data secara offline sehingga tidak perlu menunggu internet untuk bekerja.

**UX Requirements (affects ALL screens):**
- All create operations work offline
- All edit operations work offline
- Data saved to local SQLite immediately
- No blocking "waiting for network" messages
- Offline indicator di UI (top app bar or banner)
- Data will sync automatically when online

---

### US-7.2: Auto Sync When Online

**What User Wants:**
User ingin data auto-sync saat online sehingga tidak perlu manual sync every time.

**Key UI Elements (affects ALL screens):**
- Sync status indicator per item di lists
- Background sync (happens automatically, not blocking UI)
- Sync failed notification (if any item failed to sync)

**UX Requirements:**
- Auto-sync every 5 minutes when online (background)
- Sync order: Companies → Contacts → Projects → Reports → Photos
- Per-photo sync status tracking (see TECHNICAL_CONSTRAINTS.md)
- Retry logic: 3 attempts dengan exponential backoff
- If 3 attempts failed → show notification + manual retry option

---

### US-7.3: Manual Sync Button

**What User Wants:**
User ingin manual sync button sehingga dapat force sync kapan saja (tidak tunggu auto-sync).

**Key UI Elements:**
- "Sinkronkan Sekarang" button di Sync Status Screen
- Shows progress: "Menyinkronkan 5 dari 10 item..."
- Last sync time: "Terakhir disinkronisasi 5 menit yang lalu"
- Pull-to-refresh pada list views triggers sync

**UX Requirements:**
- Button shows spinner while syncing (disable button)
- Success feedback: "Semua data tersinkronisasi" (green snackbar)
- Failed feedback: "5 item gagal sync. Tap untuk lihat detail." (red snackbar + link)

---

## 📍 Navigasi

**Sudah paham user stories & requirements?**

➡️ **[Lanjut ke: DESIGN_REQUIREMENTS.md →](./DESIGN_REQUIREMENTS.md)**

Dokumen selanjutnya akan menjelaskan:
- Detailed specs untuk states (empty, error, loading)
- Component specifications
- Accessibility requirements

**Atau kembali ke:**
← [TECHNICAL_CONSTRAINTS](./TECHNICAL_CONSTRAINTS.md)

---

**Catatan:** Untuk full technical user stories dengan database specs, API contracts, dan implementation details, lihat `../USER_STORIES.md` (dokumen developer, 30+ pages). Tapi sebagai designer, document ini sudah cukup untuk start designing.
