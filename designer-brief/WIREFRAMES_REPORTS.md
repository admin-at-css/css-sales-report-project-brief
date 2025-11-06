# Wireframes - Reports Screens

← [Sebelumnya: WIREFRAMES_PROJECTS.md](./WIREFRAMES_PROJECTS.md)

---

## Overview

Dokumen ini berisi detailed ASCII wireframes untuk **EPIC 5: Create & View Reports** (2 screens tambahan - untuk Create Report lihat NESTED_INLINE_CREATION_WIREFRAMES.md).

**Total Wireframes:** 5 wireframes
- Screen 19: Report Detail View (3 states: synced, syncing, failed)
- Screen 23: Photo Full Screen View (2 states)

**Note:** Create Report screen (Screen 18) ada di [NESTED_INLINE_CREATION_WIREFRAMES.md](./NESTED_INLINE_CREATION_WIREFRAMES.md) dengan 22 wireframes lengkap.

---

## Design Specifications

### Platform & Dimensions
- **Platform:** Android Mobile
- **Screen Size:** 360 × 800 dp
- **Orientation:** Portrait only
- **Grid System:** 8dp base unit

### Typography
- **Title Large:** 22sp, Medium weight
- **Body Large:** 16sp, Regular weight
- **Body Medium:** 14sp, Regular weight
- **Label Medium:** 12sp, Medium weight

### Colors
- **Primary:** #2E7D32 (green)
- **Success:** #4CAF50 (green-500)
- **Error:** #D32F2F (red-700)
- **Info:** #1976D2 (blue-700)
- **Text Primary:** #212121 (gray-900)
- **Text Secondary:** #757575 (gray-600)

---

## Screen 19: Report Detail View

### User Story
[US-5.2: Lihat Report Detail](../developer-brief/USER_STORIES.md#us-52-lihat-report-detail)

### Tujuan
Menampilkan informasi lengkap report dengan sync status, foto gallery, dan GPS location.

---

### Wireframe 1: Report Detail - Synced State

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Detail Laporan              📤       ┃ ← Top App Bar
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃   Back + share icon
┃                                            ┃
┃  ✅ Tersinkronisasi                        ┃ ← Sync status badge
┃                                            ┃   Green, top-right
┃  INFORMASI PROYEK                          ┃ ← Section header
┃  ┌──────────────────────────────────────┐  ┃
┃  │  PT Surya Indah                      │  ┃ ← Company name (link)
┃  │  Renovasi Pabrik Sidoarjo        →   │  ┃   Project name (link)
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  DETAIL LAPORAN                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Tipe                                │  ┃
┃  │  Follow-up Meeting                   │  ┃
┃  │                                      │  ┃
┃  │  Tanggal Kunjungan                   │  ┃
┃  │  15 Jan 2025, 10:00                  │  ┃
┃  │                                      │  ┃
┃  │  Peserta                             │  ┃
┃  │  • Budi Santoso (Primary)            │  ┃ ← List of attendees
┃  │  • Siti Aminah                       │  ┃   Primary marked
┃  │                                      │  ┃
┃  │  Catatan                             │  ┃
┃  │  Diskusi mengenai coating untuk      │  ┃ ← Multiline notes
┃  │  lantai pabrik. Client tertarik      │  ┃
┃  │  dengan protective coating...        │  ┃
┃  │                                      │  ┃
┃  │  Tindakan Selanjutnya                │  ┃
┃  │  Follow-up dengan proposal harga     │  ┃
┃  │  dalam 3 hari                        │  ┃
┃  │                                      │  ┃
┃  │  Hasil                               │  ┃
┃  │  Positive - Client interested        │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  FOTO (5)                                  ┃ ← Section header dengan count
┃  ┌──────────────────────────────────────┐  ┃
┃  │ ┌────┐ ┌────┐ ┌────┐ ┌────┐ ┌────┐  │  ┃ ← Horizontal scroll
┃  │ │[1] │ │[2] │ │[3] │ │[4] │ │[5] │  │  ┃   Thumbnail 80×80dp each
┃  │ └────┘ └────┘ └────┘ └────┘ └────┘  │  ┃   Tap untuk full screen
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  LOKASI                                    ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  ┌────────────────────────────────┐  │  ┃ ← Map view (if GPS)
┃  │  │                                │  │  ┃   120dp height
┃  │  │     🗺️  Map Preview            │  │  ┃   Static map image
┃  │  │                                │  │  ┃
┃  │  └────────────────────────────────┘  │  ┃
┃  │  Jl. Sudirman No. 123, Jakarta       │  ┃ ← Address dari GPS
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  METADATA                                  ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Dibuat oleh: Budi Wijaya            │  ┃
┃  │  Dibuat pada: 15 Jan 2025, 10:30     │  ┃
┃  │  Status: Tersinkronisasi             │  ┃
┃  └──────────────────────────────────────┘  ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Dimensi:**
- Sync status badge: Top-right corner, 8dp margin
- Info sections: Full width minus 32dp, padding 16dp
- Photo thumbnails: 80×80dp each, 8dp spacing
- Map preview: 120dp height, full width

**Elemen Detail Laporan:**
- Tipe (Report Type)
- Tanggal Kunjungan (Visit Date + Time)
- Peserta (Attendees list, primary contact marked)
- Catatan (Notes, multiline)
- Tindakan Selanjutnya (Next Action)
- Hasil (Outcome)

**Foto Gallery:**
- Horizontal scroll
- Show count: "FOTO (X)"
- Thumbnails 80×80dp
- Tap thumbnail → Full screen view (Screen 23)

**Lokasi (GPS):**
- Jika GPS available: Show static map preview + address
- Jika GPS tidak available: Show "Lokasi tidak tersedia"

**Interaksi:**
- Tap company name → Navigate ke Company Detail
- Tap project name → Navigate ke Project Detail
- Tap photo thumbnail → Navigate ke Photo Full Screen (Screen 23)
- Tap map → Open full map view (Google Maps)
- Tap share icon → Share report (export as PDF atau WhatsApp)

---

### Wireframe 2: Report Detail - Syncing State

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Detail Laporan              📤       ┃
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃
┃                                            ┃
┃  ⏳ Menyinkronkan...                       ┃ ← Sync status badge
┃                                            ┃   Blue/amber, animated
┃  INFORMASI PROYEK                          ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  PT Surya Indah                      │  ┃
┃  │  Renovasi Pabrik Sidoarjo        →   │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  DETAIL LAPORAN                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  (same content as synced state)      │  ┃
┃  │  ...                                 │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  FOTO (5)                                  ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │ ┌────┐ ┌────┐ ┌────┐ ┌────┐ ┌────┐  │  ┃
┃  │ │[1] │ │[2] │ │[3] │ │[4] │ │[5] │  │  ┃ ← Photo 3 uploading
┃  │ └────┘ └────┘ └──┬─┘ └────┘ └────┘  │  ┃   Progress indicator
┃  │               ⏳ 45%                  │  ┃   Overlay pada photo
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  LOKASI                                    ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  🗺️  Map Preview                      │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  METADATA                                  ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Dibuat oleh: Budi Wijaya            │  ┃
┃  │  Dibuat pada: 15 Jan 2025, 10:30     │  ┃
┃  │  Status: Menyinkronkan (3/5 foto)    │  ┃ ← Sync progress
┃  └──────────────────────────────────────┘  ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Perubahan dari State 1:**
- Status badge: "⏳ Menyinkronkan..." (blue, animated spinner)
- Photo yang sedang upload: Progress overlay (45%)
- Metadata status: "Menyinkronkan (3/5 foto)"

**Sync Process:**
- Report data synced first
- Photos uploaded one by one
- Show progress per photo
- Update badge saat complete

---

### Wireframe 3: Report Detail - Sync Failed State

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Detail Laporan              📤       ┃
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃
┃                                            ┃
┃  ❌ Gagal sync                             ┃ ← Sync status badge (red)
┃     [ RETRY ]                              ┃   With retry button
┃                                            ┃
┃  INFORMASI PROYEK                          ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  PT Surya Indah                      │  ┃
┃  │  Renovasi Pabrik Sidoarjo        →   │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  DETAIL LAPORAN                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  (same content)                      │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  FOTO (5)                                  ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │ ┌────┐ ┌────┐ ┌────┐ ┌────┐ ┌────┐  │  ┃
┃  │ │[1] │ │[2] │ │[3] │ │[4] │ │[5] │  │  ┃
┃  │ └────┘ └────┘ └──┬─┘ └──┬─┘ └────┘  │  ┃
┃  │               ❌    ❌              │  ┃ ← Failed photos
┃  └──────────────────────────────────────┘  ┃   Red X overlay
┃                                            ┃
┃  ⚠️ Gagal upload 2 foto                    ┃ ← Error message
┃  Server timeout. Tap RETRY untuk coba lagi │  ┃
┃                                            ┃
┃  LOKASI                                    ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  🗺️  Map Preview                      │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  METADATA                                  ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Dibuat oleh: Budi Wijaya            │  ┃
┃  │  Dibuat pada: 15 Jan 2025, 10:30     │  ┃
┃  │  Status: Gagal sync (2 foto failed)  │  ┃
┃  └──────────────────────────────────────┘  ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Perubahan dari State 2:**
- Status badge: "❌ Gagal sync" (red) + RETRY button
- Failed photos: Red X overlay
- Error message: Specific error (e.g., "Server timeout", "File too large")
- Metadata: "Gagal sync (2 foto failed)"

**Error Handling:**
- Show which photos failed
- Clear error message
- Retry button → Retry failed photos only
- Keep successfully uploaded photos

**Interaksi:**
- Tap RETRY → Re-attempt sync
- Tap failed photo → Show error detail
- Data tetap tersimpan local (tidak hilang)

---

## Screen 23: Photo Full Screen View

### User Story
[US-5.2: Lihat Report Detail](../developer-brief/USER_STORIES.md#us-52-lihat-report-detail)

### Tujuan
Menampilkan foto dalam full screen dengan zoom capability dan navigation.

---

### Wireframe 4: Photo Full Screen - Default View

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←                                  ⋮    ┃ ← Top App Bar (transparent)
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃   Back + more options
┃                                            ┃   Overlay on photo
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃            ┌──────────────┐                ┃ ← Full screen photo
┃            │              │                ┃   Fit to screen
┃            │              │                ┃   Pinch to zoom
┃            │    PHOTO     │                ┃   Swipe left/right
┃            │    FULL      │                ┃   untuk next/prev
┃            │              │                ┃
┃            │              │                ┃
┃            └──────────────┘                ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃              3 / 5                         ┃ ← Photo counter
┃                                            ┃   Bottom center
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛   Transparent overlay
```

**Dimensi:**
- Photo: Full screen width & height
- Top App Bar: Transparent background, opacity 80%
- Photo counter: Bottom center, 16dp from bottom

**Gesture Interactions:**
- **Pinch:** Zoom in/out (max 3x)
- **Swipe left:** Next photo
- **Swipe right:** Previous photo
- **Double tap:** Toggle zoom (1x ↔ 2x)
- **Single tap:** Toggle UI visibility (hide/show app bar + counter)

**Interaksi:**
- Tap back (←) → Return ke Report Detail
- Tap more (⋮) → Show options:
  - Download
  - Share
  - Delete (if allowed)

---

### Wireframe 5: Photo Full Screen - Zoomed In

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←                                  ⋮    ┃ ← App Bar (transparent)
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃
┃                                            ┃
┃  ┌────────────────────────────────────┐   ┃
┃  │                                    │   ┃
┃  │                                    │   ┃
┃  │                                    │   ┃ ← Photo zoomed 2x
┃  │         PHOTO DETAIL               │   ┃   Pan to see other areas
┃  │         (ZOOMED 2x)                │   ┃   Pinch out to zoom out
┃  │                                    │   ┃
┃  │                                    │   ┃
┃  │                                    │   ┃
┃  │                                    │   ┃
┃  │                                    │   ┃
┃  │                                    │   ┃
┃  │                                    │   ┃
┃  │                                    │   ┃
┃  │                                    │   ┃
┃  └────────────────────────────────────┘   ┃
┃                                            ┃
┃              3 / 5   🔍 2.0x               ┃ ← Counter + zoom level
┃                                            ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Perubahan dari State 1:**
- Photo: Zoomed in 2x (or up to 3x)
- Zoom indicator: "🔍 2.0x" (shows current zoom level)
- Photo extends beyond screen (pan to see other parts)

**Gesture:**
- **Pan/Drag:** Move photo saat zoomed
- **Pinch out:** Zoom out
- **Double tap:** Return to 1x zoom

---

## Design System Notes

### Material Design 3 Components

**Report Detail:**
- Cards: Outlined card untuk each section
- Badges: Custom sync status badge (color-coded)
- Photo gallery: Horizontal scroll view
- Map: Static image atau embedded map view

**Photo Full Screen:**
- App Bar: Material 3 top app bar (transparent variant)
- Photo viewer: Custom zoomable image view
- Counter: Custom overlay component

### Accessibility

**Report Detail:**
- Sync status: Icon + text (bukan hanya icon)
- Photos: Alt text per photo
- Links: Clear indication (underline atau arrow)
- Map: "Open in Maps" button jika tidak accessible

**Photo Full Screen:**
- Counter: Screen reader announces "Photo 3 of 5"
- Zoom level: Screen reader announces zoom changes
- Gestures: Alternative buttons untuk non-gesture users

---

## Implementation Notes

### Report Detail
1. **Sync Status Badge:**
   - Real-time updates via WebSocket atau polling
   - Color-coded: Green (synced), Blue (syncing), Red (failed)
   - Position: Fixed at top

2. **Photo Upload:**
   - Upload queue: One at a time
   - Progress indicator per photo
   - Retry logic: Max 3 attempts per photo
   - Store failed photos locally untuk retry later

3. **GPS Location:**
   - Use Google Maps Static API untuk preview
   - Tap map → Open Google Maps app
   - Fallback: Jika GPS tidak available, show "Lokasi tidak tersedia"

4. **Share Feature:**
   - Export as PDF (generate report dengan semua info + photos)
   - Share via WhatsApp, Email, atau other apps
   - Include company, project, date, notes, photos

### Photo Full Screen
1. **Zoom Capability:**
   - Use PhotoView library atau similar
   - Min zoom: Fit to screen
   - Max zoom: 3x
   - Smooth animation

2. **Photo Navigation:**
   - Swipe gesture (left/right)
   - Optional: Thumbnail strip at bottom
   - Circular navigation: Last photo → swipe right → First photo

3. **Performance:**
   - Load full-res photo only saat tap (not in thumbnail view)
   - Cache full-res photos untuk faster navigation
   - Progressive loading (show blur → sharp)

---

## Related Documents

- **Screen Inventory:** [SCREEN_INVENTORY.md](./SCREEN_INVENTORY.md) (Screens 19, 23)
- **User Stories:** [USER_STORIES.md](../developer-brief/USER_STORIES.md) (US-5.2)
- **Create Report:** [NESTED_INLINE_CREATION_WIREFRAMES.md](./NESTED_INLINE_CREATION_WIREFRAMES.md) (Screen 18 wireframes)
- **Design Requirements:** [DESIGN_REQUIREMENTS.md](./DESIGN_REQUIREMENTS.md)

---

## Next Steps

➡️ **[Continue to: WIREFRAMES_DASHBOARD.md →](./WIREFRAMES_DASHBOARD.md)**

**Or navigate to:**
← [WIREFRAMES_PROJECTS.md](./WIREFRAMES_PROJECTS.md)
