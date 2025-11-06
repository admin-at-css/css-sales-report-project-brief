# WIREFRAMES_STATES.md

**Project:** CSS Sales Report - Mobile Application (Android)
**Design System:** Material Design 3
**Format:** ASCII Wireframes
**Last Updated:** 2025-01-07

---

## Overview

File ini berisi 20 state designs yang digunakan di berbagai layar dalam aplikasi. State designs mencakup empty states, error states, loading states, dan success states yang memberikan feedback visual kepada user tentang status aplikasi dan data.

**Total State Designs:** 20 states
- **Empty States (8)**: Kondisi ketika tidak ada data untuk ditampilkan
- **Error States (5)**: Kondisi error yang memerlukan user action atau informasi
- **Loading States (4)**: Kondisi saat data sedang diproses atau dimuat
- **Success States (3)**: Konfirmasi visual setelah user action berhasil

**Design Principles:**
1. **Informative**: Jelaskan mengapa state ini terjadi
2. **Actionable**: Berikan clear next step kepada user
3. **Friendly**: Gunakan tone yang helpful, bukan menyalahkan
4. **Visual**: Gunakan icon/illustration untuk immediate understanding
5. **Consistent**: Gunakan pattern yang sama across all states

---

## EMPTY STATES (8)

Empty states ditampilkan ketika tidak ada data untuk ditampilkan. Mereka harus menjelaskan mengapa empty dan memberikan action untuk populate data.

---

### State 1: No Reports Yet (First Time User)

### User Story
[US-4.1: Buat Report Baru](../developer-brief/USER_STORIES.md#us-41-buat-report-baru)

### Tujuan
Ditampilkan kepada sales rep yang baru pertama kali membuka aplikasi atau belum pernah membuat laporan. State ini membantu onboarding dan mendorong user untuk membuat laporan pertama.

**Dimensi:**
- Screen width: 360dp
- Empty state container: Centered, 280dp width
- Illustration: 120×120dp
- Text area: Full width minus 32dp padding
- CTA button: Full width minus 48dp, height 48dp

**Lokasi Penggunaan:**
- Dashboard (Sales Rep) - Section "Laporan Terbaru"
- My Reports List - Ketika belum ada laporan

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Laporan Saya                        ┃
┣━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┫
┃                                         ┃
┃                                         ┃
┃                                         ┃
┃              ┌─────────┐                ┃
┃              │         │                ┃
┃              │   📋    │                ┃ ← Illustration (120×120dp)
┃              │         │                ┃
┃              └─────────┘                ┃
┃                                         ┃
┃        Belum Ada Laporan                ┃ ← Heading (18sp, bold)
┃                                         ┃
┃     Anda belum membuat laporan          ┃ ← Body text (14sp, center)
┃     penjualan. Mulai dokumentasi        ┃
┃     aktivitas sales Anda sekarang!      ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │     BUAT LAPORAN PERTAMA         │   ┃ ← Primary CTA button (48dp)
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │     Pelajari Cara Membuat        │   ┃ ← Secondary CTA button (48dp)
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃                                         ┃
┃                                         ┃
┃                                         ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

### Interaksi
1. **Tap "BUAT LAPORAN PERTAMA"**: Navigate ke Create Report flow (screen 18)
2. **Tap "Pelajari Cara Membuat"**: Show tutorial overlay atau navigate ke help screen

---

### State 2: No Companies Yet

### User Story
[US-2.2: Buat Company Baru](../developer-brief/USER_STORIES.md#us-22-buat-company-baru)

### Tujuan
Ditampilkan kepada sales rep yang belum memiliki company data. Mendorong user untuk menambahkan company pertama sebagai prerequisite untuk membuat contacts dan projects.

**Lokasi Penggunaan:**
- Companies List Screen (Screen 1)

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Companies                      +    ┃
┣━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┫
┃                                         ┃
┃                                         ┃
┃              ┌─────────┐                ┃
┃              │         │                ┃
┃              │   🏢    │                ┃ ← Building illustration
┃              │         │                ┃
┃              └─────────┘                ┃
┃                                         ┃
┃        Belum Ada Perusahaan             ┃ ← Heading
┃                                         ┃
┃     Tambahkan perusahaan untuk          ┃
┃     memulai mengelola data sales        ┃
┃     dan membuat laporan.                ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │     TAMBAH PERUSAHAAN            │   ┃ ← Primary CTA
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃                                         ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

### Interaksi
1. **Tap "TAMBAH PERUSAHAAN"**: Navigate ke Create Company form (screen 2)
2. **Tap + Button (top right)**: Same action as primary CTA

---

### State 3: No Contacts for Company

### User Story
[US-3.2: Buat Contact Baru](../developer-brief/USER_STORIES.md#us-32-buat-contact-baru)

### Tujuan
Ditampilkan dalam Contacts List ketika company belum memiliki contacts. Mendorong user untuk menambahkan contact pertama untuk company tersebut.

**Lokasi Penggunaan:**
- Contacts List Screen (Screen 7) - Ketika company belum punya contact

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Contacts - PT Maju Jaya        +    ┃
┣━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┫
┃                                         ┃
┃                                         ┃
┃              ┌─────────┐                ┃
┃              │         │                ┃
┃              │   👤    │                ┃ ← Person illustration
┃              │         │                ┃
┃              └─────────┘                ┃
┃                                         ┃
┃        Belum Ada Kontak                 ┃
┃                                         ┃
┃     Tambahkan kontak untuk              ┃
┃     perusahaan ini agar dapat           ┃
┃     membuat laporan penjualan.          ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │     TAMBAH KONTAK                │   ┃
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃                                         ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

### Interaksi
1. **Tap "TAMBAH KONTAK"**: Navigate ke Create Contact form (screen 8)

---

### State 4: No Projects for Company

### User Story
[US-4.3: Buat Project Baru](../developer-brief/USER_STORIES.md#us-43-buat-project-baru)

### Tujuan
Ditampilkan dalam Projects List ketika company belum memiliki projects. Mendorong user untuk menambahkan project sebagai prerequisite untuk membuat report.

**Lokasi Penggunaan:**
- Projects List Screen (Screen 13) - Ketika company belum punya project

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Projects - PT Maju Jaya        +    ┃
┣━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┫
┃                                         ┃
┃                                         ┃
┃              ┌─────────┐                ┃
┃              │         │                ┃
┃              │   📁    │                ┃ ← Folder illustration
┃              │         │                ┃
┃              └─────────┘                ┃
┃                                         ┃
┃        Belum Ada Project                ┃
┃                                         ┃
┃     Buat project untuk perusahaan       ┃
┃     ini agar dapat mendokumentasi       ┃
┃     aktivitas penjualan.                ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │     BUAT PROJECT                 │   ┃
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃                                         ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

### Interaksi
1. **Tap "BUAT PROJECT"**: Navigate ke Create Project form (screen 15)

---

### State 5: Search Results Empty

### User Story
[US-2.1: Lihat List Companies](../developer-brief/USER_STORIES.md#us-21-lihat-list-companies)

### Tujuan
Ditampilkan ketika user melakukan search tetapi tidak ada hasil yang cocok. Memberikan feedback bahwa search query tidak menemukan hasil dan menyarankan action.

**Lokasi Penggunaan:**
- Companies List (dengan search active)
- Contacts List (dengan search active)
- Projects List (dengan search active)
- Reports List (dengan search active)

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Companies                      +    ┃
┣━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┫
┃                                         ┃
┃  ┌───────────────────────────────────┐ ┃
┃  │  🔍  "PT Sejahtera"            ✕  │ ┃ ← Active search query
┃  └───────────────────────────────────┘ ┃
┃                                         ┃
┃              ┌─────────┐                ┃
┃              │         │                ┃
┃              │   🔎    │                ┃ ← Search illustration
┃              │         │                ┃
┃              └─────────┘                ┃
┃                                         ┃
┃        Tidak Ditemukan                  ┃
┃                                         ┃
┃     Tidak ada hasil untuk               ┃
┃     "PT Sejahtera"                      ┃
┃                                         ┃
┃     Coba kata kunci lain atau           ┃
┃     tambah perusahaan baru.             ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │     CLEAR PENCARIAN              │   ┃ ← Secondary button
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

### Interaksi
1. **Tap ✕ (in search bar)**: Clear search, kembali ke list lengkap
2. **Tap "CLEAR PENCARIAN"**: Same action as ✕ button
3. **Edit Search Query**: Update results real-time

---

### State 6: Filter Results Empty

### User Story
[US-6.3: Reports List dengan Filter](../developer-brief/USER_STORIES.md#us-63-reports-list-dengan-filter)

### Tujuan
Ditampilkan ketika user apply filter tetapi tidak ada data yang match dengan filter criteria. Menunjukkan active filters dan memberikan option untuk clear filters.

**Lokasi Penggunaan:**
- Reports List (dengan filter active)
- Companies List (dengan filter active)

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Laporan Saya                        ┃
┣━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┫
┃                                         ┃
┃  ┌──────────┐ ┌────────────┐           ┃
┃  │PT Maju  ✕│ │ Tersync  ✕ │           ┃ ← Active filter chips
┃  └──────────┘ └────────────┘           ┃
┃                                         ┃
┃  0 laporan ditemukan                    ┃ ← Result count
┃                                         ┃
┃              ┌─────────┐                ┃
┃              │         │                ┃
┃              │   🔍    │                ┃
┃              │         │                ┃
┃              └─────────┘                ┃
┃                                         ┃
┃        Tidak Ada Hasil                  ┃
┃                                         ┃
┃     Tidak ada laporan yang sesuai       ┃
┃     dengan filter yang dipilih.         ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │     HAPUS SEMUA FILTER           │   ┃
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │     UBAH FILTER                  │   ┃
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

### Interaksi
1. **Tap ✕ on Filter Chip**: Remove specific filter
2. **Tap "HAPUS SEMUA FILTER"**: Clear all filters, show full list
3. **Tap "UBAH FILTER"**: Open filter bottom sheet untuk adjust

---

### State 7: No Notifications

### User Story
[US-7.1: Lihat Sync Status](../developer-brief/USER_STORIES.md#us-71-lihat-sync-status)

### Tujuan
Ditampilkan dalam Notifications/Sync Status screen ketika tidak ada sync activity atau notifications. Reassure user bahwa semua berjalan normal.

**Lokasi Penggunaan:**
- Sync Status Screen (Screen 22) - Ketika queue kosong

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Sync Status                         ┃
┣━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┫
┃                                         ┃
┃  Status: ✅ Semua Tersync               ┃ ← Status banner
┃                                         ┃
┃                                         ┃
┃              ┌─────────┐                ┃
┃              │         │                ┃
┃              │   ✅    │                ┃ ← Checkmark illustration
┃              │         │                ┃
┃              └─────────┘                ┃
┃                                         ┃
┃        Tidak Ada Aktivitas              ┃
┃                                         ┃
┃     Semua data sudah tersync            ┃
┃     dengan server. Tidak ada            ┃
┃     pending changes.                    ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │     REFRESH                      │   ┃
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃                                         ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

### Interaksi
1. **Tap "REFRESH"**: Trigger manual sync check
2. **Pull to Refresh**: Same as refresh button

---

### State 8: No Team Reports (Manager)

### User Story
[US-6.2: Home/Dashboard Manager](../developer-brief/USER_STORIES.md#us-62-homedashboard-manager)

### Tujuan
Ditampilkan kepada manager ketika tidak ada laporan dari tim. Mendorong manager untuk encourage tim membuat laporan atau check filter settings.

**Lokasi Penggunaan:**
- Manager Dashboard - Section "Laporan Terbaru dari Tim"
- Team Reports List

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Laporan Tim                         ┃
┣━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┫
┃                                         ┃
┃                                         ┃
┃              ┌─────────┐                ┃
┃              │         │                ┃
┃              │   👥    │                ┃ ← Team illustration
┃              │         │                ┃
┃              └─────────┘                ┃
┃                                         ┃
┃        Belum Ada Laporan Tim            ┃
┃                                         ┃
┃     Tim Anda belum membuat laporan      ┃
┃     penjualan. Encourage team untuk     ┃
┃     mulai mendokumentasi aktivitas.     ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │     LIHAT TIM                    │   ┃
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │     REFRESH                      │   ┃
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

### Interaksi
1. **Tap "LIHAT TIM"**: Navigate ke Team Management screen
2. **Tap "REFRESH"**: Refresh data dari server

---

## ERROR STATES (5)

Error states ditampilkan ketika terjadi kesalahan. Mereka harus menjelaskan apa yang salah dan memberikan actionable steps untuk resolve.

---

### State 9: Network Error (Offline)

### User Story
[US-7.2: Auto Sync Saat Online](../developer-brief/USER_STORIES.md#us-72-auto-sync-saat-online)

### Tujuan
Ditampilkan ketika aplikasi tidak dapat connect ke server karena offline atau network issues. Reassure user bahwa aplikasi masih berfungsi offline dan data akan disync otomatis.

**Dimensi:**
- Banner height: 48dp (persistent banner di top of screen)
- Full screen error: Centered content, 280dp width

**Lokasi Penggunaan:**
- Banner: Persistent di semua screens ketika offline
- Full screen: Saat attempt sync atau login saat offline

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ⚠️  Offline Mode - Data akan disync    ┃ ← Persistent banner (48dp)
┃      saat online                        ┃
┣━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┫
┃                                         ┃
┃              ┌─────────┐                ┃
┃              │         │                ┃
┃              │   📡❌   │                ┃ ← No signal illustration
┃              │         │                ┃
┃              └─────────┘                ┃
┃                                         ┃
┃        Tidak Ada Koneksi                ┃
┃                                         ┃
┃     Anda sedang offline. Data akan      ┃
┃     tersimpan lokal dan otomatis        ┃
┃     disync saat kembali online.         ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │     COBA LAGI                    │   ┃ ← Primary button
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │     LANJUTKAN OFFLINE            │   ┃ ← Secondary button
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃  💡 Tips: Anda tetap bisa membuat       ┃ ← Helpful tip
┃     laporan, edit data, dan semua       ┃
┃     akan disync otomatis.               ┃
┃                                         ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

### Interaksi
1. **Tap "COBA LAGI"**: Attempt connection check, jika berhasil dismiss error dan proceed
2. **Tap "LANJUTKAN OFFLINE"**: Dismiss error screen, continue using app offline
3. **Banner Auto-Dismiss**: Banner hilang otomatis saat connection restored

### Implementation Notes
- Network status dimonitor dengan ConnectivityManager
- Banner berwarna orange (warning) bukan red (error)
- Tone reassuring, bukan alarming
- Emphasize offline-first capability

---

### State 10: Server Error (500)

### User Story
[US-7.3: Tombol Manual Sync](../developer-brief/USER_STORIES.md#us-73-tombol-manual-sync)

### Tujuan
Ditampilkan ketika server mengalami internal error (5xx responses). Inform user bahwa masalah ada di server side, bukan client side, dan suggest retry later.

**Lokasi Penggunaan:**
- Saat login attempt gagal karena server error
- Saat manual sync gagal karena server error
- Saat load data dari server gagal

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Sync                                ┃
┣━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┫
┃                                         ┃
┃              ┌─────────┐                ┃
┃              │         │                ┃
┃              │   🔧    │                ┃ ← Server/tools illustration
┃              │         │                ┃
┃              └─────────┘                ┃
┃                                         ┃
┃        Server Sedang Bermasalah         ┃
┃                                         ┃
┃     Server mengalami gangguan           ┃
┃     sementara. Tim kami sedang          ┃
┃     menangani masalah ini.              ┃
┃                                         ┃
┃     Error Code: 500                     ┃ ← Technical detail (collapsible)
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │     COBA LAGI                    │   ┃
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │     LANJUTKAN OFFLINE            │   ┃
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃  💡 Data Anda aman tersimpan lokal.     ┃
┃     Sync akan dicoba otomatis nanti.    ┃
┃                                         ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

### Interaksi
1. **Tap "COBA LAGI"**: Retry sync operation
2. **Tap "LANJUTKAN OFFLINE"**: Dismiss error, continue offline
3. **Automatic Retry**: Retry sync otomatis setiap 5 minutes (exponential backoff)

---

### State 11: Photo Upload Failed

### User Story
[US-4.2: Ambil Foto untuk Report](../developer-brief/USER_STORIES.md#us-42-ambil-foto-untuk-report)

### Tujuan
Ditampilkan ketika upload foto gagal (file too large, network timeout, server rejection). Specific error message dengan clear resolution steps.

**Lokasi Penggunaan:**
- Report Detail Screen - Saat photo sync failed
- Create Report Flow - Saat add photo gagal

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Laporan Detail                      ┃
┣━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┫
┃                                         ┃
┃  PT Maju Jaya • Project Alpha           ┃
┃  5 Jan 2025 • 14:30                     ┃
┃                                         ┃
┃  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━    ┃
┃                                         ┃
┃  Foto (3)                               ┃
┃                                         ┃
┃  ┌──────┐ ┌──────┐ ┌──────┐            ┃
┃  │ ✅   │ │ ✅   │ │  ❌  │            ┃ ← Failed photo
┃  │      │ │      │ │      │            ┃
┃  │Photo │ │Photo │ │Photo │            ┃
┃  │  1   │ │  2   │ │  3   │            ┃
┃  └──────┘ └──────┘ └──────┘            ┃
┃                                         ┃
┃  ⚠️ Foto 3 Gagal Diupload               ┃ ← Error banner (red, 64dp)
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │  ⚠️ Upload Gagal                 │   ┃ ← Error detail card
┃  │                                  │   ┃
┃  │  File terlalu besar (12.5 MB)   │   ┃ ← Specific error
┃  │  Maksimal ukuran: 10 MB          │   ┃ ← Constraint
┃  │                                  │   ┃
┃  │  [COMPRESS & RETRY]  [HAPUS]     │   ┃ ← Action buttons
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃  💡 Tips: Compress foto di gallery      ┃ ← Helpful tip
┃     atau ambil foto dengan resolusi     ┃
┃     lebih rendah.                       ┃
┃                                         ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

### Interaksi
1. **Tap [COMPRESS & RETRY]**: Compress photo otomatis dan retry upload
2. **Tap [HAPUS]**: Remove failed photo dari report
3. **Tap Failed Photo Thumbnail**: Show full error detail dengan options

### Error Types & Messages
- **File too large**: "File terlalu besar (X MB). Maksimal: 10 MB"
- **Network timeout**: "Upload timeout. Cek koneksi internet Anda"
- **Server rejection**: "Server menolak file. Format tidak didukung"
- **Disk space**: "Storage penuh. Hapus beberapa file terlebih dahulu"

---

### State 12: Login Failed (Wrong Credentials)

### User Story
[US-1.1: Login dengan Email & Password](../developer-brief/USER_STORIES.md#us-11-login-dengan-email--password)

### Tujuan
Ditampilkan ketika user memasukkan email/password yang salah. Clear feedback dengan actionable steps (retry atau forgot password).

**Lokasi Penggunaan:**
- Login Screen (Screen 0) - Setelah submit dengan wrong credentials

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃                                         ┃
┃              [LOGO]                     ┃
┃         CSS Sales Report                ┃
┃                                         ┃
┃  ┌───────────────────────────────────┐ ┃
┃  │  Email                            │ ┃
┃  │  budi@example.com                 │ ┃
┃  └───────────────────────────────────┘ ┃
┃                                         ┃
┃  ┌───────────────────────────────────┐ ┃
┃  │  Password                         │ ┃
┃  │  ••••••••                         │ ┃
┃  └───────────────────────────────────┘ ┃
┃                                         ┃
┃  ⚠️ Login Gagal                         ┃ ← Error message (inline, red)
┃  Email atau password salah. Silakan     ┃
┃  coba lagi atau reset password Anda.    ┃
┃                                         ┃
┃  ┌───────────────────────────────────┐ ┃
┃  │         COBA LAGI                 │ ┃ ← Primary button
┃  └───────────────────────────────────┘ ┃
┃                                         ┃
┃         Lupa Password?                  ┃ ← Link
┃                                         ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

### Interaksi
1. **Tap "COBA LAGI"**: Clear error message, allow re-entering credentials
2. **Tap "Lupa Password?"**: Navigate ke forgot password flow (webview atau email)
3. **Edit Input Fields**: Auto-clear error message saat user mulai typing

### Security Notes
- Generic error message (tidak specify "email salah" vs "password salah")
- Rate limiting: Max 5 attempts per 15 minutes
- Show CAPTCHA setelah 3 failed attempts
- Lock account setelah 10 failed attempts

---

### State 13: Data Sync Conflict

### User Story
[US-7.2: Auto Sync Saat Online](../developer-brief/USER_STORIES.md#us-72-auto-sync-saat-online)

### Tujuan
Ditampilkan ketika terjadi sync conflict (user edit data di device A, user lain edit same data di device B, keduanya sync). User harus choose which version to keep.

**Lokasi Penggunaan:**
- Saat auto-sync detect conflict
- Modal dialog blocking user until resolved

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃                                         ┃
┃  ⚠️ Sync Conflict Terdeteksi            ┃ ← Modal dialog title
┃                                         ┃
┃  Data "PT Maju Jaya" diubah oleh        ┃
┃  2 orang secara bersamaan.              ┃
┃                                         ┃
┃  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━    ┃
┃                                         ┃
┃  📱 Versi Anda (Device Ini)             ┃ ← Local version
┃  Alamat: Jl. Sudirman No. 123           ┃
┃  No. Telepon: 021-5551234               ┃
┃  Terakhir diubah: 5 Jan, 14:35          ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │  GUNAKAN VERSI INI               │   ┃ ← Button
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━    ┃
┃                                         ┃
┃  ☁️ Versi Server                        ┃ ← Server version
┃  Alamat: Jl. Sudirman No. 125           ┃
┃  No. Telepon: 021-5555678               ┃
┃  Terakhir diubah: 5 Jan, 14:32          ┃
┃  Oleh: Siti Aminah                      ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │  GUNAKAN VERSI INI               │   ┃
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━    ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │  GABUNGKAN MANUAL                │   ┃ ← Advanced option
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

### Interaksi
1. **Tap "GUNAKAN VERSI INI" (local)**: Keep local changes, overwrite server
2. **Tap "GUNAKAN VERSI INI" (server)**: Discard local changes, use server version
3. **Tap "GABUNGKAN MANUAL"**: Navigate ke merge editor (advanced)
4. **Modal Cannot Be Dismissed**: User must resolve conflict

### Implementation Notes
- Conflicts tracked per-entity dengan version numbers
- Show diff/comparison untuk help user decide
- Default recommendation: Newest timestamp (highlighted dengan ⭐)
- Log conflict resolution untuk audit trail

---

## LOADING STATES (4)

Loading states memberikan feedback visual bahwa aplikasi sedang processing. Mereka harus indicate progress ketika possible dan reassure user bahwa app tidak frozen.

---

### State 14: Initial App Load / Splash with Loading

### User Story
[US-1.1: Login dengan Email & Password](../developer-brief/USER_STORIES.md#us-11-login-dengan-email--password)

### Tujuan
Ditampilkan saat app pertama kali dibuka. Branded splash screen dengan loading indicator. Duration maksimal 2-3 detik.

**Dimensi:**
- Full screen: 360×640dp
- Logo: 120×120dp centered
- Loading indicator: 48dp circular progress below logo

**Lokasi Penggunaan:**
- App launch (cold start)
- After login success (transition ke dashboard)

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃                                         ┃
┃                                         ┃
┃                                         ┃
┃                                         ┃
┃                                         ┃
┃              ┌─────────┐                ┃
┃              │         │                ┃
┃              │  [LOGO] │                ┃ ← App logo (120×120dp)
┃              │         │                ┃
┃              └─────────┘                ┃
┃                                         ┃
┃         CSS Sales Report                ┃ ← App name (20sp, bold)
┃                                         ┃
┃              ◐ Loading...               ┃ ← Circular progress (48dp)
┃                                         ┃
┃                                         ┃
┃                                         ┃
┃                                         ┃
┃                                         ┃
┃                                         ┃
┃                                         ┃
┃         v1.0.0 (Build 42)               ┃ ← Version (bottom, 10sp)
┃                                         ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

### Implementation Notes
- Maksimal duration: 3 seconds, timeout ke error screen jika lebih lama
- Background: Gradient atau brand color
- Loading indicator: Material Design circular progress (indeterminate)
- Tasks during splash:
  - Check auth token
  - Initialize database
  - Load user preferences
  - Check network connectivity

---

### State 15: List Loading (Skeleton)

### User Story
[US-2.1: Lihat List Companies](../developer-brief/USER_STORIES.md#us-21-lihat-list-companies)

### Tujuan
Ditampilkan saat loading list data (companies, contacts, projects, reports). Skeleton screens memberikan preview of layout dan reduce perceived loading time.

**Dimensi:**
- Skeleton item height: Same as actual item (e.g., 80dp untuk company item)
- Shimmer effect: Animated gradient dari kiri ke kanan
- Number of skeleton items: 5-6 visible items

**Lokasi Penggunaan:**
- Companies List (initial load)
- Contacts List
- Projects List
- Reports List
- Infinite scroll (show 2-3 skeleton items at bottom)

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Companies                      +    ┃
┣━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┫
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │  ▓▓▓▓▓▓▓▓▓▓▓▓                   │   ┃ ← Skeleton item 1 (shimmer)
┃  │  ▓▓▓▓▓▓▓▓                        │   ┃
┃  │  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓                │   ┃
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓                │   ┃ ← Skeleton item 2
┃  │  ▓▓▓▓▓▓▓▓▓▓                      │   ┃
┃  │  ▓▓▓▓▓▓▓▓▓▓▓▓▓                  │   ┃
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │  ▓▓▓▓▓▓▓▓▓▓                      │   ┃ ← Skeleton item 3
┃  │  ▓▓▓▓▓▓▓▓                        │   ┃
┃  │  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓                 │   ┃
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓                │   ┃ ← Skeleton item 4
┃  │  ▓▓▓▓▓▓▓▓▓▓▓▓                    │   ┃
┃  │  ▓▓▓▓▓▓▓▓▓▓▓                     │   ┃
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │  ▓▓▓▓▓▓▓▓▓▓▓▓▓                  │   ┃ ← Skeleton item 5
┃  │  ▓▓▓▓▓▓▓▓▓▓                      │   ┃
┃  │  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓               │   ┃
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

### Implementation Notes
- Use shimmer library (e.g., Facebook Shimmer for Android)
- Skeleton shape matches actual item layout
- Animation: 1.5s duration, repeat infinite
- Replace dengan actual data as soon as available (progressive rendering)
- Timeout: Show error state setelah 10 seconds

---

### State 16: Photo Uploading (Progress Bar)

### User Story
[US-4.2: Ambil Foto untuk Report](../developer-brief/USER_STORIES.md#us-42-ambil-foto-untuk-report)

### Tujuan
Ditampilkan saat upload foto ke server. Determinate progress bar menunjukkan actual upload progress. User dapat cancel upload jika needed.

**Dimensi:**
- Progress bar: Full width minus 32dp padding, height 4dp
- Progress percentage: 14sp, centered below bar
- Cancel button: 48×48dp (icon button)

**Lokasi Penggunaan:**
- Create Report flow - Saat save dengan foto
- Report Detail - Saat retry upload foto yang failed
- Background: Notification saat app di background

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Laporan Detail                      ┃
┣━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┫
┃                                         ┃
┃  PT Maju Jaya • Project Alpha           ┃
┃  5 Jan 2025 • 14:30                     ┃
┃                                         ┃
┃  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━    ┃
┃                                         ┃
┃  Foto (3)                               ┃
┃                                         ┃
┃  ┌──────┐ ┌──────┐ ┌──────┐            ┃
┃  │ ✅   │ │ ✅   │ │ ⏳   │            ┃ ← Uploading photo
┃  │      │ │      │ │      │            ┃
┃  │Photo │ │Photo │ │Photo │            ┃
┃  │  1   │ │  2   │ │  3   │            ┃
┃  └──────┘ └──────┘ └──────┘            ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │  📤 Mengupload Foto 3 of 3       │   ┃ ← Upload status card
┃  │                                  │   ┃
┃  │  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓░░░░░░░░         │   ┃ ← Progress bar (67%)
┃  │                                  │   ┃
┃  │  67% • 2.1 MB / 3.2 MB           │   ┃ ← Progress detail
┃  │  Estimasi: 5 detik               │   ┃
┃  │                                  │   ┃
┃  │             [CANCEL]              │   ┃ ← Cancel button
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃  💡 Jangan tutup aplikasi saat upload   ┃ ← Helpful tip
┃     sedang berlangsung.                 ┃
┃                                         ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

### Interaksi
1. **Tap [CANCEL]**: Cancel upload, mark photo as failed, show option to retry
2. **Background Upload**: Continue upload even if user navigate away (show persistent notification)
3. **Network Change**: Pause upload jika offline, resume otomatis saat kembali online

### Implementation Notes
- Use WorkManager untuk background upload dengan network constraint
- Upload photos sequentially (bukan parallel) untuk better progress tracking
- Show notification dengan progress bar saat app di background
- Retry logic: Auto-retry 3 times dengan exponential backoff jika gagal

---

### State 17: Syncing Data (Multi-Entity Progress)

### User Story
[US-7.3: Tombol Manual Sync](../developer-brief/USER_STORIES.md#us-73-tombol-manual-sync)

### Tujuan
Ditampilkan saat manual sync in progress. Menunjukkan progress untuk multiple entity types (companies, contacts, projects, reports). User dapat see detailed progress.

**Dimensi:**
- Full screen modal: 320dp width, variable height
- Progress indicator per entity: 40dp height
- Overall progress: Large circular progress di top

**Lokasi Penggunaan:**
- Manual Sync (triggered dari Settings atau Sync screen)
- Auto-Sync (background, show notification only)

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃                                         ┃
┃              ◐ 67%                      ┃ ← Large circular progress
┃                                         ┃
┃         Syncing Data...                 ┃ ← Title (18sp, bold)
┃                                         ┃
┃  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━    ┃
┃                                         ┃
┃  ✅ Companies (12 of 12)                ┃ ← Entity 1: Complete
┃  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓         ┃
┃                                         ┃
┃  ✅ Contacts (45 of 45)                 ┃ ← Entity 2: Complete
┃  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓         ┃
┃                                         ┃
┃  ⏳ Projects (8 of 15)                  ┃ ← Entity 3: In Progress
┃  ▓▓▓▓▓▓▓▓▓▓▓▓▓░░░░░░░░░░░░░░           ┃
┃                                         ┃
┃  ⏳ Reports (0 of 42)                   ┃ ← Entity 4: Pending
┃  ░░░░░░░░░░░░░░░░░░░░░░░░░░░░           ┃
┃                                         ┃
┃  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━    ┃
┃                                         ┃
┃  Estimasi waktu: ~2 menit               ┃ ← Time estimate
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │          BATALKAN                │   ┃ ← Cancel button
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

### Interaksi
1. **Tap "BATALKAN"**: Cancel sync, show confirmation dialog ("Yakin batalkan sync?")
2. **Background Sync**: Minimize modal, show persistent notification dengan progress
3. **Completion**: Auto-dismiss modal, show success toast

### Implementation Notes
- Sync order: Companies → Contacts → Projects → Reports (dependency order)
- Use SyncAdapter atau WorkManager untuk background sync
- Batch sync: 50 items per batch untuk prevent timeout
- Error handling: Continue sync untuk other entities jika satu entity gagal
- Detailed log: Save sync log untuk troubleshooting (accessible di Settings)

---

## SUCCESS STATES (3)

Success states memberikan positive feedback setelah user action berhasil. Mereka harus celebratory tapi tidak intrusive, dengan clear next steps.

---

### State 18: Report Created Successfully

### User Story
[US-4.1: Buat Report Baru](../developer-brief/USER_STORIES.md#us-41-buat-report-baru)

### Tujuan
Ditampilkan setelah user berhasil membuat report baru. Celebrate success dan provide quick actions (view report, create another, atau back to list).

**Dimensi:**
- Modal dialog: 320dp width, 400dp height
- Success icon: 80×80dp
- Action buttons: Full width, 48dp height

**Lokasi Penggunaan:**
- Create Report flow - Setelah save berhasil

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃                                         ┃
┃              ┌─────────┐                ┃
┃              │         │                ┃
┃              │   ✅    │                ┃ ← Success icon (80×80dp)
┃              │         │                ┃
┃              └─────────┘                ┃
┃                                         ┃
┃        Laporan Berhasil Dibuat!         ┃ ← Title (20sp, bold)
┃                                         ┃
┃     Laporan untuk PT Maju Jaya          ┃ ← Detail text (14sp)
┃     (Project Alpha) telah tersimpan.    ┃
┃                                         ┃
┃     ✅ 3 foto diupload                   ┃ ← Confirmation checklist
┃     ✅ Data disimpan lokal               ┃
┃     ⏳ Akan disync saat online           ┃
┃                                         ┃
┃  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━    ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │     LIHAT LAPORAN                │   ┃ ← Primary action
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │     BUAT LAPORAN LAGI            │   ┃ ← Secondary action
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │     KEMBALI KE DAFTAR            │   ┃ ← Tertiary action
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

### Interaksi
1. **Tap "LIHAT LAPORAN"**: Navigate ke Report Detail screen untuk newly created report
2. **Tap "BUAT LAPORAN LAGI"**: Navigate back to Create Report flow (pre-fill last used company/project)
3. **Tap "KEMBALI KE DAFTAR"**: Navigate ke My Reports List
4. **Auto-Dismiss**: Modal auto-dismiss setelah 5 seconds jika no interaction (default action: navigate ke Report Detail)

### Implementation Notes
- Show animation: Checkmark dengan scale + fade animation
- Haptic feedback: Light vibration saat success
- Sound: Optional success sound (disable-able di settings)

---

### State 19: Data Synced Successfully

### User Story
[US-7.3: Tombol Manual Sync](../developer-brief/USER_STORIES.md#us-73-tombol-manual-sync)

### Tujuan
Ditampilkan setelah manual sync atau auto-sync berhasil. Inform user tentang sync completion dan summary of synced items.

**Dimensi:**
- Toast notification: 280dp width, 120dp height, bottom of screen
- Dismiss automatically: 4 seconds
- Swipe to dismiss

**Lokasi Penggunaan:**
- After manual sync completes
- After auto-sync completes (less prominent)
- Sync Status screen

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  🏠      📋      🏢      ⚙️             ┃ ← Bottom nav
┃ Home  Laporan  Data  Settings          ┃
┣━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┫
┃                                         ┃
┃  ┌─────────────────────────────────┐   ┃
┃  │  ✅ Sync Selesai                 │   ┃ ← Toast/Snackbar (120dp)
┃  │                                  │   ┃
┃  │  15 laporan • 8 companies        │   ┃ ← Summary
┃  │  32 contacts • 12 projects       │   ┃
┃  │                                  │   ┃
┃  │  [LIHAT DETAIL]          ✕       │   ┃ ← Action + dismiss
┃  └─────────────────────────────────┘   ┃
┃                                         ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

### Interaksi
1. **Tap [LIHAT DETAIL]**: Navigate ke Sync Status screen untuk detailed log
2. **Tap ✕**: Dismiss toast immediately
3. **Swipe Down**: Dismiss toast
4. **Auto-Dismiss**: Toast disappear setelah 4 seconds

### Implementation Notes
- Use Material Snackbar component
- Background color: Success green (#4CAF50)
- Show only significant syncs (>5 items changed), otherwise silent sync
- Detailed sync log available di Sync Status screen

---

### State 20: Changes Saved Successfully

### User Story
[US-2.3: Edit Company Data](../developer-brief/USER_STORIES.md#us-23-edit-company-data)

### Tujuan
Ditampilkan setelah user berhasil save changes (edit company, contact, project). Lightweight confirmation tanpa interrupt flow.

**Dimensi:**
- Toast: 240dp width, 56dp height
- Position: Bottom center, above bottom nav
- Duration: 2 seconds

**Lokasi Penggunaan:**
- Edit Company form - Setelah save
- Edit Contact form - Setelah save
- Edit Project form - Setelah save
- Profile/Settings - Setelah save preferences

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Edit Company                        ┃
┣━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┫
┃                                         ┃
┃  Company Name                           ┃
┃  PT Maju Jaya Teknologi                 ┃
┃                                         ┃
┃  Alamat                                 ┃
┃  Jl. Sudirman No. 123                   ┃
┃                                         ┃
┃  ...                                    ┃
┃                                         ┃
┃                                         ┃
┃  ┌───────────────────────────────┐     ┃
┃  │   ✅ Perubahan tersimpan       │     ┃ ← Toast (56dp, center)
┃  └───────────────────────────────┘     ┃
┃                                         ┃
┣━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┫
┃  🏠      📋      🏢      ⚙️             ┃
┃ Home  Laporan  Data  Settings          ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

### Interaksi
1. **Auto-Navigate**: After toast dismiss (2s), navigate back ke detail view atau list
2. **Tap Toast**: Dismiss immediately dan navigate
3. **No Dismiss Button**: Simple, non-intrusive feedback

### Implementation Notes
- Use Material Toast component
- Lightweight: Just icon + short text
- Haptic feedback: Ultra-light vibration
- Background color: Success green dengan 90% opacity

---

## Cross-State Design Principles

### Consistency Guidelines

1. **Color Coding**:
   - Success: Green (#4CAF50)
   - Warning: Orange (#FF9800)
   - Error: Red (#F44336)
   - Info: Blue (#2196F3)
   - Neutral: Grey (#757575)

2. **Icon Usage**:
   - ✅ Success / Complete
   - ⚠️ Warning / Attention needed
   - ❌ Error / Failed
   - ⏳ In Progress / Loading
   - 🔍 Search / Not Found
   - 📸 Photo related
   - 📡 Connectivity
   - 👤 User / Profile
   - 🏢 Company
   - 📋 Report
   - 📁 Project

3. **Typography Hierarchy**:
   - State Title: 20sp, Bold
   - Body Text: 14sp, Regular
   - Action Button: 14sp, Medium (all caps)
   - Helper Text: 12sp, Regular

4. **Spacing**:
   - Screen padding: 16dp
   - Element spacing: 16dp vertical
   - Button spacing: 12dp between buttons
   - Section spacing: 24dp

5. **Animation**:
   - State transitions: 300ms ease-in-out
   - Toast appearance: Slide up from bottom, 200ms
   - Toast dismissal: Fade out, 150ms
   - Skeleton shimmer: 1500ms cycle

### Accessibility

1. **Screen Readers**:
   - All states have semantic labels
   - Error messages read automatically
   - Progress updates announced

2. **Color Contrast**:
   - Text on colored backgrounds: Min 4.5:1 ratio
   - Icons: Min 3:1 ratio

3. **Touch Targets**:
   - All buttons: Min 48×48dp
   - Dismissible toasts: Min 56dp height

4. **Text Scaling**:
   - Support up to 200% text scaling
   - Layout adapts without breaking

---

**End of WIREFRAMES_STATES.md**
