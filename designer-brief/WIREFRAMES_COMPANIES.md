# Wireframes - Companies Screens

← [Sebelumnya: WIREFRAMES_SYNC_SETTINGS.md](./WIREFRAMES_SYNC_SETTINGS.md)

---

## Overview

Dokumen ini berisi detailed ASCII wireframes untuk **EPIC 2: Manage Companies** (5 screens).

**Total Wireframes:** 13 wireframes
- Screen 3: Companies List (4 states)
- Screen 4: Company Detail (2 states)
- Screen 5: Create Company Form (4 states)
- Screen 6: Edit Company Form (1 wireframe)
- Screen 7: Company Search Results (2 states)

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
- **Label Large:** 14sp, Medium weight
- **Label Medium:** 12sp, Regular weight

### Colors
- **Primary:** #2E7D32 (green)
- **Text Primary:** #212121 (gray-900)
- **Text Secondary:** #757575 (gray-600)
- **Surface:** #FFFFFF (white)
- **Background:** #F5F5F5 (gray-100)
- **Divider:** #E0E0E0 (gray-300)

---

## Screen 3: Companies List

### User Story
[US-2.1: Lihat List Companies](../developer-brief/USER_STORIES.md#us-21-lihat-list-companies)

### Tujuan
Menampilkan daftar perusahaan. Sales rep melihat perusahaan yang dimilikinya, Manager melihat semua perusahaan. Mendukung search dan filter.

---

### Wireframe 1: Companies List - Empty State

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ☰  Perusahaan               🔍  ⋮       ┃ ← Top App Bar
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃   Menu, search, more options
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃              ┌─────────┐                   ┃ ← Centered illustration
┃              │         │                   ┃   120×120dp
┃              │  📊     │                   ┃   Gray-400 color
┃              │         │                   ┃
┃              └─────────┘                   ┃
┃                                            ┃
┃         Belum ada perusahaan               ┃ ← Title Medium (16sp)
┃                                            ┃   Color: Text Primary
┃     Tap tombol + untuk menambahkan         ┃ ← Body Medium (14sp)
┃     perusahaan pertama                     ┃   Color: Text Secondary
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                 ┌────┐     ┃ ← FAB (Floating Action Button)
┃                                 │ +  │     ┃   56×56dp
┃                                 └────┘     ┃   Background: Primary
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃   16dp from bottom/right
┃   🏠    📋    🏢    👤                     ┃ ← Bottom Navigation Bar
┃ Beranda Laporan Perusahaan Profil         ┃   Height: 56dp
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛   "Perusahaan" active (bold)
```

**Dimensions:**
- Top App Bar: Height 56dp
- Empty state illustration: 120×120dp, centered
- Text spacing: 16dp below illustration
- FAB: 56×56dp, 16dp from bottom-right corner
- Bottom Nav: Height 56dp

**State:**
- No companies in database (empty list)
- FAB visible (sales rep can create)
- Bottom Nav: "Perusahaan" tab active (green indicator)

**Interactions:**
- Tap menu (☰) → Open navigation drawer
- Tap search (🔍) → Expand search bar
- Tap more (⋮) → Show options menu (filter, sort)
- Tap FAB (+) → Navigate to Create Company Form (Screen 5)
- Tap Bottom Nav tabs → Navigate to other screens

---

### Wireframe 2: Companies List - Loaded State

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ☰  Perusahaan               🔍  ⋮       ┃
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃
┃                                            ┃ ← Pull-to-refresh area
┃  ┌──────────────────────────────────────┐  ┃
┃  │  PT Surya Indah                      │  ┃ ← Company Card (elevated)
┃  │  Jl. Sudirman No. 123, Jakarta...    │  ┃   Title: Title Medium (16sp)
┃  │  3 kontak  •  2 proyek               │  ┃   Address: Body Medium (14sp)
┃  └──────────────────────────────────────┘  ┃   Meta: Label Medium (12sp)
┃                                            ┃   Height: 88dp
┃  ┌──────────────────────────────────────┐  ┃   Margin: 8dp vertical
┃  │  PT Maju Jaya                        │  ┃
┃  │  Jl. Gatot Subroto No. 45, Bandu...  │  ┃
┃  │  1 kontak  •  1 proyek               │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  PT Karya Bersama                    │  ┃
┃  │  Jl. Ahmad Yani No. 67, Surabaya     │  ┃
┃  │  5 kontak  •  4 proyek               │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  PT Sentosa Utama                    │  ┃
┃  │  Jl. Diponegoro No. 89, Semarang     │  ┃
┃  │  2 kontak  •  3 proyek               │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  PT Cahaya Mas                       │  ┃
┃  │  Jl. Veteran No. 12, Medan           │  ┃
┃  │  4 kontak  •  1 proyek               │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                 ┌────┐     ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │      MUAT LEBIH BANYAK               │  ┃ ← Load more button
┃  └──────────────────────────────────────┘  ┃   Outlined button, 48dp
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃
┃   🏠    📋    🏢    👤                     ┃
┃ Beranda Laporan Perusahaan Profil         ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Dimensions:**
- Company card: Full width minus 32dp (16dp margins), height 88dp
- Card padding: 16dp all sides
- Card elevation: 2dp
- Card corner radius: 8dp
- Vertical spacing between cards: 8dp
- Load more button: Height 48dp, 16dp bottom margin

**Company Card Elements:**
- Line 1: Company name (Title Medium, 16sp, bold)
- Line 2: Address (Body Medium, 14sp, truncated with "...")
- Line 3: Metadata (Label Medium, 12sp, secondary color)
  - Format: "X kontak • Y proyek"

**Pagination:**
- Shows 20 companies per page
- "MUAT LEBIH BANYAK" button at bottom
- Or implement infinite scroll (load more on scroll)

**Interactions:**
- Tap company card → Navigate to Company Detail (Screen 4)
- Pull down → Refresh list (reload from server)
- Scroll to bottom → Auto-load next page (if infinite scroll)
- Tap "MUAT LEBIH BANYAK" → Load next 20 companies

---

### Wireframe 3: Companies List - Loading State (Shimmer)

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ☰  Perusahaan               🔍  ⋮       ┃
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  ████████████░░░░░░░░░░              │  ┃ ← Shimmer placeholder
┃  │  ████████████████████████░░░░░░      │  ┃   Animated gradient
┃  │  ████░░░░░  ████░░░░░                │  ┃   Gray-200 → Gray-300
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  ████████████░░░░░░░░░░              │  ┃
┃  │  ████████████████████████░░░░░░      │  ┃
┃  │  ████░░░░░  ████░░░░░                │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  ████████████░░░░░░░░░░              │  ┃
┃  │  ████████████████████████░░░░░░      │  ┃
┃  │  ████░░░░░  ████░░░░░                │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  ████████████░░░░░░░░░░              │  ┃
┃  │  ████████████████████████░░░░░░      │  ┃
┃  │  ████░░░░░  ████░░░░░                │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  ████████████░░░░░░░░░░              │  ┃
┃  │  ████████████████████████░░░░░░      │  ┃
┃  │  ████░░░░░  ████░░░░░                │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                 ┌────┐     ┃
┃                                 │ +  │     ┃
┃                                 └────┘     ┃
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃
┃   🏠    📋    🏢    👤                     ┃
┃ Beranda Laporan Perusahaan Profil         ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Shimmer Animation:**
- Gradient sweeps left to right (1000ms duration, repeat)
- Colors: Gray-200 (#EEEEEE) → Gray-300 (#E0E0E0)
- Placeholder rectangles mimic actual content layout
- 5-6 placeholder cards visible

**Duration:**
- Show shimmer during initial load (0-3 seconds)
- Replace with actual data when loaded
- If error → Show error state (not shown in wireframe)

---

### Wireframe 4: Companies List - Search Active

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  ┌────────────────────────────┐  X   ┃ ← Search expanded
┃     │ surya                      │       ┃   Back button, search input, clear
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃
┃                                            ┃
┃  1 perusahaan ditemukan                    ┃ ← Result count
┃                                            ┃   Body Medium, secondary color
┃  ┌──────────────────────────────────────┐  ┃   16dp top margin
┃  │  PT Surya Indah                      │  ┃
┃  │  Jl. Sudirman No. 123, Jakarta...    │  ┃ ← Highlighted match
┃  │  3 kontak  •  2 proyek               │  ┃   "surya" in bold
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
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
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃
┃   🏠    📋    🏢    👤                     ┃
┃ Beranda Laporan Perusahaan Profil         ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Search Behavior:**
- Tap search icon (🔍) → Expand to full-width search bar
- Search bar replaces app bar title
- Shows back arrow (dismiss search) and X (clear text)
- Real-time filtering as user types
- Result count shows: "X perusahaan ditemukan"
- Matching text highlighted in results

**Search Scope:**
- Company name (primary)
- Address (secondary)
- City (secondary)

**Interactions:**
- Type text → Filter list in real-time (debounce 300ms)
- Tap X → Clear search text
- Tap back arrow → Exit search, return to full list
- Tap company → Navigate to detail (same as normal)

---

## Screen 4: Company Detail

### User Story
[US-2.1: Lihat List Companies](../developer-brief/USER_STORIES.md#us-21-lihat-list-companies)

### Tujuan
Menampilkan informasi lengkap perusahaan dengan tabs untuk Contacts dan Projects.

---

### Wireframe 5: Company Detail - Contacts Tab

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  PT Surya Indah              ✏️       ┃ ← Top App Bar
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃   Company name + edit icon
┃                                            ┃
┃  INFORMASI PERUSAHAAN                      ┃ ← Section header
┃  ┌──────────────────────────────────────┐  ┃   Label Medium (12sp)
┃  │  Nama                                │  ┃
┃  │  PT Surya Indah                      │  ┃ ← Read-only fields
┃  │                                      │  ┃   Label + value pairs
┃  │  Alamat                              │  ┃   16dp padding
┃  │  Jl. Sudirman No. 123, Kel. Karet,   │  ┃
┃  │  Kec. Setiabudi, Jakarta Selatan     │  ┃
┃  │                                      │  ┃
┃  │  Kota                                │  ┃
┃  │  Jakarta                             │  ┃
┃  │                                      │  ┃
┃  │  Dibuat oleh                         │  ┃
┃  │  Budi Wijaya                         │  ┃
┃  │                                      │  ┃
┃  │  Dibuat pada                         │  ┃
┃  │  12 Jan 2025, 10:30                  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌─────────────────┬─────────────────────┐ ┃ ← Tabs
┃  │  KONTAK (3)     │     PROYEK (2)      │ ┃   Height: 48dp
┃  └─────────────────┴─────────────────────┘ ┃   Active tab: underline
┃                                            ┃   (KONTAK active)
┃  ┌──────────────────────────────────────┐  ┃
┃  │  👤  Budi Santoso  [PRIMARY CONTACT] │  ┃ ← Contact card
┃  │      Manager Procurement             │  ┃   Name + badge
┃  │      📞 0812-3456-7890               │  ┃   Position, phone
┃  └──────────────────────────────────────┘  ┃   Height: 88dp
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  👤  Siti Aminah                     │  ┃
┃  │      Direktur Operasional            │  ┃
┃  │      📞 0813-9876-5432               │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                 ┌────┐     ┃
┃                                 │ +  │     ┃ ← FAB (add contact)
┃                                 └────┘     ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Dimensions:**
- Info card: Full width minus 32dp, padding 16dp
- Label: Label Medium (12sp), secondary color
- Value: Body Large (16sp), primary color
- Tabs: Height 48dp, equal width
- Contact card: Height 88dp, padding 16dp
- Badge: Height 20dp, padding 4dp horizontal

**Company Info Fields:**
- Nama (required)
- Alamat (optional, multiline)
- Kota (optional)
- Dibuat oleh (sales rep name, read-only)
- Dibuat pada (timestamp, read-only)

**Tabs:**
- KONTAK (X) - Shows contact count
- PROYEK (Y) - Shows project count
- Active tab: Underline indicator (primary color, 2dp height)

**Contact Card:**
- Icon: Material icon "person" (24dp)
- Name: Title Medium (16sp) + [PRIMARY CONTACT] badge if is_primary
- Position: Body Medium (14sp), secondary color
- Phone: Body Medium (14sp) with phone icon

**Interactions:**
- Tap edit icon (✏️) → Navigate to Edit Company Form (Screen 6)
- Tap tab → Switch between Kontak and Proyek views
- Tap contact card → Navigate to Contact Detail (Screen 9)
- Tap FAB (+) → Navigate to Create Contact Form (Screen 10)

---

### Wireframe 6: Company Detail - Projects Tab

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  PT Surya Indah              ✏️       ┃
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃
┃                                            ┃
┃  INFORMASI PERUSAHAAN                      ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Nama                                │  ┃
┃  │  PT Surya Indah                      │  ┃
┃  │                                      │  ┃
┃  │  Alamat                              │  ┃
┃  │  Jl. Sudirman No. 123, Jakarta...    │  ┃
┃  │                                      │  ┃
┃  │  Kota: Jakarta  •  Dibuat: 12 Jan    │  ┃ ← Compressed view
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌─────────────────┬─────────────────────┐ ┃
┃  │   KONTAK (3)    │    PROYEK (2)       │ ┃
┃  └─────────────────┴─────────────────────┘ ┃ ← PROYEK active
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Renovasi Pabrik Sidoarjo            │  ┃ ← Project card
┃  │  [ACTIVE]  Rp 500,000,000            │  ┃   Name + status badge + value
┃  │  Closing: 31 Mar 2025                │  ┃   Expected close date
┃  └──────────────────────────────────────┘  ┃   Height: 88dp
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Proyek Marine Coating Kapal         │  ┃
┃  │  [WON]  Rp 1,200,000,000             │  ┃ ← Status: WON (green)
┃  │  Closed: 15 Jan 2025                 │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
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

**Project Card:**
- Line 1: Project name (Title Medium, 16sp)
- Line 2: Status badge + Estimated value
  - Status badges color-coded:
    - ACTIVE: Blue (#1976D2)
    - WON: Green (#4CAF50)
    - LOST: Red (#D32F2F)
    - ON HOLD: Gray (#757575)
- Line 3: Expected close date (or "Closed" if Won/Lost)

**Interactions:**
- Tap project card → Navigate to Project Detail (Screen 14)
- Tap FAB (+) → Navigate to Create Project Form (Screen 15)

---

## Screen 5: Create Company Form

### User Story
[US-2.2: Buat Company Baru](../developer-brief/USER_STORIES.md#us-22-buat-company-baru)

### Tujuan
Memungkinkan sales rep membuat perusahaan baru dengan form fields dan validation.

---

### Wireframe 7: Create Company - Empty Form

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Tambah Perusahaan           💾       ┃ ← Top App Bar
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃   Back + save icon
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃ ← 16dp top margin
┃  │  Nama Perusahaan *                   │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃ ← Text input (required)
┃  │  │                                │  │  ┃   Height: 56dp
┃  │  └────────────────────────────────┘  │  ┃   Asterisk indicates required
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Alamat                              │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃ ← Multiline text input
┃  │  │                                │  │  ┃   Height: 96dp (3 lines)
┃  │  │                                │  │  ┃   Optional field
┃  │  │                                │  │  ┃
┃  │  └────────────────────────────────┘  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Kota                                │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃ ← Text input (optional)
┃  │  │                                │  │  ┃   Height: 56dp
┃  │  └────────────────────────────────┘  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃ ← Bottom action bar
┃  │  BATAL              SIMPAN           │  ┃   Fixed at bottom
┃  └──────────────────────────────────────┘  ┃   SIMPAN disabled (gray)
┃                                            ┃   Height: 56dp
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛   Elevation: 4dp
```

**Dimensions:**
- Form fields: Full width minus 32dp (16dp margins)
- Single-line input: Height 56dp
- Multiline input (Alamat): Height 96dp (3 lines)
- Field spacing: 16dp vertical
- Bottom action bar: Height 56dp, fixed position

**Form Fields:**
1. **Nama Perusahaan*** (required)
   - Type: Text input
   - Max length: 255 characters
   - Validation: Cannot be empty

2. **Alamat** (optional)
   - Type: Multiline text input
   - Max length: 500 characters
   - 3 lines visible

3. **Kota** (optional)
   - Type: Text input
   - Max length: 100 characters

**Bottom Actions:**
- BATAL (secondary button, outlined)
- SIMPAN (primary button, filled)
  - Disabled when: Nama field empty
  - Enabled when: Nama filled (alamat/kota optional)

**Auto-save:**
- Draft saved to local SQLite every 30 seconds
- Show indicator: "Terakhir disimpan 1 menit yang lalu" (tiny text, 10sp, gray)
- Position: Above bottom bar, 8dp margin

**Interactions:**
- Tap field → Focus, show keyboard
- Type text → Enable SIMPAN button if nama filled
- Tap BATAL → Show discard dialog (if changes made)
- Tap SIMPAN → Validate, submit, navigate back
- Tap save icon (💾) → Same as SIMPAN button

---

### Wireframe 8: Create Company - Partially Filled

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Tambah Perusahaan           💾       ┃
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Nama Perusahaan *                   │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃
┃  │  │ PT Sejahtera Mandiri           │  │  ┃ ← Filled (has value)
┃  │  └────────────────────────────────┘  │  ┃   Border: Primary color
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Alamat                              │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃
┃  │  │ Jl. Pemuda No. 45, Kel. Sawah  │  │  ┃ ← Partially filled
┃  │  │ Besar, Kec. Gayamsari,         │  │  ┃   2 lines used
┃  │  │                                │  │  ┃   Cursor blinking
┃  │  └────────────────────────────────┘  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Kota                                │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃
┃  │  │                                │  │  ┃ ← Empty (optional)
┃  │  └────────────────────────────────┘  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃  Terakhir disimpan 30 detik yang lalu      ┃ ← Auto-save indicator
┃                                            ┃   Label Small (10sp), gray
┃  ┌──────────────────────────────────────┐  ┃
┃  │  BATAL              SIMPAN           │  ┃ ← SIMPAN now ENABLED
┃  └──────────────────────────────────────┘  ┃   Background: Primary green
┃                                            ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Changes from State 1:**
- Nama field: Filled dengan "PT Sejahtera Mandiri"
- Alamat field: Partially filled (2 lines)
- Kota field: Still empty (optional)
- SIMPAN button: ENABLED (nama is filled)
- Auto-save indicator visible: "Terakhir disimpan 30 detik yang lalu"

**Validation State:**
- Nama: Valid (not empty) ✓
- Alamat: Valid (optional, any value accepted) ✓
- Kota: Valid (optional, empty accepted) ✓
- Form: Valid, can submit

---

### Wireframe 9: Create Company - Validation Error

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Tambah Perusahaan           💾       ┃
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Nama Perusahaan *                   │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃
┃  │  │                                │  │  ┃ ← Empty field (error)
┃  │  └────────────────────────────────┘  │  ┃   Border: Red (#D32F2F)
┃  │  ⚠️ Nama perusahaan wajib diisi      │  ┃ ← Error message
┃  └──────────────────────────────────────┘  ┃   Body Small (12sp), red
┃                                            ┃   8dp below field
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Alamat                              │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃
┃  │  │                                │  │  ┃
┃  │  │                                │  │  ┃
┃  │  │                                │  │  ┃
┃  │  └────────────────────────────────┘  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Kota                                │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃
┃  │  │                                │  │  ┃
┃  │  └────────────────────────────────┘  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  BATAL              SIMPAN           │  ┃ ← SIMPAN disabled (gray)
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Validation Trigger:**
- User tapped SIMPAN with empty Nama field
- Or user focused Nama field then left it empty (on blur)

**Error Display:**
- Field border: Red (#D32F2F), 2dp width
- Error icon: Warning icon (⚠️), 16dp
- Error message: "Nama perusahaan wajib diisi"
  - Body Small (12sp), red color
  - Position: 8dp below field

**Interactions:**
- User starts typing in Nama → Error disappears
- Error persists until field has valid value

---

### Wireframe 10: Create Company - Saving State

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Tambah Perusahaan           💾       ┃
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Nama Perusahaan *                   │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃
┃  │  │ PT Sejahtera Mandiri           │  │  ┃ ← Fields DISABLED
┃  │  └────────────────────────────────┘  │  ┃   Opacity: 50%
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Alamat                              │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃
┃  │  │ Jl. Pemuda No. 45, Semarang    │  │  ┃
┃  │  │                                │  │  ┃
┃  │  │                                │  │  ┃
┃  │  └────────────────────────────────┘  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Kota                                │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃
┃  │  │ Semarang                       │  │  ┃
┃  │  └────────────────────────────────┘  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │              ⏳ Menyimpan...         │  ┃ ← Loading state
┃  └──────────────────────────────────────┘  ┃   Spinner + text
┃                                            ┃   Background: Lighter green
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Changes from State 2:**
- All fields: DISABLED (opacity 50%, no interaction)
- Bottom bar: Shows loading spinner + "Menyimpan..." text
- Background: Light green (#C5E1A5)
- No button clicks possible during save

**Duration:**
- Saving state: 1-3 seconds (API call time)
- If success → Navigate back to Companies List, show success snackbar
- If error → Show error message, re-enable form

---

## Screen 6: Edit Company Form

### User Story
[US-2.3: Edit Company](../developer-brief/USER_STORIES.md#us-23-edit-company)

### Tujuan
Memungkinkan sales rep mengedit perusahaan yang sudah ada (hanya perusahaan miliknya).

---

### Wireframe 11: Edit Company - Pre-filled Form

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Edit Perusahaan             💾       ┃ ← Top App Bar
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃   Title: "Edit Perusahaan"
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Nama Perusahaan *                   │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃
┃  │  │ PT Surya Indah                 │  │  ┃ ← Pre-filled with existing
┃  │  └────────────────────────────────┘  │  ┃   data from database
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Alamat                              │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃
┃  │  │ Jl. Sudirman No. 123, Kel.     │  │  ┃
┃  │  │ Karet, Kec. Setiabudi,         │  │  ┃
┃  │  │ Jakarta Selatan                │  │  ┃
┃  │  └────────────────────────────────┘  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Kota                                │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃
┃  │  │ Jakarta                        │  │  ┃
┃  │  └────────────────────────────────┘  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃  Terakhir disimpan baru saja               ┃ ← Auto-save indicator
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  BATAL              SIMPAN           │  ┃ ← Actions
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Differences from Create Form:**
- Top App Bar title: "Edit Perusahaan" (not "Tambah")
- All fields: Pre-filled with existing company data
- Same validation rules as Create
- Same auto-save behavior
- Same layout and interactions

**Permissions:**
- Only sales rep who created company can edit
- Manager: Read-only (no edit icon on detail screen)

---

## Screen 7: Company Search Results

### User Story
[US-2.1: Lihat List Companies](../developer-brief/USER_STORIES.md#us-21-lihat-list-companies)

### Tujuan
Menampilkan daftar perusahaan yang difilter berdasarkan query pencarian.

---

### Wireframe 12: Search Results - Found

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  ┌────────────────────────────┐  X   ┃ ← Search bar
┃     │ jakarta                    │       ┃   Query: "jakarta"
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃
┃                                            ┃
┃  3 perusahaan ditemukan                    ┃ ← Result count
┃                                            ┃   Body Medium, secondary
┃  ┌──────────────────────────────────────┐  ┃
┃  │  PT Surya Indah                      │  ┃
┃  │  Jl. Sudirman No. 123, Jakarta...    │  ┃ ← "Jakarta" highlighted
┃  │  3 kontak  •  2 proyek               │  ┃   Bold text
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  PT Mitra Sejahtera                  │  ┃
┃  │  Jl. Thamrin No. 67, Jakarta Pus...  │  ┃
┃  │  2 kontak  •  1 proyek               │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  PT Cahaya Cemerlang                 │  ┃
┃  │  Jl. Gatot Subroto, Jakarta Sela...  │  ┃
┃  │  4 kontak  •  3 proyek               │  ┃
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
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃
┃   🏠    📋    🏢    👤                     ┃
┃ Beranda Laporan Perusahaan Profil         ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Search Results:**
- Shows count: "X perusahaan ditemukan"
- Matches highlighted: Query term in bold
- Same card design as normal list
- Real-time filtering (300ms debounce)

**Interactions:**
- Same as normal list (tap card → detail)
- Type more → Update results in real-time
- Clear (X) → Remove filter, show all

---

### Wireframe 13: Search Results - No Results

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  ┌────────────────────────────┐  X   ┃
┃     │ xyz123                     │       ┃ ← Query with no matches
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃              ┌─────────┐                   ┃
┃              │         │                   ┃
┃              │   🔍    │                   ┃ ← Search icon (gray)
┃              │         │                   ┃   120×120dp
┃              └─────────┘                   ┃
┃                                            ┃
┃    Tidak ada hasil untuk "xyz123"          ┃ ← Title Medium (16sp)
┃                                            ┃
┃    Coba kata kunci lain atau buat          ┃ ← Body Medium (14sp)
┃    perusahaan baru.                        ┃   Secondary color
┃                                            ┃
┃                                            ┃
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
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃
┃   🏠    📋    🏢    👤                     ┃
┃ Beranda Laporan Perusahaan Profil         ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**No Results State:**
- Empty state illustration (search icon)
- Heading: "Tidak ada hasil untuk "[query]""
- Suggestion: "Coba kata kunci lain atau buat perusahaan baru."
- FAB still visible (user can create new company)

---

## Design System Notes

### Material Design 3 Components

**Companies List:**
- Cards: Elevated card (2dp elevation)
- FAB: Material 3 FAB (56×56dp)
- Bottom Navigation: Material 3 nav bar
- Pull-to-refresh: Material circular progress

**Company Detail:**
- Tabs: Material 3 primary tabs
- List items: Material 3 list item (2-line, 3-line)
- Info cards: Outlined card

**Form Screens:**
- Text fields: Material 3 filled text field
- Buttons: Material 3 filled button (SIMPAN), outlined button (BATAL)
- Error states: Red border + error text

### Accessibility

**Companies List:**
- Cards: Minimum 88dp height (touch target)
- Search: Clear label for screen readers
- FAB: Content description "Tambah perusahaan"

**Form Screens:**
- Labels: Always visible (not just placeholders)
- Error messages: Read by screen reader
- Required fields: Asterisk (*) + aria-required

---

## Implementation Notes

### Companies List
1. **Pagination:**
   - Load 20 companies per page
   - Implement either "Load More" button or infinite scroll
   - Cache results for 5 minutes

2. **Search:**
   - Client-side filtering for < 100 companies
   - Server-side search for > 100 companies
   - Debounce: 300ms after last keystroke

3. **Pull-to-Refresh:**
   - Fetch latest data from server
   - Merge with local changes (offline-first)
   - Show sync status indicator

### Form Screens
1. **Auto-save:**
   - Save draft to SQLite every 30 seconds
   - On app close, save immediately
   - On submit, delete draft

2. **Validation:**
   - Real-time validation on blur (field loses focus)
   - Show errors immediately, not on submit
   - Disable submit if invalid

3. **Offline Support:**
   - Save to local SQLite first
   - Queue for sync when online
   - Show "Pending sync" badge on list

---

## Related Documents

- **Screen Inventory:** [SCREEN_INVENTORY.md](./SCREEN_INVENTORY.md) (Screens 3-7)
- **User Stories:** [USER_STORIES.md](../developer-brief/USER_STORIES.md) (US-2.1, US-2.2, US-2.3)
- **Design Requirements:** [DESIGN_REQUIREMENTS.md](./DESIGN_REQUIREMENTS.md)

---

## Next Steps

➡️ **[Continue to: WIREFRAMES_CONTACTS.md →](./WIREFRAMES_CONTACTS.md)**

**Or navigate to:**
← [WIREFRAMES_SYNC_SETTINGS.md](./WIREFRAMES_SYNC_SETTINGS.md)
