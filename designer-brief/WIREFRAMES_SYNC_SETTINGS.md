# Wireframes - Sync, Settings & Profile Screens

← [Sebelumnya: WIREFRAMES_AUTHENTICATION.md](./WIREFRAMES_AUTHENTICATION.md)

---

## Overview

Dokumen ini berisi detailed ASCII wireframes untuk **EPIC 7: Sync & Settings** dan **Additional Screens**.

**Total Wireframes:** 12 wireframes
- Screen 28: Sync Status (4 states)
- Screen 29: Settings (2 states)
- Screen 30: Profile (2 states)
- Screen 31: Onboarding (4 slides)

---

## Design Specifications

### Platform & Dimensions
- **Platform:** Android Mobile
- **Screen Size:** 360 × 800 dp
- **Orientation:** Portrait only
- **Grid System:** 8dp base unit

### Typography
- **Heading Large:** 32sp, Medium weight
- **Title Large:** 22sp, Medium weight
- **Body Large:** 16sp, Regular weight
- **Body Medium:** 14sp, Regular weight
- **Label Medium:** 12sp, Medium weight

### Colors
- **Primary:** #2E7D32 (green)
- **Success:** #4CAF50 (green-500)
- **Error:** #D32F2F (red-700)
- **Warning:** #F57C00 (amber-700)
- **Info:** #1976D2 (blue-700)
- **Text Primary:** #212121 (gray-900)
- **Text Secondary:** #757575 (gray-600)
- **Surface:** #FFFFFF (white)
- **Background:** #F5F5F5 (gray-100)

---

## Screen 28: Sync Status Screen

### User Story
[US-7.2: Auto Sync Saat Online](../developer-brief/USER_STORIES.md#us-72-auto-sync-saat-online), [US-7.3: Tombol Manual Sync](../developer-brief/USER_STORIES.md#us-73-tombol-manual-sync)

### Tujuan
Memberikan visibility lengkap tentang sync status, memungkinkan manual sync, dan menampilkan detailed progress untuk semua entities.

---

### Wireframe 1: Sync Status - All Synced (Success State)

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Status Sinkronisasi          🔄      ┃ ← Top App Bar
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃   Back button + refresh icon
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃ ← 16dp top margin
┃  │  ✅  Semua data tersinkronisasi      │  ┃
┃  │                                      │  ┃ ← Status Card (elevated)
┃  │  Terakhir disinkronisasi:            │  ┃   Background: #E8F5E9 (green-50)
┃  │  12 Jan 2025, 14:35                  │  ┃   Height: auto (wrap content)
┃  └──────────────────────────────────────┘  ┃   Padding: 16dp
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃ ← 16dp spacing
┃  │    SINKRONKAN SEKARANG               │  ┃ ← Primary Button
┃  └──────────────────────────────────────┘  ┃   Height: 48dp
┃                                            ┃   Background: #2E7D32
┃  ┌──────────────────────────────────────┐  ┃ ← 24dp spacing
┃  │  Detail Sinkronisasi            ▼    │  ┃ ← Accordion header (collapsed)
┃  └──────────────────────────────────────┘  ┃   Height: 48dp
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃ ← 16dp spacing
┃  │  📶  Online                           │  ┃ ← Network Status Indicator
┃  │                                      │  ┃   Background: #E3F2FD (blue-50)
┃  │  Koneksi stabil                      │  ┃   Height: 56dp
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃ ← 16dp spacing
┃  │  Log Transaksi                  ▼    │  ┃ ← Accordion header (collapsed)
┃  └──────────────────────────────────────┘  ┃   Optional section
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Dimensions:**
- Screen margin: 16dp left/right
- Status card: Full width minus 32dp, padding 16dp
- Button: Full width minus 32dp, height 48dp
- Accordion headers: Height 48dp, chevron icon 24dp
- Network indicator: Height 56dp

**State:**
- All entities synced (no pending, no failed)
- Status card: Green background (#E8F5E9)
- Manual sync button: ENABLED
- Detail accordion: COLLAPSED
- Network: ONLINE

**Interactions:**
- Tap refresh icon → Trigger manual sync
- Tap "SINKRONKAN SEKARANG" → Start sync (transition to State 2)
- Tap "Detail Sinkronisasi" → Expand accordion (show entity-level details)
- Tap "Log Transaksi" → Expand to show last 20 transactions

---

### Wireframe 2: Sync Status - Syncing in Progress

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Status Sinkronisasi          🔄      ┃
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  ⏳  Menyinkronkan data...           │  ┃ ← Loading state
┃  │                                      │  ┃   Background: #FFF9C4 (amber-50)
┃  │  ━━━━━━━━━━━━░░░░░░░░  70%          │  ┃   Progress bar + percentage
┃  │                                      │  ┃
┃  │  Menyinkronkan 7 dari 10 item        │  ┃ ← Progress text
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │    BATALKAN SINKRONISASI             │  ┃ ← Secondary Button (outlined)
┃  └──────────────────────────────────────┘  ┃   Height: 48dp
┃                                            ┃   Border: Gray
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Detail Sinkronisasi            ▲    │  ┃ ← Accordion (EXPANDED)
┃  ├──────────────────────────────────────┤  ┃
┃  │  📊 Perusahaan                       │  ┃
┃  │     2 pending  •  0 failed           │  ┃ ← Entity status
┃  │     ━━━━━━░░░░  60%                  │  ┃   Mini progress bar
┃  │                                      │  ┃
┃  │  👤 Kontak                           │  ┃
┃  │     3 pending  •  0 failed           │  ┃
┃  │     ━━━━━━━░░░  70%                  │  ┃
┃  │                                      │  ┃
┃  │  📁 Proyek                           │  ┃
┃  │     1 pending  •  0 failed           │  ┃
┃  │     ━━━━━━━━░░  80%                  │  ┃
┃  │                                      │  ┃
┃  │  📝 Laporan                          │  ┃
┃  │     1 synced   •  0 failed           │  ┃
┃  │     ━━━━━━━━━━  100% ✅              │  ┃
┃  │                                      │  ┃
┃  │  📷 Foto                             │  ┃
┃  │     10 pending •  0 failed           │  ┃
┃  │     ━━━░░░░░░░  30%                  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  📶  Online                           │  ┃
┃  │  Koneksi stabil                      │  ┃
┃  └──────────────────────────────────────┘  ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Changes from State 1:**
- Status card: Amber background (#FFF9C4), loading icon
- Progress bar: Shows 70% completion
- Progress text: "Menyinkronkan 7 dari 10 item"
- Button: Changed to "BATALKAN SINKRONISASI" (cancel option)
- Accordion: EXPANDED, showing per-entity progress
- Each entity: Shows pending count + progress bar

**Entity Progress:**
- Perusahaan: 2 pending, 60%
- Kontak: 3 pending, 70%
- Proyek: 1 pending, 80%
- Laporan: 1 synced (100% ✅)
- Foto: 10 pending, 30%

**Interactions:**
- Tap "BATALKAN SINKRONISASI" → Cancel ongoing sync
- Auto-refresh progress every 1 second
- When complete → Transition to State 1 (All Synced)

---

### Wireframe 3: Sync Status - Partial Failed

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Status Sinkronisasi          🔄      ┃
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  ⚠️  5 item gagal disinkronkan       │  ┃ ← Warning state
┃  │                                      │  ┃   Background: #FFEBEE (red-50)
┃  │  10 item berhasil disinkronkan       │  ┃   Border: #EF5350 (red-400)
┃  │                                      │  ┃
┃  │  Terakhir disinkronisasi:            │  ┃
┃  │  12 Jan 2025, 14:40                  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │    COBA LAGI                         │  ┃ ← Primary Button (retry)
┃  └──────────────────────────────────────┘  ┃   Background: #2E7D32
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Detail Sinkronisasi            ▲    │  ┃ ← Accordion (EXPANDED)
┃  ├──────────────────────────────────────┤  ┃   Show which entities failed
┃  │  📊 Perusahaan                       │  ┃
┃  │     0 pending  •  2 failed  ❌       │  ┃ ← Failed indicator
┃  │     Tap untuk lihat detail           │  ┃   Tappable to see which ones
┃  │                                      │  ┃
┃  │  👤 Kontak                           │  ┃
┃  │     0 pending  •  0 failed  ✅       │  ┃ ← Success indicator
┃  │                                      │  ┃
┃  │  📁 Proyek                           │  ┃
┃  │     0 pending  •  0 failed  ✅       │  ┃
┃  │                                      │  ┃
┃  │  📝 Laporan                          │  ┃
┃  │     0 pending  •  0 failed  ✅       │  ┃
┃  │                                      │  ┃
┃  │  📷 Foto                             │  ┃
┃  │     0 pending  •  3 failed  ❌       │  ┃
┃  │     Tap untuk lihat detail           │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  📶  Online                           │  ┃
┃  │  Koneksi stabil                      │  ┃
┃  └──────────────────────────────────────┘  ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Changes from State 2:**
- Status card: Red background (#FFEBEE), warning icon
- Shows failed count (5 items) + success count (10 items)
- Button: "COBA LAGI" (retry failed items only)
- Accordion: EXPANDED, showing which entities failed
- Failed entities: Red ❌ indicator + "Tap untuk lihat detail"

**Failed Items:**
- Perusahaan: 2 failed (e.g., "PT Surya Indah", "PT Maju Jaya")
- Foto: 3 failed (e.g., "photo_001.jpg", "photo_005.jpg", "photo_009.jpg")

**Interactions:**
- Tap "COBA LAGI" → Retry only failed items
- Tap "Perusahaan" row → Expand to show list of failed companies with error messages
- Tap "Foto" row → Expand to show list of failed photos with upload progress

**Error Messages (examples):**
- "Server timeout (504)"
- "File too large (max 5MB)"
- "Koneksi terputus"

---

### Wireframe 4: Sync Status - Offline

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Status Sinkronisasi          🔄      ┃
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  📵  Anda sedang offline             │  ┃ ← Offline state
┃  │                                      │  ┃   Background: #ECEFF1 (gray-50)
┃  │  15 item menunggu sinkronisasi       │  ┃   Border: #90A4AE (gray-400)
┃  │                                      │  ┃
┃  │  Data akan disinkronkan otomatis     │  ┃
┃  │  saat koneksi internet tersedia.     │  ┃
┃  │                                      │  ┃
┃  │  Terakhir disinkronisasi:            │  ┃
┃  │  12 Jan 2025, 10:15                  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │    COBA SINKRONKAN                   │  ┃ ← Primary Button (DISABLED)
┃  └──────────────────────────────────────┘  ┃   Background: #E0E0E0 (gray)
┃                                            ┃   Text: #9E9E9E
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Detail Sinkronisasi            ▲    │  ┃
┃  ├──────────────────────────────────────┤  ┃
┃  │  📊 Perusahaan                       │  ┃
┃  │     3 pending  •  0 failed           │  ┃ ← Pending count
┃  │                                      │  ┃
┃  │  👤 Kontak                           │  ┃
┃  │     5 pending  •  0 failed           │  ┃
┃  │                                      │  ┃
┃  │  📁 Proyek                           │  ┃
┃  │     2 pending  •  0 failed           │  ┃
┃  │                                      │  ┃
┃  │  📝 Laporan                          │  ┃
┃  │     4 pending  •  0 failed           │  ┃
┃  │                                      │  ┃
┃  │  📷 Foto                             │  ┃
┃  │     1 pending  •  0 failed           │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  📵  Offline                          │  ┃ ← Network indicator
┃  │  Tidak ada koneksi internet          │  ┃   Background: #ECEFF1
┃  └──────────────────────────────────────┘  ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Changes from State 1:**
- Status card: Gray background, offline icon
- Shows pending items count (15 items waiting)
- Informational text: Auto-sync when online
- Button: DISABLED (cannot sync while offline)
- Accordion: Shows pending counts per entity
- Network indicator: OFFLINE status (gray background)

**Interactions:**
- Tap button → No action (disabled)
- Tap pending entity → See list of pending items (read-only preview)
- When network reconnects → Auto-transition to State 2 (Syncing)

---

## Screen 29: Settings Screen

### User Story
Not explicitly in MVP user stories, tapi required untuk app configuration.

### Purpose
Allow users to configure app behavior (auto-sync, language, etc.) dan access logout.

---

### Wireframe 5: Settings Screen - Default

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Pengaturan                            ┃ ← Top App Bar
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃   Title: Pengaturan
┃                                            ┃
┃  AKUN                                      ┃ ← Section Header
┃  ┌──────────────────────────────────────┐  ┃   Text: Label Medium (12sp)
┃  │  Email                               │  ┃   Color: Text Secondary
┃  │  budi@cssgroup.co.id                 │  ┃
┃  │                                      │  ┃ ← List Item (2-line)
┃  │  Role                                │  ┃   Height: 72dp
┃  │  Sales Representative                │  ┃   Read-only (no chevron)
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  SINKRONISASI                              ┃ ← Section Header
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Auto-sync                       ⚪ │  ┃ ← Toggle Switch (ON)
┃  └──────────────────────────────────────┘  ┃   Height: 56dp
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Sync via WiFi only              ⚪ │  ┃ ← Toggle Switch (ON)
┃  └──────────────────────────────────────┘  ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Sync interval                    ▶  │  ┃ ← List Item (navigates)
┃  │  Setiap 15 menit                     │  ┃   Shows current value
┃  └──────────────────────────────────────┘  ┃   Chevron indicates sub-screen
┃                                            ┃
┃  APLIKASI                                  ┃ ← Section Header
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Bahasa                           ▶  │  ┃
┃  │  Bahasa Indonesia                    │  ┃
┃  └──────────────────────────────────────┘  ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Tentang Aplikasi                 ▶  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  AKUN                                      ┃ ← Section Header
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Logout                              │  ┃ ← Destructive action
┃  └──────────────────────────────────────┘  ┃   Text: Red (#D32F2F)
┃                                            ┃   Height: 56dp
┃                                            ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Dimensions:**
- Screen margin: 0dp (full width list items)
- Section headers: 48dp height, 16dp left padding
- List items: 56-72dp height, 16dp left/right padding
- Toggle switches: 48dp width
- Dividers: 1dp height, #E0E0E0

**Settings Values:**
- Email: budi@cssgroup.co.id (read-only)
- Role: Sales Representative (read-only)
- Auto-sync: ON (toggle)
- Sync via WiFi only: ON (toggle)
- Sync interval: Setiap 15 menit (dropdown: 5 min | 15 min | 30 min | Manual only)
- Bahasa: Bahasa Indonesia (dropdown: Bahasa Indonesia | English)

**Interactions:**
- Tap toggle switch → Toggle ON/OFF with animation
- Tap "Sync interval" → Navigate to sub-screen with 4 radio options
- Tap "Bahasa" → Navigate to language picker (2 options)
- Tap "Tentang Aplikasi" → Navigate to About screen
- Tap "Logout" → Show confirmation dialog (Wireframe 6)

---

### Wireframe 6: Settings - Logout Confirmation Dialog

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Pengaturan                            ┃
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃
┃                                            ┃
┃        ┌─────────────────────────────┐     ┃ ← Scrim (dimmed background)
┃        │                             │     ┃   Opacity: 60%
┃        │  Keluar dari Akun?          │     ┃
┃        │                             │     ┃ ← Alert Dialog
┃        │  Anda yakin ingin keluar?   │     ┃   Width: 280dp
┃        │                             │     ┃   Elevation: 8dp
┃        │  Data yang belum             │     ┃   Corner radius: 8dp
┃        │  disinkronkan akan tetap     │     ┃
┃        │  tersimpan di perangkat ini. │     ┃ ← Warning text
┃        │                             │     ┃   Body Medium (14sp)
┃        │                             │     ┃   Color: Text Secondary
┃        │  ┌────────┐  ┌────────────┐ │     ┃
┃        │  │ BATAL  │  │   KELUAR   │ │     ┃ ← Action buttons
┃        │  └────────┘  └────────────┘ │     ┃   Batal: Text button
┃        │                             │     ┃   Keluar: Text button (red)
┃        └─────────────────────────────┘     ┃
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
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Dialog Elements:**
- Title: "Keluar dari Akun?" (Title Large, 22sp)
- Body text: Warning about unsync'd data
- Buttons:
  - "BATAL" (secondary, gray text)
  - "KELUAR" (destructive, red text #D32F2F)

**Interactions:**
- Tap "BATAL" → Dismiss dialog, stay on Settings
- Tap "KELUAR" → Clear auth token, navigate to Login screen
- Tap outside dialog (scrim) → Dismiss dialog

---

## Screen 30: Profile Screen

### User Story
Not in MVP user stories, tapi useful untuk user info display.

### Purpose
Display user profile information, stats, dan quick actions.

---

### Wireframe 7: Profile Screen - Sales Rep View

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Profil                                ┃ ← Top App Bar
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃
┃                                            ┃
┃                ┌─────┐                     ┃ ← 24dp top margin
┃                │     │                     ┃
┃                │  BW │                     ┃ ← Avatar (circle, 80dp)
┃                │     │                     ┃   Background: Primary
┃                └─────┘                     ┃   Text: White, 24sp
┃                                            ┃   Shows initials
┃           Budi Wijaya                      ┃ ← 8dp below avatar
┃                                            ┃   Title Large (22sp)
┃      Sales Representative                  ┃ ← 4dp below name
┃                                            ┃   Badge (rounded chip)
┃     budi@cssgroup.co.id                    ┃   Background: #E8F5E9
┃                                            ┃   Text: Primary
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃ ← 24dp spacing
┃  │  📊 STATISTIK                        │  ┃ ← Stats section
┃  │                                      │  ┃   Elevated card
┃  │  ┌────────────┬────────────┬───────┐ │  ┃
┃  │  │   45       │    12      │   8   │ │  ┃ ← 3-column grid
┃  │  │ Laporan    │ Perusahaan │Proyek │ │  ┃   Numbers: Title Large
┃  │  └────────────┴────────────┴───────┘ │  ┃   Labels: Body Medium
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  AKSI CEPAT                                ┃ ← Section header
┃  ┌──────────────────────────────────────┐  ┃
┃  │  ⚙️  Pengaturan                    ▶  │  ┃ ← List item (navigates)
┃  └──────────────────────────────────────┘  ┃   Height: 56dp
┃  ┌──────────────────────────────────────┐  ┃
┃  │  ❓  Bantuan                       ▶  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  ℹ️  Tentang Aplikasi              ▶  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃ ← 16dp spacing
┃  │         KELUAR                       │  ┃ ← Logout button (outlined)
┃  └──────────────────────────────────────┘  ┃   Height: 48dp
┃                                            ┃   Border: #D32F2F (red)
┃                                            ┃   Text: #D32F2F
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Dimensions:**
- Avatar: 80dp diameter, centered
- Name: Title Large (22sp), centered
- Role badge: Height 24dp, padding 8dp horizontal
- Stats card: Full width minus 32dp, padding 16dp
- Stats columns: Equal width, 16dp spacing
- List items: 56dp height

**Profile Data (Sales Rep):**
- Name: Budi Wijaya
- Role: Sales Representative
- Email: budi@cssgroup.co.id
- Stats:
  - Laporan: 45 (total reports created)
  - Perusahaan: 12 (total companies owned)
  - Proyek: 8 (total projects owned)

**Interactions:**
- Avatar: Non-interactive (no photo upload in MVP)
- Tap "Pengaturan" → Navigate to Settings screen (Screen 29)
- Tap "Bantuan" → Navigate to Help screen (external or webview)
- Tap "Tentang Aplikasi" → Navigate to About screen
- Tap "KELUAR" → Show logout confirmation dialog

---

### Wireframe 8: Profile Screen - Manager View

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Profil                                ┃
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃
┃                                            ┃
┃                ┌─────┐                     ┃
┃                │     │                     ┃
┃                │  AS │                     ┃ ← Manager initials
┃                │     │                     ┃
┃                └─────┘                     ┃
┃                                            ┃
┃          Ahmad Sutrisno                    ┃
┃                                            ┃
┃             Manager                        ┃ ← Manager badge
┃                                            ┃   Background: #E3F2FD (blue-50)
┃     ahmad@cssgroup.co.id                   ┃   Text: #1976D2 (blue-700)
┃                                            ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  📊 TIM                              │  ┃ ← Team stats (Manager only)
┃  │                                      │  ┃
┃  │  ┌────────────┬────────────┬───────┐ │  ┃
┃  │  │   150      │    5       │  45   │ │  ┃
┃  │  │ Laporan    │ Sales Rep  │Proyek │ │  ┃
┃  │  │ (Minggu)   │  (Aktif)   │(Aktif)│ │  ┃
┃  │  └────────────┴────────────┴───────┘ │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  AKSI CEPAT                                ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  ⚙️  Pengaturan                    ▶  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  ❓  Bantuan                       ▶  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  ℹ️  Tentang Aplikasi              ▶  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │         KELUAR                       │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Differences from Sales Rep View:**
- Role badge: "Manager" (blue background, blue text)
- Stats section: **TIM** (team stats, not personal)
  - Laporan (Minggu): 150 (all team reports this week)
  - Sales Rep (Aktif): 5 (active sales reps in team)
  - Proyek (Aktif): 45 (all active projects)

**Manager Stats:**
- Show team-level data, not personal
- Time-based: "Minggu ini" (this week)
- Read-only (no edit capability)

---

## Screen 31: Onboarding

### User Story
Not in MVP user stories, tapi important untuk first-time user experience.

### Purpose
Introduce app features dan key benefits kepada first-time users.

---

### Wireframe 9: Onboarding - Slide 1 (Welcome)

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃                                      Skip ┃ ← Skip button (top right)
┃                                            ┃   Text: Body Large, Primary
┃                                            ┃
┃                                            ┃
┃              ┌───────────┐                 ┃ ← 80dp top margin
┃              │           │                 ┃
┃              │           │                 ┃
┃              │   [IMG]   │                 ┃ ← Illustration (200×200dp)
┃              │           │                 ┃   Simple, 1-color
┃              │  Welcome  │                 ┃   Green (#2E7D32)
┃              │           │                 ┃
┃              └───────────┘                 ┃
┃                                            ┃
┃                                            ┃ ← 32dp spacing
┃      Selamat datang di                     ┃ ← Title Large (22sp)
┃      CSS Sales Report!                     ┃   Centered
┃                                            ┃   Color: Text Primary
┃                                            ┃
┃   Kelola laporan kunjungan customer        ┃ ← Body Large (16sp)
┃   dengan mudah dan terstruktur.            ┃   Centered
┃                                            ┃   Color: Text Secondary
┃                                            ┃   Padding: 0 32dp
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃              ● ○ ○ ○                       ┃ ← Page indicators
┃                                            ┃   4 dots, 8dp size
┃                                            ┃   Active: Primary
┃                                            ┃   Inactive: Gray-300
┃                                            ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃ ← 24dp bottom margin
┃  │           LANJUT                     │  ┃ ← Primary Button
┃  └──────────────────────────────────────┘  ┃   Height: 48dp
┃                                            ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Dimensions:**
- Illustration: 200×200dp, centered
- Title: Title Large (22sp), max 2 lines
- Body: Body Large (16sp), max 3 lines, 32dp side padding
- Page indicators: 8dp diameter, 8dp spacing
- Button: Full width minus 48dp, 24dp bottom margin

**Content:**
- Slide 1: Welcome + general intro
- Illustration: Welcome graphic (handshake or checkmark)

**Interactions:**
- Tap "Skip" → Navigate to Login (skip all slides)
- Tap "LANJUT" → Swipe to Slide 2
- Swipe left → Next slide
- Swipe right → Previous slide (disabled on Slide 1)

---

### Wireframe 10: Onboarding - Slide 2 (Easy Reports)

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃                                      Skip ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃              ┌───────────┐                 ┃
┃              │           │                 ┃
┃              │           │                 ┃
┃              │   [IMG]   │                 ┃ ← Illustration: Form/checklist
┃              │           │                 ┃
┃              │   Easy    │                 ┃
┃              │  Reports  │                 ┃
┃              └───────────┘                 ┃
┃                                            ┃
┃                                            ┃
┃      Buat laporan kunjungan                ┃
┃      dengan mudah                          ┃
┃                                            ┃
┃   Form terstruktur dengan auto-save,       ┃
┃   foto, dan GPS location.                  ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃              ○ ● ○ ○                       ┃ ← Page 2 active
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │           LANJUT                     │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Content:**
- Slide 2: Easy reports feature
- Illustration: Form/checklist icon
- Highlight: Structured forms, auto-save, photos, GPS

---

### Wireframe 11: Onboarding - Slide 3 (Offline First)

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃                                      Skip ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃              ┌───────────┐                 ┃
┃              │           │                 ┃
┃              │           │                 ┃
┃              │   [IMG]   │                 ┃ ← Illustration: Cloud sync
┃              │           │                 ┃
┃              │  Offline  │                 ┃
┃              │   First   │                 ┃
┃              └───────────┘                 ┃
┃                                            ┃
┃                                            ┃
┃      Bekerja offline,                      ┃
┃      sync otomatis saat online             ┃
┃                                            ┃
┃   Tidak ada koneksi internet? Tidak        ┃
┃   masalah! Data tersimpan lokal dan        ┃
┃   sync otomatis saat online.               ┃
┃                                            ┃
┃                                            ┃
┃              ○ ○ ● ○                       ┃ ← Page 3 active
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │           LANJUT                     │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Content:**
- Slide 3: Offline-first feature (CRITICAL selling point)
- Illustration: Cloud with sync arrows
- Highlight: No internet needed, auto-sync when online

---

### Wireframe 12: Onboarding - Slide 4 (Get Started)

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃                                            ┃ ← No "Skip" (last slide)
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃              ┌───────────┐                 ┃
┃              │           │                 ┃
┃              │           │                 ┃
┃              │   [IMG]   │                 ┃ ← Illustration: Rocket/Start
┃              │           │                 ┃
┃              │   Ready   │                 ┃
┃              │     !     │                 ┃
┃              └───────────┘                 ┃
┃                                            ┃
┃                                            ┃
┃      Mari mulai!                           ┃
┃                                            ┃
┃                                            ┃
┃   Login dengan akun CSS Anda untuk         ┃
┃   mulai membuat laporan.                   ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃              ○ ○ ○ ●                       ┃ ← Page 4 active
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │           MULAI                      │  ┃ ← Button text: "MULAI"
┃  └──────────────────────────────────────┘  ┃   (not "LANJUT")
┃                                            ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Content:**
- Slide 4: Call to action
- Illustration: Rocket or start flag
- Button: "MULAI" (changed from "LANJUT")

**Interactions:**
- Tap "MULAI" → Navigate to Login screen
- No "Skip" button (already on last slide)
- Swipe right → Previous slide

---

## Design System Notes

### Material Design 3 Components Used

**Sync Status Screen:**
- Cards: Elevated cards with 2dp elevation
- Progress bars: Linear progress indicator (Material 3)
- Accordion: Expandable list items
- Badges: Status chips (success/error/warning)

**Settings Screen:**
- List items: Material 3 list with dividers
- Toggle switches: Material 3 switch component
- Dialogs: Material 3 alert dialog

**Profile Screen:**
- Avatar: Material 3 circular avatar
- Stats cards: Elevated card with grid layout
- List items: Standard Material list

**Onboarding:**
- Illustrations: Simple, single-color SVG graphics
- Page indicators: Material 3 dot indicators
- Buttons: Material 3 filled button

### Accessibility

**Sync Status:**
- Status icons with text labels (not icon-only)
- Color + icon for failed items (not color alone)
- Clear error messages
- Retry actions clearly labeled

**Settings:**
- Toggle switches: Clear ON/OFF labels
- List items: Minimum 56dp height (touch target)
- Confirmation dialog for destructive actions

**Profile:**
- Avatar: Uses initials if no photo
- Stats: Clear labels with numbers
- High contrast text

**Onboarding:**
- Skip button: Always visible (except last slide)
- Swipe optional (button navigation available)
- Clear page indicators

---

## Implementation Notes

### Sync Status
1. **Real-time Updates:**
   - Poll sync status every 3 seconds during active sync
   - Use WebSocket or long-polling for live progress
   - Update UI without full refresh

2. **Error Handling:**
   - Group errors by type (network, server, validation)
   - Show detailed error per failed item
   - Allow retry per-entity or per-item

3. **Performance:**
   - Lazy-load transaction log (don't load all on init)
   - Cache last sync status for instant display

### Settings
1. **Persistence:**
   - Save settings to SharedPreferences
   - Sync settings to server (optional)
   - Apply settings immediately (no "Save" button needed)

2. **Sync Interval:**
   - If set to "Manual only", disable auto-sync
   - Show warning if WiFi-only + currently on cellular

### Profile
1. **Stats Calculation:**
   - Sales rep: Count from local SQLite (owned entities)
   - Manager: Fetch from API (aggregated team stats)
   - Cache stats for 5 minutes

### Onboarding
1. **Show Once:**
   - Set flag in SharedPreferences after completion
   - Never show again unless app data cleared

2. **Illustrations:**
   - Use simple SVG illustrations (lightweight)
   - Fallback to Material icons if custom illustrations unavailable

---

## Related Documents

- **Screen Inventory:** [SCREEN_INVENTORY.md](./SCREEN_INVENTORY.md) (Screens 28-31)
- **User Stories:** [USER_STORIES.md](../developer-brief/USER_STORIES.md) (US-7.2, US-7.3)
- **Design Requirements:** [DESIGN_REQUIREMENTS.md](./DESIGN_REQUIREMENTS.md)

---

## Next Steps

➡️ **[Continue to: WIREFRAMES_COMPANIES.md →](./WIREFRAMES_COMPANIES.md)**

**Or navigate to:**
← [WIREFRAMES_AUTHENTICATION.md](./WIREFRAMES_AUTHENTICATION.md)
