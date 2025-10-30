# Design Requirements - Specifications Detail

← [Sebelumnya: USER_STORIES_FOR_DESIGNER](./USER_STORIES_FOR_DESIGNER.md)

---

## 📋 Overview

Dokumen ini berisi **detailed specifications** untuk design deliverables Anda.

**Sections:**
1. States Requirements (empty, error, loading, success)
2. Component Specifications (sizes, colors, behaviors)
3. Accessibility Requirements (WCAG compliance)
4. Export Specifications (developer handoff)

---

## 🎨 Part 1: States Requirements

### Empty States (8 Designs Required)

**Pattern untuk Semua Empty States:**
```
┌─────────────────────────────────┐
│                                  │
│        [Illustration/Icon]       │ ← Simple 1-color illustration or Material Icon, 80x80dp
│                                  │
│        Primary Message           │ ← Heading, 16sp, medium weight, neutral-900
│     Secondary explanation        │ ← Body text, 14sp, regular, neutral-600
│                                  │
│     [Call-to-Action Button]      │ ← Primary button (if applicable)
│                                  │
└─────────────────────────────────┘
```

**Specific Empty States:**

#### 1. Empty Companies List
- **Icon:** business (Material Icon, 80dp, neutral-400)
- **Heading:** "Belum ada perusahaan"
- **Body:** "Tap tombol + di bawah untuk menambahkan perusahaan pertama Anda"
- **CTA:** None (FAB sudah visible)

#### 2. Empty Contacts List
- **Icon:** person (Material Icon)
- **Heading:** "Belum ada kontak"
- **Body:** "Tambahkan kontak untuk perusahaan ini"
- **CTA:** "Tambah Kontak" button (primary)

#### 3. Empty Projects List
- **Icon:** folder (Material Icon)
- **Heading:** "Belum ada proyek"
- **Body:** "Tambahkan proyek untuk track sales opportunity"
- **CTA:** "Tambah Proyek" button

#### 4. Empty Reports List
- **Icon:** description (Material Icon)
- **Heading:** "Belum ada laporan"
- **Body:** "Buat laporan kunjungan pertama Anda"
- **CTA:** "Buat Laporan" button

#### 5. Empty Search Results
- **Icon:** search_off (Material Icon)
- **Heading:** "Tidak ada hasil"
- **Body:** "Coba gunakan kata kunci yang berbeda"
- **CTA:** None

#### 6. Empty Notifications
- **Icon:** notifications_none (Material Icon)
- **Heading:** "Tidak ada notifikasi"
- **Body:** "Notifikasi akan muncul di sini"
- **CTA:** None

#### 7. Offline + Empty List
- **Icon:** cloud_off (Material Icon)
- **Heading:** "Anda sedang offline"
- **Body:** "Belum ada data lokal untuk ditampilkan. Hubungkan ke internet untuk sinkronisasi."
- **CTA:** None

#### 8. Manager Empty Dashboard (Edge Case)
- **Icon:** groups (Material Icon)
- **Heading:** "Belum ada aktivitas tim"
- **Body:** "Laporan dari sales representatives akan muncul di sini"
- **CTA:** None

---

### Error States (5 Designs Required)

**Pattern:**
```
┌─────────────────────────────────┐
│ ⚠️ Error Heading                │ ← Icon + heading, error-500 color
│                                  │
│ Explanation of what went wrong  │ ← Body text, neutral-600
│ and how to fix it                │
│                                  │
│ [Action Button] [Dismiss]       │ ← Error-500 button + text button
└─────────────────────────────────┘
```

#### 1. Network Error
- **Icon:** wifi_off (Material Icon, error-500)
- **Heading:** "Tidak ada koneksi internet"
- **Body:** "Periksa koneksi internet Anda dan coba lagi"
- **Actions:**
  - Primary: "Coba Lagi" button (error-500 background)
  - Secondary: "Tutup" text button

#### 2. Sync Failed
- **Icon:** sync_problem (Material Icon, error-500)
- **Heading:** "Gagal menyinkronkan data"
- **Body:** "5 item gagal disinkronkan. Tap 'Lihat Detail' untuk melihat item mana yang gagal."
- **Actions:**
  - Primary: "Lihat Detail" button
  - Secondary: "Coba Lagi" outlined button
  - Tertiary: "Abaikan" text button

#### 3. Form Validation Error
- **Display:** Inline below invalid field
- **Icon:** error (Material Icon, 16dp, error-500)
- **Text:** Error message (12sp, error-500)
- **Border:** Field border turns error-500 (2dp)
- **Examples:**
  - "Nama perusahaan wajib diisi"
  - "Format email tidak valid"
  - "Nomor telepon harus berisi angka"

#### 4. Photo Upload Failed
- **Icon:** image_not_supported (Material Icon, error-500)
- **Heading:** "Gagal upload 3 dari 10 foto"
- **Body:** "Periksa koneksi internet dan coba lagi"
- **Display:** Show which photos failed (red border on thumbnails)
- **Actions:**
  - "Coba Lagi" button per failed photo
  - OR global "Coba Lagi Semua" button

#### 5. Generic Error
- **Icon:** error_outline (Material Icon, error-500)
- **Heading:** "Terjadi kesalahan"
- **Body:** "Silakan coba lagi. Jika masalah berlanjut, hubungi support."
- **Actions:**
  - "Coba Lagi" button
  - "Tutup" text button

---

### Loading States (4 Designs Required)

#### 1. List Loading (Shimmer Placeholders)
- **Don't:** Show spinner di tengah empty screen
- **DO:** Show skeleton placeholders (shimmer effect)
- **Specifications:**
  - Card shape dengan rounded corners (match actual card design)
  - Shimmer animation: light-to-dark gradient moving left-to-right
  - Animation duration: 1.5s infinite loop
  - Show 5-6 placeholder cards (don't show 20, looks broken)

**Example Shimmer Card:**
```
┌────────────────────────────────┐
│ ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓             │ ← Title (shimmer)
│ ░░░░░░░░░░░░░░░░                │ ← Subtitle (shimmer)
│ ░░░░░░░░░                       │ ← Metadata (shimmer)
└────────────────────────────────┘
```

#### 2. Form Submitting
- **Button State:** Change to loading state
  - Show circular progress indicator (16dp) inside button
  - Change text to "Menyimpan..." atau "Mengirim..."
  - Disable button (reduce opacity to 0.6)
- **Form:** Keep visible but disable all inputs
- **Don't:** Show full-screen spinner

**Example:**
```
Before: [Simpan]
During: [⟳ Menyimpan...]  ← Spinner + text, button disabled
```

#### 3. Photo Uploading (Per-Photo Progress)
- **Display:** Progress circle overlay on each photo thumbnail
- **Specifications:**
  - Circular progress indicator (40dp diameter)
  - Show percentage text inside circle: "45%"
  - Color: primary-500
  - Background overlay: neutral-900 dengan 60% opacity

**Visual:**
```
┌─────┐
│     │
│ 45% │ ← Progress circle
│     │
└─────┘
```

#### 4. Sync in Progress
- **Location:** Top banner (below app bar)
- **Display:** Horizontal progress bar + text
- **Specifications:**
  - Banner height: 48dp
  - Background: primary-50 (light blue)
  - Progress bar: primary-500
  - Text: "Menyinkronkan 5 dari 10 item..." (14sp, primary-900)
  - Icon: sync (Material Icon, animated rotation)
  - Dismissible: X button (top right)

---

### Success States (3 Designs Required)

#### 1. Success Snackbar
- **Use Case:** After successful create/edit/delete operations
- **Specifications:**
  - Position: Bottom of screen, 16dp from bottom & sides
  - Height: 48dp (single line) or auto (multi-line)
  - Background: success-500 (green)
  - Text: White, 14sp
  - Icon: check_circle (Material Icon, 20dp, white)
  - Duration: 3 seconds (auto-dismiss)
  - Action button (optional): "Undo" text button (white text)

**Examples:**
- "Perusahaan berhasil ditambahkan"
- "Laporan berhasil disimpan"
- "Kontak berhasil diperbarui"

#### 2. Sync Success Indicator
- **Display:** Badge or icon next to synced item
- **Specifications:**
  - Icon: check_circle (Material Icon, 16dp, success-500)
  - Text (optional): "Tersinkronisasi" (12sp, success-700)
  - Or just icon (if space limited)

#### 3. Report Submitted Success Screen
- **Use Case:** After submitting report (Step 4 complete)
- **Design:**
  ```
  ┌─────────────────────────────────┐
  │                                  │
  │        ✓ (Large checkmark)       │ ← 120dp, success-500
  │                                  │
  │    Laporan berhasil disimpan     │ ← Heading, 20sp, bold
  │                                  │
  │  Data akan tersinkronisasi saat  │ ← Body, 14sp
  │         online kembali           │
  │                                  │
  │    [Lihat Laporan]  [Tutup]     │ ← Primary + text button
  └─────────────────────────────────┘
  ```

---

## 📐 Part 2: Component Specifications

### Buttons

**Filled Button (Primary Action):**
- Height: 40dp
- Padding horizontal: 24dp
- Corner radius: 20dp (fully rounded)
- Background: primary-500
- Text: White, 14sp, medium weight, all caps
- Min width: 64dp
- Touch target: 48x48dp minimum (add invisible padding if needed)

**Outlined Button (Secondary Action):**
- Same as filled, but:
- Background: transparent
- Border: 1dp, primary-500
- Text: primary-500

**Text Button (Tertiary Action):**
- No background, no border
- Text: primary-500, 14sp, medium weight
- Padding horizontal: 12dp

**FAB (Floating Action Button):**
- Size: 56x56dp
- Corner radius: 16dp (Material Design 3 style)
- Background: primary-500
- Icon: 24dp, white
- Elevation: 6dp (level 3)
- Position: 16dp from bottom-right corner

---

### Text Fields

**Outlined Text Field (Default):**
- Height: 56dp
- Corner radius: 4dp
- Border: 1dp, neutral-300 (rest), primary-500 (focused), error-500 (error)
- Label: 12sp, floating (moves up when focused)
- Input text: 16sp, neutral-900
- Padding: 16dp horizontal, 16dp vertical

**Text Area (Multi-line):**
- Min height: 120dp
- Auto-expand up to max 240dp
- Same styling as outlined text field

---

### Cards

**Elevated Card (Default):**
- Corner radius: 12dp
- Elevation: 1dp (level 1)
- Background: surface (white)
- Padding: 16dp
- Margin: 8dp between cards

**Card Content:**
- Title: 16sp, medium weight, neutral-900
- Subtitle: 14sp, regular, neutral-600
- Metadata: 12sp, regular, neutral-500
- Spacing: 8dp between elements

---

### List Items

**3-Line List Item:**
- Height: 88dp
- Padding: 16dp horizontal
- Title: 16sp, medium weight
- Subtitle: 14sp, regular, max 2 lines (ellipsis)
- Metadata: 12sp, regular

**With Avatar/Icon:**
- Leading element: 40x40dp circle (avatar) atau 24dp icon
- Text starts 72dp from left edge

---

## ♿ Part 3: Accessibility Requirements

### Color Contrast (WCAG AA Minimum)

**Text Contrast Ratios:**
- Normal text (14-16sp): Min 4.5:1 contrast ratio
- Large text (18sp+): Min 3:1 contrast ratio
- UI components (buttons, borders): Min 3:1

**Test Your Colors:**
- Use Stark plugin untuk Figma
- OR use WebAIM contrast checker: https://webaim.org/resources/contrastchecker/

**Common Issues:**
- ❌ Light gray text (neutral-400) on white background = fails WCAG
- ✅ Dark gray text (neutral-600+) on white = passes

---

### Touch Targets

**Minimum Size:**
- All tappable elements: 48x48dp minimum
- If visual element is smaller (e.g., 24dp icon), add invisible padding to reach 48x48dp

**Spacing:**
- Min 8dp spacing between touch targets
- Ideally 16dp for comfortable tapping

---

### Text Sizing

**Minimum Text Sizes:**
- Body text: 14sp minimum (16sp preferred)
- Captions/metadata: 12sp minimum
- Buttons: 14sp

**Line Height:**
- Body text: 1.5x (e.g., 16sp text = 24sp line height)
- Headings: 1.2x

**Max Line Length:**
- Body text: Max 60-70 characters per line (for readability)
- Use padding to limit line length if needed

---

### Alternative Text (For Developer)

**Document in Figma:**
- For each icon: Add description layer
  - Example: Icon "search" → description "Cari perusahaan"
- For each image: Add alt text
  - Example: Company logo → "Logo PT Surya Indah"

**Why:** Developer akan implement sebagai contentDescription di Android

---

## 📤 Part 4: Export Specifications

### Figma Organization

**Page Structure:**
1. **Cover** - Project overview, version, last updated
2. **Design System** - Colors, typography, components
3. **Screens - Priority** - First 5 screens
4. **Screens - Main** - Remaining 26 screens
5. **States** - Empty, error, loading, success
6. **Prototypes** - Interactive flows
7. **Icons** - List of Material Icons used

---

### Frame Naming Convention

**Pattern:** `[Category] - [Screen Name] - [State]`

**Examples:**
- `Auth - Login - Default`
- `Auth - Login - Error`
- `Companies - List - Empty State`
- `Companies - List - Loaded`
- `Reports - Create - Step 1`
- `Reports - Create - Step 2`

---

### Component Naming

**Pattern:** `[Type]/[Variant]/[State]`

**Examples:**
- `Button/Filled/Default`
- `Button/Filled/Hover`
- `Button/Filled/Disabled`
- `Card/Elevated/Default`
- `TextField/Outlined/Empty`
- `TextField/Outlined/Filled`
- `TextField/Outlined/Error`

---

### Export Settings (For Icons)

**Material Icons:**
- Use Material Symbols from Google Fonts
- Export as SVG (24dp)
- Color: Neutral-900 (black) - developer will tint programmatically

**Don't export:**
- Screenshots from other apps
- Stock photos
- Copyrighted images

---

## ✅ Checklist: Before Final Handoff

### Design Quality

- [ ] All 31 screens designed di 360x800dp
- [ ] All screens use brand colors dari CSS Logo Guidelines
- [ ] All screens use Roboto font (default Android)
- [ ] All text is Bahasa Indonesia (placeholder content)
- [ ] All touch targets minimum 48x48dp
- [ ] All text contrast passes WCAG AA (use Stark to check)
- [ ] Consistent spacing (8dp grid system)
- [ ] Consistent component usage (all buttons use same component)

### States Coverage

- [ ] 8 empty states designed
- [ ] 5 error states designed
- [ ] 4 loading states designed
- [ ] 3 success states designed

### Design System

- [ ] Color tokens documented with hex codes
- [ ] Typography scale documented (display, headline, title, body, label)
- [ ] All components created as Figma components
- [ ] Spacing system documented (4, 8, 16, 24, 32dp)

### Prototypes

- [ ] Flow 1: Create Report (clickable, with transitions)
- [ ] Flow 2: Create Company (clickable)
- [ ] Flow 3: Edit Contact (clickable)
- [ ] Flow 4: Sync Failed Handling (clickable)
- [ ] Flow 5: Manager View Reports (clickable)

### Documentation

- [ ] All frames properly named
- [ ] All components properly named
- [ ] Icons list page created
- [ ] Product Owner can view & comment on Figma

---

## 📍 Navigasi

**Selesai membaca Design Requirements?**

➡️ **[Lanjut ke: REFERENCE_APPS.md →](./REFERENCE_APPS.md)**

Dokumen terakhir berisi reference apps untuk study offline UX patterns.

**Atau kembali ke:**
← [USER_STORIES_FOR_DESIGNER](./USER_STORIES_FOR_DESIGNER.md)
