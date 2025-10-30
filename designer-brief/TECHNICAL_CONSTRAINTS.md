# Technical Constraints - Hal Teknis Yang Harus Anda Ketahui

← [Sebelumnya: SCREEN_INVENTORY](./SCREEN_INVENTORY.md)

---

## 📋 Overview

Dokumen ini menjelaskan **technical constraints** yang affect design decisions Anda. Ini bukan technical jargon yang rumit - ini adalah hal-hal praktis yang HARUS Anda consider saat design.

**TL;DR:** Ada beberapa limitations yang mempengaruhi UX design. Read this carefully untuk avoid designing something yang impossible/expensive untuk implement.

---

## 🔴 CRITICAL: Offline-First Architecture

### What It Means

App ini **HARUS bekerja tanpa internet**. Ini bukan optional feature, ini adalah **core requirement**.

**Why This Matters for UX:**
- Sales reps sering di area dengan no signal (pabrik, warehouse, basement)
- Mereka tidak bisa "tunggu sampai ada WiFi" untuk buat report
- App harus jelas communicate: "You're offline, but everything still works"

### Design Implications

#### 1. **Offline Indicator (MANDATORY)**

**Requirement:** Harus ada indicator yang jelas show network status.

**Where:**
- Top App Bar (small badge: "Offline" or "Online")
- OR persistent banner (jika offline)
- Color code:
  - Green = Online & Synced
  - Blue = Online & Syncing
  - Gray = Offline (but data will sync later)
  - Red = Sync Failed (need attention)

**Example Design Pattern:**
```
┌─────────────────────────────────┐
│ Beranda       [Offline 🔴]  👤 │ ← Top app bar
├─────────────────────────────────┤
│ ⚠️ Anda offline                │ ← Banner (optional, jika offline)
│ Data akan di-sync otomatis     │
│ saat online kembali             │
├─────────────────────────────────┤
│ (content)                        │
└─────────────────────────────────┘
```

#### 2. **Sync Status Per Item (MANDATORY)**

**Requirement:** User harus tahu status sync untuk EACH report/company/contact.

**Design Pattern:**
- Badge atau icon next to item di list
- Status options:
  - ✓ "Tersinkronisasi" (green check) - data sudah di server
  - ⟳ "Menyinkronkan..." (blue spinner) - currently uploading
  - ⚠️ "Pending" (amber clock) - waiting to sync
  - ✗ "Gagal sync" (red X + retry button) - sync failed, manual retry needed

**Example in List:**
```
┌────────────────────────────────┐
│ PT Surya Indah        ✓       │ ← Synced (green)
│ Jakarta Selatan                │
├────────────────────────────────┤
│ PT Maju Jaya          ⟳       │ ← Syncing (blue spinner)
│ Tangerang                      │
├────────────────────────────────┤
│ PT Sentosa            ⚠️       │ ← Pending sync (amber)
│ Bekasi                         │
├────────────────────────────────┤
│ PT Global             ✗ Retry │ ← Failed (red + action)
│ Bandung                        │
└────────────────────────────────┘
```

#### 3. **Draft Auto-Save (MANDATORY)**

**Requirement:** Forms harus auto-save every 30 seconds ke local storage.

**Why:** Jika user keluar app tanpa sengaja (phone call masuk, battery mati), data tidak hilang.

**Design Pattern:**
- Small indicator di bottom form: "Terakhir disimpan 1 menit yang lalu" (gray text, 12sp)
- NO spinner (happens silently in background)
- Jika save failed (unlikely), show warning: "Gagal menyimpan draft"

**Example:**
```
┌────────────────────────────────┐
│ (Form content)                  │
│                                 │
│ [Batal]         [Simpan]       │ ← Action buttons
├─────────────────────────────────┤
│ Terakhir disimpan 32 detik lalu│ ← Auto-save indicator (small, subtle)
└─────────────────────────────────┘
```

#### 4. **Optimistic UI Updates**

**Requirement:** Ketika user create/edit data offline, UI harus update immediately (jangan tunggu sync).

**Why:** User expect instant feedback. "Saving..." yang lama = frustrating.

**Design Pattern:**
- Create company → immediately show di list dengan "Pending" badge
- User tidak perlu tunggu sync complete untuk continue work
- If sync fails later → show notification + keep data local (don't delete!)

---

## 🔴 CRITICAL: Pagination (Performance Requirement)

### What It Means

Lists **HARUS** paginate (max 20 items per page). Tidak bisa show semua 100+ items sekaligus.

**Why:** Performance. Phone akan freeze/lag jika render 100+ items di scroll list.

### Design Implications

#### **Don't Design:**
- ❌ Infinite scroll dengan load all items
- ❌ "Show All" button yang load 100 items at once

#### **DO Design:**
- ✅ Load 20 items per page
- ✅ "Load More" button (di bottom list)
- ✅ OR auto-load on scroll (but still batch by 20)
- ✅ Show count: "Menampilkan 20 dari 150 perusahaan"

**Example:**
```
┌────────────────────────────────┐
│ Companies (150)                 │
├────────────────────────────────┤
│ (20 company cards)              │
│                                 │
├────────────────────────────────┤
│ Menampilkan 20 dari 150        │
│ [Muat Lebih Banyak] ← Button  │
└────────────────────────────────┘
```

---

## 🟡 IMPORTANT: Photo Upload Management

### What It Means

Reports bisa have up to **10 photos**. Each photo harus show individual upload status.

**Why:** Network might fail mid-upload. Photo 1-5 success, photo 6 failed, photo 7-10 pending. User harus tahu which photos need retry.

### Design Implications

#### **Per-Photo Status Tracking (MANDATORY)**

**Design Pattern:**
Di Create Report - Step 3 (Add Photos):

```
┌────────────────────────────────┐
│ Foto Laporan (3 dari 10)       │
├────────────────────────────────┤
│ ┌─────┐ ┌─────┐ ┌─────┐       │
│ │Photo│ │Photo│ │Photo│       │
│ │  1  │ │  2  │ │  3  │       │
│ │  ✓  │ │ ⟳45%│ │  ✗  │       │ ← Status per photo
│ └─────┘ └─────┘ └─────┘       │
│ Success Uploading Failed       │
└────────────────────────────────┘
```

**Status Options:**
- ✓ Green checkmark = Uploaded successfully
- ⟳ + % = Uploading (show progress 0-100%)
- ⚠️ Amber clock = Pending upload (waiting)
- ✗ Red X + "Retry" = Failed (tap to retry individual photo)

#### **Photo Compression (Background)**

**Requirement:** Photos di-compress automatically ke <500KB.

**Design Pattern:**
- NO UI untuk compression process (happens automatically)
- Optional: Show toast "Foto dikompresi untuk menghemat data" (first time only)
- User tidak perlu tahu technical details

---

## 🟡 IMPORTANT: Material Design 3 Components Only

### What It Means

Developer akan gunakan **Material Design 3** standard components. Custom components = expensive & slow.

**Why This Matters:**
- Jika Anda design custom button/card/dialog yang TIDAK ada di Material Design 3 → developer harus build from scratch = $$$
- Standard Material components = faster development = cheaper = less bugs

### Design Implications

#### **Use Standard Material Components:**

**Buttons:**
- ✅ Filled Button (primary action)
- ✅ Outlined Button (secondary action)
- ✅ Text Button (tertiary action)
- ✅ FAB (Floating Action Button)
- ✅ Icon Button
- ❌ Custom gradient buttons
- ❌ Neumorphic buttons
- ❌ Buttons with complex shadows/animations

**Forms:**
- ✅ Text Field (outlined variant)
- ✅ Dropdown Menu (Material Dropdown)
- ✅ Date Picker (Material Date Picker)
- ✅ Checkbox (Material Checkbox)
- ✅ Radio Button
- ❌ Custom fancy date picker with custom calendar design
- ❌ Multi-step form wizard dengan custom stepper

**Lists & Cards:**
- ✅ Material Card (elevated or outlined)
- ✅ List Item (1-line, 2-line, 3-line)
- ✅ Divider
- ❌ Custom card dengan complex shadows/glassmorphism
- ❌ Custom list item layout yang TIDAK follow Material specs

**Navigation:**
- ✅ Top App Bar
- ✅ Bottom Navigation Bar (3-5 items)
- ✅ Navigation Drawer (optional)
- ❌ Custom tab bar design
- ❌ Custom hamburger menu animation

**Reference:**
- Material Design 3 documentation: https://m3.material.io/
- Figma Material Design 3 Kit: https://www.figma.com/community/file/1035203688168086460

---

## 🟡 IMPORTANT: Platform Constraints

### Android Only (No iOS)

**Implications:**
- ❌ Don't design iOS-style components (iOS switches, iOS navigation patterns)
- ✅ Use Android back button (hardware back button)
- ✅ Use Material Design (Android design language)

### Portrait Only (No Landscape)

**Implications:**
- Design untuk portrait orientation (360 × 800 dp)
- Tidak perlu design landscape variant
- Lock orientation to portrait di developer implementation

### Minimum SDK 21 (Android 5.0 from 2014)

**Implications:**
- Some old phones have small screens (5 inch)
- Some old phones have weak processors
- **Design Considerations:**
  - Touch targets minimum 48 × 48 dp (big enough for fingers)
  - Don't design complex animations (old phones can't handle)
  - Use simple transitions (fade, slide - no fancy spring animations)

---

## 🟢 NICE TO KNOW: Form Validation

### Real-time vs Submit Validation

**Requirement:** Validation errors harus show AFTER user blur input field (not while typing).

**Why:** Showing error while user still typing = annoying.

**Design Pattern:**
```
User flow:
1. User taps Nama Perusahaan field
2. User types "P" → no error yet
3. User taps next field (blur event) → now check validation
4. If empty/invalid → show red error text below field + red border
```

**Example:**
```
┌────────────────────────────────┐
│ Nama Perusahaan *              │
│ ┌────────────────────────────┐ │
│ │                            │ │ ← Normal state (gray border)
│ └────────────────────────────┘ │
│                                 │
│ Nama Perusahaan *              │
│ ┌────────────────────────────┐ │
│ │                            │ │ ← Error state (red border)
│ └────────────────────────────┘ │
│ ⚠️ Nama perusahaan wajib diisi│ ← Error message (red text, 12sp)
└────────────────────────────────┘
```

---

## 🟢 NICE TO KNOW: Currency Format

### Input Format

**Requirement:** User input currency harus dengan thousand separators.

**Format:** Rp 50.000.000 (titik sebagai thousand separator, Indonesian format)

**Design Pattern:**
- Input field dengan prefix "Rp" (gray text)
- Auto-format as user types (add thousand separators)
- Example: User types "50000000" → show as "Rp 50.000.000"

**Technical Note:**
- Stored as INTEGER di database (bukan floating point) untuk precision
- Designer tidak perlu worry tentang ini - just design input dengan "Rp" prefix dan thousand separators

---

## 🟢 NICE TO KNOW: GPS Auto-Capture

### How It Works

**Requirement:** Saat user create report, app auto-capture GPS coordinates (if permission granted).

**Design Pattern:**
- **Permission Granted:**
  - Show: "Lokasi: -6.2088, 106.8456 ✓" (green check)
  - Optional: Show small map preview (static, not interactive)

- **Permission Denied or GPS Unavailable:**
  - Show: "Lokasi tidak tersedia" (gray text)
  - NO error message (this is optional field)
  - NO popup asking for permission again (annoying)

**Don't Design:**
- ❌ Manual GPS input (lat/long input fields)
- ❌ "Request Permission" button di report form
- ❌ Big error message if GPS not available

**DO Design:**
- ✅ Simple indicator: Tersedia atau Tidak Tersedia
- ✅ If available → show coordinates + optional map preview
- ✅ Subtle, not prominent (GPS is nice-to-have, not critical)

---

## ⚠️ Common Mistakes to Avoid

### ❌ DON'T Design:

1. **Real-Time Sync Animations**
   - Example: Live updating dashboard ketika data sync
   - Why: Complex to implement, not critical for MVP
   - Instead: Show sync status badge, user can manual refresh

2. **Complex Multi-Step Wizards dengan Progress Bars**
   - Example: 10-step wizard dengan animated progress bar
   - Why: Hard to maintain, frustrating UX
   - Instead: Max 4 steps, simple stepper indicator

3. **Custom Animations & Transitions**
   - Example: Spring animations, bounce effects, parallax scrolling
   - Why: Expensive to develop, performance issues on old phones
   - Instead: Simple fade/slide transitions (Material Design default)

4. **Infinite Scroll tanpa Pagination**
   - Example: Load semua 1000 items at once
   - Why: Performance nightmare
   - Instead: Paginate 20 items per page

5. **Custom Component yang TIDAK ada di Material Design**
   - Example: Custom glassmorphism cards, neumorphic buttons
   - Why: Developer harus build from scratch = expensive
   - Instead: Use Material Design 3 components

### ✅ DO Design:

1. **Clear Offline Indicators**
   - Always show network status
   - Always show sync status per item

2. **Simple, Standard Components**
   - Use Material Design 3 library
   - Prioritize usability over uniqueness

3. **Big Touch Targets**
   - Minimum 48 × 48 dp for tappable areas
   - Users have large fingers, small buttons = frustration

4. **Clear Error Messages**
   - Tell user what went wrong AND how to fix it
   - Example: "Gagal upload foto. Periksa koneksi internet dan coba lagi." (not just "Error")

5. **Draft Auto-Save Indicators**
   - Show "Terakhir disimpan X menit yang lalu"
   - Builds trust that data won't be lost

---

## 📍 Navigasi

**Sudah paham technical constraints?**

➡️ **[Lanjut ke: USER_STORIES_FOR_DESIGNER.md →](./USER_STORIES_FOR_DESIGNER.md)**

Dokumen selanjutnya akan menjelaskan:
- User stories (simplified untuk designer)
- Acceptance criteria untuk each feature
- Detailed requirements per screen

**Atau kembali ke:**
← [SCREEN_INVENTORY](./SCREEN_INVENTORY.md)
