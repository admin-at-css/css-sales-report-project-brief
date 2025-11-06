# WIREFRAMES_DASHBOARD.md

**Project:** CSS Sales Report - Mobile Application (Android)
**Design System:** Material Design 3
**Format:** ASCII Wireframes
**Last Updated:** 2025-01-07

---

## Overview

File ini berisi wireframe untuk layar Home/Dashboard dan Reports List untuk kedua role (Sales Rep dan Manager). Dashboard menampilkan statistik cepat dan akses ke laporan terbaru, sedangkan Reports List menyediakan pencarian dan filter lengkap.

**Total Wireframes:** 4 wireframes
- Screen 24: Home/Dashboard (Sales Rep) - Quick stats + recent reports
- Screen 25: My Reports List (Sales Rep) - With filters & search
- Screen 26: Home/Dashboard (Manager) - Team stats
- Screen 27: Team Reports List (Manager) - With filters

---

## Screen 24: Home/Dashboard (Sales Rep)

### Wireframe 1: Sales Rep Dashboard - Default View

### User Story
[US-6.1: Home/Dashboard Sales Rep](../developer-brief/USER_STORIES.md#us-61-homedashboard-sales-rep)

### Tujuan
Memberikan sales rep pandangan cepat tentang aktivitas mereka, termasuk statistik laporan dan akses cepat ke laporan terbaru. Dashboard ini adalah landing page setelah login.

**Dimensi:**
- Screen width: 360dp
- Status bar: 24dp
- App bar: 64dp (dengan title dan profile avatar)
- Content area: Full width minus 16dp padding kiri/kanan
- Card elevation: 2dp
- Bottom navigation: 80dp

**Key Metrics Cards:**
- Height: 80dp each
- Spacing: 12dp between cards
- Corner radius: 12dp
- Background: Surface container (Material 3)

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃ 🕐 10:30        CSS Sales Report    👤 ┃ ← App bar (64dp)
┣━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┫
┃                                         ┃
┃  Selamat Pagi, Budi 👋                  ┃ ← Greeting (16sp, padding 16dp top)
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │  📊 Total Laporan Bulan Ini      │   ┃ ← Metric card 1 (80dp height)
┃  │                                  │   ┃
┃  │  15 laporan                      │   ┃ ← Large number (24sp, bold)
┃  │  +3 dari bulan lalu              │   ┃ ← Comparison (12sp, secondary)
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │  ✅ Berhasil Sync                │   ┃ ← Metric card 2
┃  │                                  │   ┃
┃  │  12 laporan                      │   ┃
┃  │  3 pending sync                  │   ┃
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │  🏢 Companies Aktif              │   ┃ ← Metric card 3
┃  │                                  │   ┃
┃  │  8 companies                     │   ┃
┃  │  2 projects berjalan             │   ┃
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━    ┃ ← Divider (16dp margin)
┃                                         ┃
┃  Laporan Terbaru                        ┃ ← Section header (14sp, bold)
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │  PT Maju Jaya                    │   ┃ ← Recent report card 1
┃  │  Project Alpha • 5 Jan 2025      │   ┃
┃  │  3 foto • ✅ Tersync              │   ┃
┃  │  Tap untuk lihat detail →        │   ┃
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │  CV Sejahtera                    │   ┃ ← Recent report card 2
┃  │  Project Beta • 4 Jan 2025       │   ┃
┃  │  2 foto • ⏳ Pending sync         │   ┃
┃  │  Tap untuk lihat detail →        │   ┃
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │  LIHAT SEMUA LAPORAN →           │   ┃ ← CTA button (56dp height)
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃                                         ┃
┃                                         ┃ ← Scrollable content area
┣━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┫
┃  🏠      📋      🏢      ⚙️             ┃ ← Bottom nav (80dp)
┃ Home  Laporan  Data  Settings          ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

### Interaksi
1. **Tap Profile Avatar (top right)**: Navigate ke Profile/Settings screen
2. **Tap Metric Card**: Expand untuk melihat detail/breakdown (future enhancement)
3. **Tap Recent Report Card**: Navigate ke Report Detail screen
4. **Tap "LIHAT SEMUA LAPORAN"**: Navigate ke My Reports List (Screen 25)
5. **Tap Bottom Nav Item**: Navigate ke respective screen
6. **Pull to Refresh**: Refresh dashboard data dan sync status

### State
- Default: Menampilkan data real-time dari local database
- Loading: Skeleton cards saat initial load
- Error: Toast notification jika gagal load data
- Empty: Jika belum ada laporan, tampilkan empty state dengan CTA "Buat Laporan Pertama"

### Implementation Notes
- Dashboard data diambil dari local SQLite database
- Metric calculations:
  - "Total Laporan Bulan Ini": COUNT reports WHERE created_at BETWEEN first_day_of_month AND today
  - "Berhasil Sync": COUNT reports WHERE sync_status = 'synced'
  - "Companies Aktif": COUNT DISTINCT companies WHERE user_id = current_user
- Recent reports limited to 2-3 items, sorted by created_at DESC
- Refresh dilakukan setiap kali screen difokuskan (onResume)

---

## Screen 25: My Reports List (Sales Rep)

### Wireframe 2: My Reports List - With Filters & Search

### User Story
[US-6.3: Reports List dengan Filter](../developer-brief/USER_STORIES.md#us-63-reports-list-dengan-filter)

### Tujuan
Menampilkan semua laporan milik sales rep dengan kemampuan search, filter berdasarkan company/project, dan sort berdasarkan tanggal. List mendukung infinite scroll dan pull-to-refresh.

**Dimensi:**
- Screen width: 360dp
- App bar with search: 56dp
- Filter chips: 40dp height, scrollable horizontal
- List item: 96dp height
- Bottom navigation: 80dp

**Filter Options:**
- Semua Laporan (default)
- Filter by Company (dropdown)
- Filter by Project (dropdown)
- Filter by Sync Status (Tersync / Pending / Gagal)
- Sort by: Terbaru / Terlama / Company Name

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Laporan Saya              🔍  ⋮     ┃ ← App bar (56dp)
┣━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┫
┃                                         ┃
┃  ┌───────────────────────────────────┐ ┃ ← Search bar (48dp, collapsed)
┃  │  🔍  Cari laporan...              │ ┃
┃  └───────────────────────────────────┘ ┃
┃                                         ┃
┃  ┌──────┐ ┌──────┐ ┌────────┐ ┌─────┐ ┃ ← Filter chips (40dp, horizontal scroll)
┃  │ Semua│ │Company│ │ Project│ │Sort │ ┃
┃  └──────┘ └──────┘ └────────┘ └─────┘ ┃
┃                                         ┃
┃  42 laporan ditemukan                   ┃ ← Result count (12sp, secondary)
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │  PT Maju Jaya                    │   ┃ ← Report list item 1 (96dp)
┃  │  Project Alpha                   │   ┃
┃  │  5 Jan 2025 • 14:30              │   ┃
┃  │  3 foto • ✅ Tersync              │   ┃
┃  │  📸 📸 📸                          │   ┃ ← Photo thumbnails
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │  CV Sejahtera                    │   ┃ ← Report list item 2
┃  │  Project Beta                    │   ┃
┃  │  4 Jan 2025 • 09:15              │   ┃
┃  │  2 foto • ⏳ Pending sync         │   ┃
┃  │  📸 📸                            │   ┃
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │  PT Global Tech                  │   ┃ ← Report list item 3
┃  │  Project Gamma                   │   ┃
┃  │  3 Jan 2025 • 16:45              │   ┃
┃  │  5 foto • ❌ Gagal sync           │   ┃
┃  │  📸 📸 📸 📸 📸    [RETRY]        │   ┃ ← Retry button for failed
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │  CV Mitra Sejati                 │   ┃ ← Report list item 4
┃  │  Project Delta                   │   ┃
┃  │  2 Jan 2025 • 11:00              │   ┃
┃  │  1 foto • ✅ Tersync              │   ┃
┃  │  📸                               │   ┃
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃  ⏳ Loading more...                     ┃ ← Infinite scroll indicator
┃                                         ┃
┣━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┫
┃  🏠      📋      🏢      ⚙️             ┃ ← Bottom nav
┃ Home  Laporan  Data  Settings          ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

### Interaksi
1. **Tap Search Bar**: Expand search bar, show keyboard, filter list as user types
2. **Tap Filter Chip**: Show bottom sheet dengan filter options
   - **Company Chip**: Dropdown list semua companies user
   - **Project Chip**: Dropdown list semua projects (filtered by company if selected)
   - **Sort Chip**: Radio buttons (Terbaru / Terlama / Company Name)
3. **Tap Report List Item**: Navigate ke Report Detail screen
4. **Tap [RETRY] Button**: Retry sync untuk laporan yang gagal
5. **Long Press Report Item**: Show action menu (Edit / Delete / Share)
6. **Pull to Refresh**: Refresh report list dan sync status
7. **Scroll to Bottom**: Load next 20 reports (infinite scroll)
8. **Tap ⋮ (Menu)**: Show options (Export Data / Sync All / Settings)

### State
- **Default**: Semua laporan ditampilkan, sorted by created_at DESC
- **Searching**: Filter list by company name, project name, or report notes
- **Filtered**: Applied filters shown as active chips dengan X button untuk clear
- **Empty**: Jika tidak ada laporan, tampilkan empty state "Belum ada laporan"
- **Loading**: Skeleton items saat initial load atau infinite scroll
- **Error**: Toast notification jika gagal load data

### Implementation Notes
- Initial load: 20 reports, kemudian load 20 more per scroll
- Search: Real-time filter dengan debounce 300ms
- Filter persistence: Save filter state di SharedPreferences
- Sync status badge colors:
  - ✅ Tersync: Green (#4CAF50)
  - ⏳ Pending: Orange (#FF9800)
  - ❌ Gagal: Red (#F44336)
- Photo thumbnails: 40×40dp, corner radius 4dp, show first 5 photos only

---

## Screen 26: Home/Dashboard (Manager)

### Wireframe 3: Manager Dashboard - Team Overview

### User Story
[US-6.2: Home/Dashboard Manager](../developer-brief/USER_STORIES.md#us-62-homedashboard-manager)

### Tujuan
Memberikan manager pandangan komprehensif tentang aktivitas tim, termasuk statistik agregat, top performers, dan laporan terbaru dari seluruh tim. Dashboard ini membantu manager untuk monitoring dan decision-making.

**Dimensi:**
- Screen width: 360dp
- App bar: 64dp
- Metric cards: 80dp height each
- Team member card: 64dp height
- Bottom navigation: 80dp

**Key Differences from Sales Rep Dashboard:**
- Menampilkan data agregat dari seluruh tim (bukan hanya user sendiri)
- Menampilkan top performers / most active sales reps
- Menampilkan laporan dari semua team members

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃ 🕐 10:30        CSS Sales Report    👤 ┃ ← App bar (64dp)
┣━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┫
┃                                         ┃
┃  Selamat Pagi, Manager 👋               ┃ ← Greeting (16sp, padding 16dp top)
┃  Tim Sales Region Jakarta               ┃ ← Team name (12sp, secondary)
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │  📊 Total Laporan Tim (Bulan)    │   ┃ ← Metric card 1 (80dp height)
┃  │                                  │   ┃
┃  │  87 laporan                      │   ┃ ← Large number (24sp, bold)
┃  │  +12 dari bulan lalu             │   ┃ ← Comparison (12sp, secondary)
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │  👥 Sales Reps Aktif             │   ┃ ← Metric card 2
┃  │                                  │   ┃
┃  │  12 sales reps                   │   ┃
┃  │  10 aktif hari ini               │   ┃
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │  ⏳ Pending Sync                  │   ┃ ← Metric card 3
┃  │                                  │   ┃
┃  │  15 laporan                      │   ┃
┃  │  5 gagal sync (perlu perhatian) │   ┃ ← Alert (red text)
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━    ┃ ← Divider
┃                                         ┃
┃  Top Performers Bulan Ini 🏆            ┃ ← Section header (14sp, bold)
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │  👤 Budi Wijaya                  │   ┃ ← Top performer 1 (64dp)
┃  │  15 laporan • 8 companies        │   ┃
┃  │  ⭐⭐⭐⭐⭐                         │   ┃ ← Performance rating
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │  👤 Siti Aminah                  │   ┃ ← Top performer 2
┃  │  12 laporan • 6 companies        │   ┃
┃  │  ⭐⭐⭐⭐                          │   ┃
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │  👤 Agus Santoso                 │   ┃ ← Top performer 3
┃  │  10 laporan • 5 companies        │   ┃
┃  │  ⭐⭐⭐⭐                          │   ┃
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━    ┃
┃                                         ┃
┃  Laporan Terbaru dari Tim               ┃ ← Section header
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │  Budi W. • PT Maju Jaya          │   ┃ ← Recent report 1
┃  │  Project Alpha • 5 Jan 2025      │   ┃
┃  │  3 foto • ✅ Tersync              │   ┃
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │  Siti A. • CV Sejahtera          │   ┃ ← Recent report 2
┃  │  Project Beta • 5 Jan 2025       │   ┃
┃  │  2 foto • ⏳ Pending sync         │   ┃
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │  LIHAT SEMUA LAPORAN TIM →       │   ┃ ← CTA button
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┣━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┫
┃  🏠      📋      🏢      ⚙️             ┃ ← Bottom nav
┃ Home  Laporan  Data  Settings          ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

### Interaksi
1. **Tap Profile Avatar (top right)**: Navigate ke Profile/Settings screen
2. **Tap Metric Card**: Expand untuk melihat detail/breakdown
   - **"Pending Sync" Card**: Tap untuk lihat list laporan pending sync dengan detail
   - **"Sales Reps Aktif" Card**: Tap untuk lihat list semua sales reps dengan status
3. **Tap Top Performer Card**: Navigate ke Sales Rep Profile/Detail screen
4. **Tap Recent Report Card**: Navigate ke Report Detail screen
5. **Tap "LIHAT SEMUA LAPORAN TIM"**: Navigate ke Team Reports List (Screen 27)
6. **Pull to Refresh**: Refresh dashboard data untuk seluruh tim

### State
- **Default**: Menampilkan data real-time dari local database (manager melihat semua data tim)
- **Loading**: Skeleton cards saat initial load
- **Error**: Toast notification jika gagal load data
- **Empty**: Jika tim belum ada aktivitas, tampilkan empty state

### Implementation Notes
- Dashboard data diambil dari local SQLite database dengan filter by team/region
- Manager melihat data dari semua sales reps dalam tim/region mereka
- Metric calculations:
  - "Total Laporan Tim": SUM reports WHERE created_by IN (team_member_ids)
  - "Sales Reps Aktif": COUNT users WHERE last_active_date = today AND role = 'sales_rep'
  - "Pending Sync": COUNT reports WHERE sync_status IN ('pending', 'failed')
- Top performers: Ranked by report count dalam periode (bulan ini)
- Performance rating: Based on report frequency dan consistency
- Recent reports limited to 2-3 items, sorted by created_at DESC

---

## Screen 27: Team Reports List (Manager)

### Wireframe 4: Team Reports List - With Sales Rep Filter

### User Story
[US-6.3: Reports List dengan Filter](../developer-brief/USER_STORIES.md#us-63-reports-list-dengan-filter)

### Tujuan
Menampilkan semua laporan dari tim dengan kemampuan filter berdasarkan sales rep, company, project, dan sync status. Manager dapat melihat laporan dari sales rep mana pun dalam tim mereka.

**Dimensi:**
- Screen width: 360dp
- App bar with search: 56dp
- Filter chips: 40dp height, scrollable horizontal
- List item: 96dp height (sama dengan sales rep view)
- Bottom navigation: 80dp

**Key Difference from Sales Rep View:**
- Tambahan filter "Sales Rep" untuk filter by team member
- Setiap report item menampilkan nama sales rep yang membuat laporan

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Laporan Tim                🔍  ⋮     ┃ ← App bar (56dp)
┣━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┫
┃                                         ┃
┃  ┌───────────────────────────────────┐ ┃ ← Search bar (48dp)
┃  │  🔍  Cari laporan...              │ ┃
┃  └───────────────────────────────────┘ ┃
┃                                         ┃
┃  ┌────────┐ ┌──────┐ ┌────────┐ ┌───┐ ┃ ← Filter chips (horizontal scroll)
┃  │SalesRep│ │Company│ │Project │ │...│ ┃
┃  └────────┘ └──────┘ └────────┘ └───┘ ┃
┃                                         ┃
┃  156 laporan ditemukan                  ┃ ← Result count
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │  👤 Budi W.                      │   ┃ ← Report list item 1 (96dp)
┃  │  PT Maju Jaya • Project Alpha    │   ┃
┃  │  5 Jan 2025 • 14:30              │   ┃
┃  │  3 foto • ✅ Tersync              │   ┃
┃  │  📸 📸 📸                          │   ┃
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │  👤 Siti A.                      │   ┃ ← Report list item 2
┃  │  CV Sejahtera • Project Beta     │   ┃
┃  │  5 Jan 2025 • 09:15              │   ┃
┃  │  2 foto • ⏳ Pending sync         │   ┃
┃  │  📸 📸                            │   ┃
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │  👤 Agus S.                      │   ┃ ← Report list item 3
┃  │  PT Global Tech • Project Gamma  │   ┃
┃  │  4 Jan 2025 • 16:45              │   ┃
┃  │  5 foto • ❌ Gagal sync           │   ┃
┃  │  📸 📸 📸 📸 📸    [RETRY]        │   ┃
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │  👤 Budi W.                      │   ┃ ← Report list item 4
┃  │  CV Mitra • Project Delta        │   ┃
┃  │  4 Jan 2025 • 11:00              │   ┃
┃  │  1 foto • ✅ Tersync              │   ┃
┃  │  📸                               │   ┃
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │  👤 Dewi R.                      │   ┃ ← Report list item 5
┃  │  PT Sejahtera • Project Epsilon  │   ┃
┃  │  3 Jan 2025 • 15:20              │   ┃
┃  │  4 foto • ✅ Tersync              │   ┃
┃  │  📸 📸 📸 📸                       │   ┃
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │  👤 Rina M.                      │   ┃ ← Report list item 6
┃  │  CV Abadi • Project Zeta         │   ┃
┃  │  3 Jan 2025 • 10:45              │   ┃
┃  │  2 foto • ⏳ Pending sync         │   ┃
┃  │  📸 📸                            │   ┃
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃  ⏳ Loading more...                     ┃ ← Infinite scroll indicator
┃                                         ┃
┣━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┫
┃  🏠      📋      🏢      ⚙️             ┃ ← Bottom nav
┃ Home  Laporan  Data  Settings          ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

### Interaksi
1. **Tap Search Bar**: Expand search bar, filter list as user types (search by sales rep name, company, project)
2. **Tap Filter Chip**: Show bottom sheet dengan filter options
   - **Sales Rep Chip**: Dropdown list semua team members (multi-select)
   - **Company Chip**: Dropdown list semua companies (multi-select)
   - **Project Chip**: Dropdown list semua projects (filtered by company if selected)
   - **Sync Status Chip**: Radio buttons (Semua / Tersync / Pending / Gagal)
   - **Date Range Chip**: Date picker untuk filter by range
3. **Tap Report List Item**: Navigate ke Report Detail screen (read-only untuk manager)
4. **Tap [RETRY] Button**: Trigger manual sync untuk laporan yang gagal (manager memiliki permission)
5. **Long Press Report Item**: Show action menu (View Detail / Export / Force Sync)
6. **Pull to Refresh**: Refresh report list dan sync status untuk semua tim
7. **Scroll to Bottom**: Load next 20 reports (infinite scroll)
8. **Tap ⋮ (Menu)**: Show options (Export All Data / Bulk Sync / Filter Settings)

### State
- **Default**: Semua laporan tim ditampilkan, sorted by created_at DESC
- **Searching**: Filter list by sales rep, company, project, or notes
- **Filtered**: Applied filters shown as active chips dengan X button untuk clear
  - Example: "Budi W. (3) × Company: PT Maju Jaya (2) × Tersync (1) ×"
- **Empty**: Jika tidak ada laporan (atau filtered result empty), tampilkan appropriate message
- **Loading**: Skeleton items saat initial load atau infinite scroll
- **Error**: Toast notification jika gagal load data

### Implementation Notes
- Initial load: 20 reports, kemudian load 20 more per scroll
- Manager melihat reports WHERE created_by IN (team_member_ids)
- Search: Real-time filter dengan debounce 300ms
- Filter persistence: Save filter state di SharedPreferences per manager
- Multi-select filters:
  - Sales Rep: Allow selecting multiple team members untuk compare
  - Company: Allow selecting multiple companies
- Export functionality:
  - Export filtered results to CSV/Excel
  - Include all report details, photos (as links), dan metadata
- Bulk Sync: Manager dapat trigger sync untuk multiple failed reports sekaligus
- Permissions:
  - Manager dapat VIEW semua reports dalam tim
  - Manager TIDAK dapat EDIT reports (read-only)
  - Manager dapat RETRY sync untuk failed reports

---

## Cross-Screen Consistency

### Navigation Patterns
1. **Bottom Navigation (4 tabs)**:
   - 🏠 Home (Dashboard) - Always returns to role-appropriate dashboard
   - 📋 Laporan (Reports List) - Sales Rep: My Reports / Manager: Team Reports
   - 🏢 Data (Companies/Contacts/Projects) - Shared navigation hub
   - ⚙️ Settings (Profile, Sync, Logout) - Role-appropriate settings

2. **Back Button Behavior**:
   - From Dashboard: Exit app (dengan confirmation)
   - From Reports List: Return to Dashboard
   - From Report Detail: Return to Reports List (atau Dashboard jika dari recent report card)

### Design Tokens (Material 3)
- **Primary Color**: #1976D2 (Blue 700)
- **Secondary Color**: #424242 (Grey 800)
- **Success**: #4CAF50 (Green 500)
- **Warning**: #FF9800 (Orange 500)
- **Error**: #F44336 (Red 500)
- **Surface**: #FFFFFF (White)
- **Surface Container**: #F5F5F5 (Grey 100)

### Typography
- **Heading 1**: 24sp, Bold (Dashboard greeting)
- **Heading 2**: 20sp, Bold (Metric card values)
- **Heading 3**: 16sp, Bold (Section headers)
- **Body 1**: 14sp, Regular (List item primary text)
- **Body 2**: 12sp, Regular (List item secondary text)
- **Caption**: 11sp, Regular (Timestamps, metadata)

### Spacing (8dp Grid)
- Screen padding: 16dp (left/right)
- Card margin: 12dp (between cards)
- List item padding: 16dp (internal)
- Section spacing: 24dp (between sections)

---

## Accessibility Considerations

1. **Touch Targets**: Minimum 48×48dp untuk semua interactive elements
2. **Color Contrast**: Minimum 4.5:1 ratio untuk text, 3:1 untuk UI components
3. **Text Scaling**: Support dynamic type sizing (user dapat increase font size di settings)
4. **Screen Readers**: Semantic labels untuk semua interactive elements
5. **Focus Indicators**: Clear visual feedback saat navigation dengan keyboard/D-pad

---

## Performance Considerations

1. **Lazy Loading**: List items rendered only when visible (RecyclerView dengan ViewHolder pattern)
2. **Image Optimization**: Photo thumbnails di-compress dan di-cache locally
3. **Database Queries**: Indexed queries untuk fast filtering dan sorting
4. **Pagination**: Load 20 items per page untuk mencegah memory issues
5. **Caching**: Dashboard metrics di-cache dengan TTL 5 minutes

---

**End of WIREFRAMES_DASHBOARD.md**
