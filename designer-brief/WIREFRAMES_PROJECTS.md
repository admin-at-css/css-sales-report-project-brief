# Wireframes - Projects Screens

← [Sebelumnya: WIREFRAMES_CONTACTS.md](./WIREFRAMES_CONTACTS.md)

---

## Overview

Dokumen ini berisi detailed ASCII wireframes untuk **EPIC 4: Manage Projects** (5 screens).

**Total Wireframes:** 10 wireframes
- Screen 13: Projects List (2 states)
- Screen 14: Project Detail (1 wireframe with Create Report button)
- Screen 15: Create Project Form (3 states)
- Screen 16: Edit Project Form (1 wireframe)
- Screen 17: Project Search/Filter Results (2 states)

---

## Design Specifications

### Platform & Dimensions
- **Platform:** Android Mobile
- **Screen Size:** 360 × 800 dp
- **Orientation:** Portrait only
- **Grid System:** 8dp base unit

### Typography
- **Title Large:** 22sp, Medium weight
- **Title Medium:** 16sp, Medium weight
- **Body Large:** 16sp, Regular weight
- **Body Medium:** 14sp, Regular weight

### Colors
- **Primary:** #2E7D32 (green)
- **Status Colors:**
  - Active: #1976D2 (blue)
  - Won: #4CAF50 (green)
  - Lost: #D32F2F (red)
  - On Hold: #757575 (gray)

---

## Screen 13: Projects List (per Company)

### User Story
[US-4.1: Lihat List Projects per Company](../developer-brief/USER_STORIES.md#us-41-lihat-list-projects-per-company)

### Tujuan
Menampilkan daftar projects untuk perusahaan tertentu. Ditampilkan dalam tab "Proyek" di Company Detail screen.

---

### Wireframe 1: Projects List - Loaded State

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  PT Surya Indah              ✏️       ┃ ← Company Detail App Bar
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃
┃                                            ┃
┃  INFORMASI PERUSAHAAN                      ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  PT Surya Indah • Jakarta           │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌─────────────────┬─────────────────────┐ ┃ ← Tabs
┃  │   KONTAK (3)    │    PROYEK (2)       │ ┃   PROYEK active
┃  └─────────────────┴─────────────────────┘ ┃
┃                                            ┃
┃  ┌──────────────────────────┬───────────┐  ┃ ← Filter dropdown
┃  │ Filter: Semua Status     │     ▼     │  ┃   Height: 48dp
┃  └──────────────────────────┴───────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Renovasi Pabrik Sidoarjo            │  ┃ ← Project Card
┃  │  [ACTIVE]    Rp 500,000,000          │  ┃   Name + status + value
┃  │  Closing: 31 Mar 2025                │  ┃   Expected close date
┃  └──────────────────────────────────────┘  ┃   Height: 88dp
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Proyek Marine Coating Kapal         │  ┃
┃  │  [WON]    Rp 1,200,000,000           │  ┃ ← WON badge (green)
┃  │  Closed: 15 Jan 2025                 │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                 ┌────┐     ┃
┃                                 │ +  │     ┃ ← FAB (add project)
┃                                 └────┘     ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Dimensi:**
- Filter dropdown: Lebar penuh minus 32dp, tinggi 48dp
- Project card: Lebar penuh minus 32dp, tinggi 88dp
- Card padding: 16dp all sides
- Status badge: Tinggi 20dp, padding 4dp horizontal

**Elemen Project Card:**
- Baris 1: Nama project (Title Medium, 16sp, bold)
- Baris 2: Status badge + Nilai estimasi
  - Badge colors:
    - ACTIVE: Blue background (#E3F2FD), blue text (#1976D2)
    - WON: Green background (#E8F5E9), green text (#4CAF50)
    - LOST: Red background (#FFEBEE), red text (#D32F2F)
    - ON HOLD: Gray background (#F5F5F5), gray text (#757575)
  - Nilai: Rp format dengan thousand separators
- Baris 3: Expected close date atau "Closed" date

**Filter Options:**
- Semua Status
- Active
- Won
- Lost
- On Hold

**Interaksi:**
- Tap filter dropdown → Show bottom sheet dengan 5 options
- Tap project card → Navigate ke Project Detail (Screen 14)
- Tap FAB (+) → Navigate ke Create Project Form (Screen 15)

---

### Wireframe 2: Projects List - Empty State

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  PT Surya Indah              ✏️       ┃
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃
┃                                            ┃
┃  INFORMASI PERUSAHAAN                      ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  PT Surya Indah • Jakarta           │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌─────────────────┬─────────────────────┐ ┃
┃  │   KONTAK (3)    │    PROYEK (0)       │ ┃
┃  └─────────────────┴─────────────────────┘ ┃
┃                                            ┃
┃                                            ┃
┃              ┌─────────┐                   ┃ ← Empty state
┃              │         │                   ┃   120×120dp
┃              │  📁     │                   ┃   Gray-400
┃              │         │                   ┃
┃              └─────────┘                   ┃
┃                                            ┃
┃     Belum ada proyek untuk perusahaan ini  ┃ ← Title Medium (16sp)
┃                                            ┃
┃     Tap tombol + untuk menambahkan         ┃ ← Body Medium (14sp)
┃     proyek pertama                         ┃   Secondary color
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                 ┌────┐     ┃
┃                                 │ +  │     ┃
┃                                 └────┘     ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

---

## Screen 14: Project Detail

### User Story
[US-4.1: Lihat List Projects per Company](../developer-brief/USER_STORIES.md#us-41-lihat-list-projects-per-company), [US-4.5: Buat Report dari Project Detail](../developer-brief/USER_STORIES.md#us-45-buat-report-dari-project-detail-new)

### Tujuan
Menampilkan informasi lengkap project dan action button untuk create report.

---

### Wireframe 3: Project Detail - Default View

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Renovasi Pabrik Sidoarjo    ✏️       ┃ ← Top App Bar
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃   Project name + edit
┃                                            ┃
┃  INFORMASI PROYEK                          ┃ ← Section header
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Nama Proyek                         │  ┃
┃  │  Renovasi Pabrik Sidoarjo            │  ┃
┃  │                                      │  ┃
┃  │  Tipe Proyek                         │  ┃
┃  │  Industrial                          │  ┃
┃  │                                      │  ┃
┃  │  Segmentasi                          │  ┃
┃  │  • Protective Coating                │  ┃ ← Chips/bullets
┃  │  • Floor Coating                     │  ┃
┃  │                                      │  ┃
┃  │  Sumber                              │  ┃
┃  │  Canvassing                          │  ┃
┃  │                                      │  ┃
┃  │  Status                              │  ┃
┃  │  [ACTIVE]                            │  ┃ ← Status badge
┃  │                                      │  ┃
┃  │  Nilai Estimasi                      │  ┃
┃  │  Rp 500,000,000                      │  ┃
┃  │                                      │  ┃
┃  │  Tanggal Penutupan                   │  ┃
┃  │  31 Mar 2025                         │  ┃
┃  │                                      │  ┃
┃  │  Kontak Utama                        │  ┃
┃  │  Budi Santoso (Manager) →            │  ┃ ← Link to contact
┃  │                                      │  ┃
┃  │  Perusahaan                          │  ┃
┃  │  PT Surya Indah →                    │  ┃ ← Link to company
┃  │                                      │  ┃
┃  │  Dibuat oleh                         │  ┃
┃  │  Budi Wijaya • 10 Jan 2025           │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  BUAT LAPORAN UNTUK PROJECT INI      │  ┃ ← Fixed button (US-4.5)
┃  └──────────────────────────────────────┘  ┃   Primary button, 48dp
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛   Fixed at bottom
```

**Dimensi:**
- Info card: Lebar penuh minus 32dp, padding 16dp
- Fixed button: Lebar penuh minus 32dp, tinggi 48dp, 16dp dari bottom

**Informasi Project:**
- Nama Proyek
- Tipe: Architectural | Industrial | Infrastructure | Marine
- Segmentasi (multi-select): Decorative, Protective Coating, Floor Coating, Marine Coating
- Sumber: Canvassing, Referral from Customer, Referral from Partner, Website, Social Media
- Status badge (color-coded)
- Nilai Estimasi (Rp format)
- Tanggal Penutupan (expected)
- Kontak Utama (link)
- Perusahaan (link)
- Dibuat oleh + tanggal

**Interaksi:**
- Tap edit icon (✏️) → Navigate ke Edit Project Form (Screen 16)
- Tap "Kontak Utama" → Navigate ke Contact Detail
- Tap "Perusahaan" → Navigate ke Company Detail
- Tap "BUAT LAPORAN" → Open bottom sheet dengan report type picker

---

## Screen 15: Create Project Form

### User Story
[US-4.2: Buat Project Baru](../developer-brief/USER_STORIES.md#us-42-buat-project-baru)

### Tujuan
Memungkinkan sales rep membuat project baru untuk perusahaan tertentu.

---

### Wireframe 4: Create Project - Empty Form

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Tambah Proyek               💾       ┃ ← Top App Bar
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Nama Proyek *                       │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃
┃  │  │                                │  │  ┃
┃  │  └────────────────────────────────┘  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Tipe Proyek *                       │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃
┃  │  │ Pilih tipe proyek          ▼   │  │  ┃ ← Dropdown
┃  │  └────────────────────────────────┘  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Segmentasi *                        │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃
┃  │  │ Pilih segmentasi           ▼   │  │  ┃ ← Multi-select
┃  │  └────────────────────────────────┘  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Sumber *                            │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃
┃  │  │ Pilih sumber               ▼   │  │  ┃ ← Dropdown
┃  │  └────────────────────────────────┘  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Status *                            │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃
┃  │  │ Active                     ▼   │  │  ┃ ← Dropdown (default)
┃  │  └────────────────────────────────┘  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  (scroll for more fields below)            ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Form Fields (Part 1):**
1. **Nama Proyek*** - Text input, max 255 characters
2. **Tipe Proyek*** - Dropdown:
   - Architectural
   - Industrial
   - Infrastructure
   - Marine
3. **Segmentasi*** - Multi-select (chips):
   - Decorative
   - Protective Coating
   - Floor Coating
   - Marine Coating
4. **Sumber*** - Dropdown:
   - Canvassing
   - Referral from Customer
   - Referral from Partner
   - Website
   - Social Media
5. **Status*** - Dropdown (default: Active):
   - Active
   - Won
   - Lost
   - On Hold

---

### Wireframe 5: Create Project - Scrolled (More Fields)

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Tambah Proyek               💾       ┃
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃
┃                                            ┃
┃  (scroll up for fields above)              ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Nilai Estimasi *                    │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃
┃  │  │ Rp                             │  │  ┃ ← Currency input
┃  │  └────────────────────────────────┘  │  ┃   Auto-format dengan ","
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Tanggal Penutupan                   │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃
┃  │  │                            📅  │  │  ┃ ← Date picker
┃  │  └────────────────────────────────┘  │  ┃   Optional
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Kontak Utama *                      │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃
┃  │  │ Pilih kontak               ▼   │  │  ┃ ← Dropdown
┃  │  └────────────────────────────────┘  │  ┃   List contacts dari
┃  └──────────────────────────────────────┘  ┃   company ini only
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃  Terakhir disimpan 1 menit yang lalu       ┃ ← Auto-save indicator
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  BATAL              SIMPAN           │  ┃ ← Bottom actions
┃  └──────────────────────────────────────┘  ┃   SIMPAN disabled (gray)
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Form Fields (Part 2):**
6. **Nilai Estimasi*** - Currency input
   - Format: Rp dengan thousand separator (,)
   - Example: Rp 500,000,000
   - Real-time formatting saat typing
7. **Tanggal Penutupan** - Date picker (optional)
   - Opens calendar dialog
8. **Kontak Utama*** - Dropdown
   - List semua contacts dari company ini
   - Harus pilih 1 contact

**Validasi:**
- Semua fields dengan (*) required
- Nilai Estimasi: Minimum Rp 1,000,000
- Kontak Utama: Harus ada minimal 1 contact di company (jika tidak ada, user harus create contact dulu)

**State:**
- SIMPAN button: Disabled sampai semua required fields valid
- Auto-save: Setiap 30 detik

---

### Wireframe 6: Create Project - Segmentasi Multi-Select

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Tambah Proyek               💾       ┃
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Nama Proyek *                       │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃
┃  │  │ Renovasi Pabrik Sidoarjo       │  │  ┃
┃  │  └────────────────────────────────┘  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Tipe Proyek *                       │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃
┃  │  │ Industrial                     │  │  ┃
┃  │  └────────────────────────────────┘  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Segmentasi *                        │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃
┃  │  │ 2 dipilih                  ▲   │  │  ┃ ← Multi-select (expanded)
┃  │  └────────────────────────────────┘  │  ┃
┃  │                                      │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃ ← Chip list (selected)
┃  │  │  Protective Coating      ✕    │  │  ┃   Chips closeable
┃  │  │  Floor Coating           ✕    │  │  ┃
┃  │  └────────────────────────────────┘  │  ┃
┃  │                                      │  ┃
┃  │  Available options:                  │  ┃ ← Available list
┃  │  ☐ Decorative                        │  ┃   Checkboxes
┃  │  ☑ Protective Coating                │  ┃
┃  │  ☑ Floor Coating                     │  ┃
┃  │  ☐ Marine Coating                    │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  (scroll for more fields)                  ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Multi-Select Behavior:**
- Tap dropdown → Expand untuk show checkboxes
- Check option → Add chip ke selected area
- Tap ✕ pada chip → Remove selection
- Minimum 1 selection required
- Selected chips: Green background (#E8F5E9), green text

---

## Screen 16: Edit Project Form

### User Story
[US-4.3: Edit Project](../developer-brief/USER_STORIES.md#us-43-edit-project)

### Tujuan
Memungkinkan sales rep mengedit project yang sudah ada.

---

### Wireframe 7: Edit Project - Pre-filled Form

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Edit Proyek                 💾       ┃ ← Top App Bar
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃   Title: "Edit Proyek"
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Nama Proyek *                       │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃
┃  │  │ Renovasi Pabrik Sidoarjo       │  │  ┃ ← Pre-filled
┃  │  └────────────────────────────────┘  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Tipe Proyek *                       │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃
┃  │  │ Industrial                 ▼   │  │  ┃
┃  │  └────────────────────────────────┘  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Segmentasi *                        │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃
┃  │  │  Protective Coating      ✕    │  │  ┃ ← Pre-selected chips
┃  │  │  Floor Coating           ✕    │  │  ┃
┃  │  └────────────────────────────────┘  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  (scroll for more fields)                  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Nilai Estimasi *                    │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃
┃  │  │ Rp 500,000,000                 │  │  ┃ ← Pre-filled value
┃  │  └────────────────────────────────┘  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  (more fields...)                          ┃
┃                                            ┃
┃  Terakhir disimpan baru saja               ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  BATAL              SIMPAN           │  ┃
┃  └──────────────────────────────────────┘  ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Catatan:**
- Sama dengan Create Project Form, tapi semua fields pre-filled dengan data existing
- Validasi sama seperti Create

---

## Screen 17: Project Search/Filter Results

### User Story
[US-4.1: Lihat List Projects per Company](../developer-brief/USER_STORIES.md#us-41-lihat-list-projects-per-company)

### Tujuan
Menampilkan projects yang difilter berdasarkan status.

---

### Wireframe 8: Filter Results - Active Only

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  PT Surya Indah              ✏️       ┃
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃
┃                                            ┃
┃  INFORMASI PERUSAHAAN                      ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  PT Surya Indah • Jakarta           │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌─────────────────┬─────────────────────┐ ┃
┃  │   KONTAK (3)    │    PROYEK (2)       │ ┃
┃  └─────────────────┴─────────────────────┘ ┃
┃                                            ┃
┃  ┌──────────────────────────┬───────────┐  ┃
┃  │ Filter: Active           │     ▼     │  ┃ ← Filter selected
┃  └──────────────────────────┴───────────┘  ┃
┃                                            ┃
┃  1 proyek ditemukan                        ┃ ← Result count
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Renovasi Pabrik Sidoarjo            │  ┃
┃  │  [ACTIVE]    Rp 500,000,000          │  ┃
┃  │  Closing: 31 Mar 2025                │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                 ┌────┐     ┃
┃                                 │ +  │     ┃
┃                                 └────┘     ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

---

### Wireframe 9: Filter Results - No Results

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  PT Surya Indah              ✏️       ┃
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃
┃                                            ┃
┃  INFORMASI PERUSAHAAN                      ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  PT Surya Indah • Jakarta           │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌─────────────────┬─────────────────────┐ ┃
┃  │   KONTAK (3)    │    PROYEK (2)       │ ┃
┃  └─────────────────┴─────────────────────┘ ┃
┃                                            ┃
┃  ┌──────────────────────────┬───────────┐  ┃
┃  │ Filter: Lost             │     ▼     │  ┃ ← Filter: Lost
┃  └──────────────────────────┴───────────┘  ┃
┃                                            ┃
┃              ┌─────────┐                   ┃
┃              │         │                   ┃
┃              │  🔍     │                   ┃ ← Empty state
┃              │         │                   ┃
┃              └─────────┘                   ┃
┃                                            ┃
┃     Tidak ada proyek dengan status Lost    ┃ ← Dynamic message
┃                                            ┃
┃     Coba filter lain atau buat proyek baru ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                 ┌────┐     ┃
┃                                 │ +  │     ┃
┃                                 └────┘     ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

---

## Design System Notes

### Material Design 3 Components

**Projects List:**
- Filter dropdown: Exposed dropdown menu
- Cards: Elevated card (2dp)
- Status badges: Custom chip component

**Project Detail:**
- Info card: Outlined card
- Accordion: Expandable list item
- Fixed button: Material 3 filled button

**Form Screens:**
- Dropdowns: Material 3 exposed dropdown menu
- Multi-select: Custom chip input + checkbox list
- Currency input: Text field dengan custom formatting
- Date picker: Material 3 date picker dialog

### Accessibility

- Status badges: Color + text (tidak hanya color)
- Currency input: Clear format guidance
- Multi-select: Keyboard navigable
- Filter: Screen reader announces result count

---

## Implementation Notes

### Projects List
1. **Default Filter:** "Semua Status"
2. **Filter Persistence:** Remember last filter selection per user
3. **Sorting:** Within filter, sort by expected close date (ascending)

### Project Detail
1. **Create Report Button (US-4.5):**
   - Only visible untuk sales rep (not manager)
   - Fixed position at bottom
   - Tap → Open bottom sheet dengan 5 report types (NO "Initial Visit")
   - See NESTED_INLINE_CREATION_WIREFRAMES.md wireframes 18-22

### Form Screens
1. **Currency Input:**
   - Real-time formatting: "500000000" → "Rp 500,000,000"
   - Store as integer di database (remove "Rp" dan ",")
   - Min value: Rp 1,000,000
   - Max value: Rp 999,999,999,999 (999 billion)

2. **Segmentasi Multi-Select:**
   - Min 1 selection required
   - Max 4 selections (all options)
   - Store as JSON array: ["Protective Coating", "Floor Coating"]

3. **Kontak Utama Dropdown:**
   - Only show contacts dari company ini
   - Jika company tidak punya contacts → Show message: "Buat kontak dulu"
   - Disable SIMPAN button jika tidak ada contacts available

---

## Related Documents

- **Screen Inventory:** [SCREEN_INVENTORY.md](./SCREEN_INVENTORY.md) (Screens 13-17)
- **User Stories:** [USER_STORIES.md](../developer-brief/USER_STORIES.md) (US-4.1, US-4.2, US-4.3, US-4.5)
- **Design Requirements:** [DESIGN_REQUIREMENTS.md](./DESIGN_REQUIREMENTS.md)
- **Nested Inline Creation:** [NESTED_INLINE_CREATION_WIREFRAMES.md](./NESTED_INLINE_CREATION_WIREFRAMES.md) (Wireframes 18-22)

---

## Next Steps

➡️ **[Continue to: WIREFRAMES_REPORTS.md →](./WIREFRAMES_REPORTS.md)**

**Or navigate to:**
← [WIREFRAMES_CONTACTS.md](./WIREFRAMES_CONTACTS.md)
