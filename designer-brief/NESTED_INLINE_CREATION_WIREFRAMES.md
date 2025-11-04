# Nested Inline Creation - UX Wireframes & Specifications
## CSS Sales Report - Create Report Flow Enhancement

← [Sebelumnya: User Stories for Designer](./USER_STORIES_FOR_DESIGNER.md)

---

**Versi:** 1.0
**Terakhir Diperbarui:** November 2025
**Target User:** Budi (47, basic tech) & Dina (32, tech-savvy)

---

## 📋 Problem Statement

### Workflow Gap Yang Ditemukan

**Current Documented Flow:**
```
Step 1: User membuat Company (separate screen)
Step 2: User membuat Contact (separate screen)
Step 3: User membuat Project (separate screen)
Step 4: User membuat Report (select project dari dropdown)
```

**Real-World Sales Rep Workflow:**
```
Step 1: Sales rep melakukan visit
Step 2: Setelah meeting, langsung ingin buat report
Step 3: Berpikir: "Project apa ini?" (project-first mental model)
Step 4: BLOCKED jika project/company belum ada → Harus abandon report
```

**Masalah:**
- ❌ Sales rep harus meninggalkan report form untuk create dependencies
- ❌ Tidak sesuai mental model ("Project duluan, company adalah property dari project")
- ❌ Melanggar goal "< 5 menit per report" (jika company baru, +3-5 menit)
- ❌ Bad first-time experience (new customer visit = BLOCKED)
- ❌ Budi akan frustasi dan kembali ke WhatsApp

---

## ✅ Solution: Nested Inline Creation (Option 2)

### Konsep Solusi

**Prinsip:** User TIDAK PERNAH meninggalkan report screen, no matter what entities perlu dibuat.

**Key Features:**
- ✅ Create project inline (di dalam report form)
- ✅ Create company inline (nested di dalam project form) - **jika diperlukan**
- ✅ Create contact inline (nested di dalam project form) - **jika diperlukan**
- ✅ Select existing entities dari dropdown (path tercepat - 60% kasus)
- ✅ Flexible: Mix create new + select existing dalam satu flow

**Pattern:** "Bottom of Dropdown" - Opsi "[+ Buat [Entity] Baru]" selalu di bottom dropdown

**Kenapa Option 2 (bukan Option 1: Basic Inline):**
- Option 1 hanya allow create project inline, company/contact harus sudah ada → Masih block user
- Option 2 allow create SEMUA entities inline → Zero friction, handles semua scenarios

---

## 🎯 User Scenarios (Semua Didukung!)

### Scenario A: Select Existing Project (60% of visits)

**Contoh:** Budi visit PT Indofood untuk project "Factory Expansion" yang sudah ada

**Flow:** 2 menit, zero inline creation
```
1. Open "Buat Laporan"
2. Tap "Project" dropdown
3. Select "PT Indofood - Factory Expansion Q4 2025"
4. Company & Contact auto-filled (read-only)
5. Fill report details → Submit
```

**Wireframe:** Lihat Section "Wireframes - Scenario A"

---

### Scenario B: Create New Project, Existing Company (30% of visits)

**Contoh:** Budi visit PT Indofood (existing) untuk project BARU "Warehouse Renovation 2026"

**Flow:** 3-4 menit
```
1. Open "Buat Laporan"
2. Tap "Project" dropdown → Tap "[+ Buat Project Baru]"
3. Inline project form expands
4. Company dropdown → Select "PT Indofood" (existing)
5. Primary Contact dropdown → Select "Budi Hartono" (existing)
6. Fill project name, type, value → Save project
7. Fill report details → Submit
```

**Wireframe:** Lihat Section "Wireframes - Scenario B"

---

### Scenario C: New Project, Existing Company, New Contact (8% of visits)

**Contoh:** Budi visit PT Indofood, tetapi ada procurement manager baru

**Flow:** 4-5 menit
```
1. Open "Buat Laporan"
2. Tap "Project" dropdown → Tap "[+ Buat Project Baru]"
3. Inline project form expands
4. Company dropdown → Select "PT Indofood" (existing)
5. Primary Contact dropdown → Tap "[+ Buat Contact Baru]"
6. Nested contact form expands → Fill name, position, phone → Save contact
7. Fill project name, type, value → Save project
8. Fill report details → Submit
```

**Wireframe:** Lihat Section "Wireframes - Scenario C"

---

### Scenario D: Brand New Customer (2% of visits)

**Contoh:** Budi visit PT Astra untuk PERTAMA KALI (tidak ada di database)

**Flow:** 5-6 menit
```
1. Open "Buat Laporan"
2. Tap "Project" dropdown → Tap "[+ Buat Project Baru]"
3. Inline project form expands
4. Company dropdown → Tap "[+ Buat Company Baru]"
5. Nested company form expands → Fill company name → Save company
6. Primary Contact dropdown → Tap "[+ Buat Contact Baru]"
7. Nested contact form expands → Fill name, position, phone → Save contact
8. Fill project name, type, value → Save project
9. Fill report details → Submit
```

**Wireframe:** Lihat Section "Wireframes - Scenario D"

---

## 📱 Wireframes - UI States

### State 1: Report Form - Default (No Project Selected)

```
┌─────────────────────────────────────────────────────┐
│ ← Buat Laporan Kunjungan                           │ ← App bar
├─────────────────────────────────────────────────────┤
│                                                     │
│ Project *                                           │ ← Label (12sp, gray)
│ ┌─────────────────────────────────────────────────┐ │
│ │ Pilih project                           [▼]   │ │ ← Dropdown (closed)
│ └─────────────────────────────────────────────────┘ │   Gray placeholder
│                                                     │   48dp height
│ Report Type *                                       │
│ ┌─────────────────────────────────────────────────┐ │
│ │ Pilih tipe kunjungan                    [▼]   │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ Tanggal Kunjungan *                                 │
│ ┌─────────────────────────────────────────────────┐ │
│ │ Nov 4, 2025                             [📅]  │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ... (other fields below - scrollable)              │
│                                                     │
└─────────────────────────────────────────────────────┘
```

---

### State 2: Project Dropdown - Open (Existing Projects Shown)

```
┌─────────────────────────────────────────────────────┐
│ ← Buat Laporan Kunjungan                           │
├─────────────────────────────────────────────────────┤
│                                                     │
│ Project *                                           │
│ ┌─────────────────────────────────────────────────┐ │
│ │ Pilih project                           [▲]   │ │ ← Shows up arrow
│ └─────────────────────────────────────────────────┘ │
│ ┌─────────────────────────────────────────────────┐ │ ← Dropdown overlay
│ │ 🔍 Cari project...                         [x]│ │   (appears)
│ ├─────────────────────────────────────────────────┤ │   Elevation 8dp
│ │  PT Indofood - Factory Expansion Q4 2025      │ │   White background
│ │  PT Unilever - Warehouse Coating 2025         │ │
│ │  PT Astra - Factory Automation                 │ │   Scrollable list
│ │  PT Sinar Mas - Office Building Project       │ │   (max 5 visible)
│ │  PT Telkom - Data Center Infrastructure       │ │
│ ├─────────────────────────────────────────────────┤ │ ← Divider (2dp)
│ │  ➕ Buat Project Baru                          │ │   Gray (#E0E0E0)
│ └─────────────────────────────────────────────────┘ │
│                                                     │   Green text
│                                                     │   (#4CAF50)
│ Report Type *                                       │   Bold (600)
│ ┌─────────────────────────────────────────────────┐ │
│ │ Pilih tipe kunjungan                    [▼]   │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ... (dimmed - dropdown active)                     │
│                                                     │
└─────────────────────────────────────────────────────┘
```

**Interaksi:**
- User tap dropdown → Expands dengan animation (slide down, 200ms)
- User scroll → Existing projects scrollable
- "[+ Buat Project Baru]" **selalu sticky di bottom** (tidak scroll)

---

### State 3: Inline Project Creation - Expanded

```
┌─────────────────────────────────────────────────────┐
│ ← Buat Laporan Kunjungan                           │
├─────────────────────────────────────────────────────┤
│                                                     │
│ Project *                                           │
│ ┌─────────────────────────────────────────────────┐ │
│ │ (Membuat project baru...)               [▼]   │ │ ← Status text
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌───────────────────────────────────────────────┐   │ ← Inline form
│ │  📋 DETAIL PROJECT BARU                      │   │   Light gray bg
│ ├───────────────────────────────────────────────┤   │   (#F5F5F5)
│ │                                               │   │   5dp left indent
│ │  Company *                                    │   │   8dp padding
│ │  ┌─────────────────────────────────────────┐  │   │   Rounded 8dp
│ │  │ Pilih company                     [▼] │  │   │
│ │  └─────────────────────────────────────────┘  │   │
│ │                                               │   │
│ │  Nama Project *                               │   │
│ │  ┌─────────────────────────────────────────┐  │   │
│ │  │ [Ketik nama project...]               │  │   │ ← Auto-focus
│ │  └─────────────────────────────────────────┘  │   │
│ │                                               │   │
│ │  Tipe Project *                               │   │
│ │  ┌─────────────────────────────────────────┐  │   │
│ │  │ Pilih tipe                        [▼] │  │   │
│ │  └─────────────────────────────────────────┘  │   │
│ │                                               │   │
│ │  Estimasi Nilai *                             │   │
│ │  ┌─────────────────────────────────────────┐  │   │
│ │  │ Rp [0]                                │  │   │
│ │  └─────────────────────────────────────────┘  │   │
│ │                                               │   │
│ │  Primary Contact *                            │   │
│ │  ┌─────────────────────────────────────────┐  │   │
│ │  │ Pilih contact                     [▼] │  │   │
│ │  └─────────────────────────────────────────┘  │   │
│ │                                               │   │
│ │  ┌──────────────┬──────────────────────────┐  │   │
│ │  │ Batalkan    │ Simpan Project & Lanjut │  │   │ ← Buttons
│ │  └──────────────┴──────────────────────────┘  │   │   48dp height
│ └───────────────────────────────────────────────┘   │
│                                                     │
│ Report Type *                                       │ ← Other fields
│ ┌─────────────────────────────────────────────────┐ │   pushed down
│ │ Pilih tipe kunjungan                    [▼]   │ │   (still visible)
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ... (scrollable)                                   │
│                                                     │
└─────────────────────────────────────────────────────┘
```

**Animation:**
- Expand: Slide down + fade in (300ms, ease-out)
- Other fields push down smoothly (maintain scroll position)

---

### State 4: Nested Company Creation (Level 3)

```
┌─────────────────────────────────────────────────────┐
│ ← Buat Laporan > Buat Project > Buat Company      │ ← Breadcrumb
├─────────────────────────────────────────────────────┤
│                                                     │
│ Project *                                           │
│ ┌─────────────────────────────────────────────────┐ │
│ │ (Membuat project baru...)               [▼]   │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌───────────────────────────────────────────────┐   │ ← Level 2
│ │  📋 DETAIL PROJECT BARU                      │   │   (Project)
│ ├───────────────────────────────────────────────┤   │
│ │                                               │   │
│ │  Company *                                    │   │
│ │  ┌─────────────────────────────────────────┐  │   │
│ │  │ (Membuat company baru...)         [▼] │  │   │
│ │  └─────────────────────────────────────────┘  │   │
│ │                                               │   │
│ │  ┌───────────────────────────────────────┐    │   │ ← Level 3
│ │  │  🏢 COMPANY BARU                     │    │   │   (Company)
│ │  ├───────────────────────────────────────┤    │   │   Light blue bg
│ │  │                                       │    │   │   (#E3F2FD)
│ │  │  Nama Company *                       │    │   │   10dp indent
│ │  │  ┌─────────────────────────────────┐  │    │   │
│ │  │  │ [Ketik nama company...]        │  │    │   │ ← Auto-focus
│ │  │  └─────────────────────────────────┘  │    │   │
│ │  │  ⚠️ Company name tidak boleh kosong │    │   │ ← Validation
│ │  │                                       │    │   │   (real-time)
│ │  │  ┌──────────────┬──────────────────┐  │    │   │
│ │  │  │ Batalkan    │ ✓ Simpan        │  │    │   │
│ │  │  └──────────────┴──────────────────┘  │    │   │
│ │  └───────────────────────────────────────┘    │   │
│ │                                               │   │
│ │  Nama Project *                               │   │
│ │  ┌─────────────────────────────────────────┐  │   │
│ │  │ [Ketik nama project...]               │  │   │
│ │  └─────────────────────────────────────────┘  │   │
│ │  ... (other fields)                           │   │
│ └───────────────────────────────────────────────┘   │
│                                                     │
│ ... (scrollable)                                   │
│                                                     │
└─────────────────────────────────────────────────────┘
```

**Visual Depth:**
- Level 1 (Report): White (#FFFFFF)
- Level 2 (Project): Light gray (#F5F5F5), 5dp indent
- Level 3 (Company): Light blue (#E3F2FD), 10dp total indent

---

### State 5: After Nested Save - Success

```
┌─────────────────────────────────────────────────────┐
│ ← Buat Laporan Kunjungan                           │
├─────────────────────────────────────────────────────┤
│                                                     │
│ Project *                                           │
│ ┌─────────────────────────────────────────────────┐ │
│ │ (Membuat project baru...)               [▼]   │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌───────────────────────────────────────────────┐   │
│ │  📋 DETAIL PROJECT BARU                      │   │
│ ├───────────────────────────────────────────────┤   │
│ │                                               │   │
│ │  Company * ✓                                  │   │ ← Green checkmark
│ │  ┌─────────────────────────────────────────┐  │   │
│ │  │ PT Astra                          [▼] │  │   │ ← Newly created
│ │  └─────────────────────────────────────────┘  │   │   company selected
│ │                [Green border - 2s duration]    │   │   (with green
│ │                                               │   │    border glow)
│ │  [Nested form COLLAPSED - animated out]       │   │
│ │                                               │   │
│ │  Nama Project *                               │   │ ← Back to normal
│ │  ┌─────────────────────────────────────────┐  │   │   position
│ │  │ [Ketik nama project...]               │  │   │
│ │  └─────────────────────────────────────────┘  │   │
│ │  ...                                          │   │
│ └───────────────────────────────────────────────┘   │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🎉 Company berhasil dibuat!                    [x]│ ← Toast (bottom)
└─────────────────────────────────────────────────────┘   Green bg
                                                          Fades out 2s
```

**Animation:**
- Nested form: Fade out + slide up (200ms, ease-in)
- Green border: Appear + fade to normal (2000ms total)
- Toast: Slide up from bottom (200ms) → Stay 2s → Fade out (200ms)

---

## 🎨 Dropdown Pattern Specification

### "Bottom of Dropdown" - Universal Pattern

Digunakan untuk **SEMUA** dropdowns: Project, Company, Contact

```
┌─────────────────────────────────────────┐
│ 🔍 [Cari...]                       [x]│ ← Search (optional, jika >10 items)
├─────────────────────────────────────────┤   200ms debounce
│  PT Indofood                    ✓      │ ← Existing items (scrollable)
│  PT Unilever                           │   48dp height each
│  PT Astra International                │   Left: 16dp padding
│  PT Sinar Mas                          │   Right: 16dp padding
│  PT Telkom                             │
│  ... (15 more - scrollable)            │   Max visible: 5 items
├─────────────────────────────────────────┤   = 240dp max height
│  ➕ Buat [Entity] Baru                 │ ← ALWAYS at bottom
└─────────────────────────────────────────┘   (sticky, no scroll)
    ↑
    Different color: Green (#4CAF50) or Blue (#2196F3)
    Icon: ➕ (20dp, add_circle_outline)
    Text: Bold (600 weight), 14sp
    Touch target: 48dp height
    Background on tap: Green/Blue 10% opacity
```

**Behavior:**
- Tap existing item → Select & close dropdown → Populate field
- Tap "[+ Buat [Entity] Baru]" → Inline form expands → Dropdown closes
- Search: Filter existing items real-time, "[+ Buat...]" always visible
- Outside tap → Close dropdown without selection

---

## 🎯 User Flow Diagrams

### Flow 1: Scenario B (Most Common: 30%)

Create new project, existing company/contact

```
┌─────────┐
│  START  │ Sales rep selesai visit
└────┬────┘
     ▼
┌─────────────────────────────────┐
│ Open "Buat Laporan"             │
└────┬────────────────────────────┘
     ▼
┌─────────────────────────────────┐
│ Tap "Project" dropdown          │
└────┬────────────────────────────┘
     ▼
┌─────────────────────────────────┐
│ See existing projects + create  │
│ Tap "[+ Buat Project Baru]"     │
└────┬────────────────────────────┘
     ▼
┌─────────────────────────────────┐
│ Inline project form expands     │ (300ms animation)
└────┬────────────────────────────┘
     ▼
┌─────────────────────────────────┐
│ Tap "Company" dropdown          │
│ SELECT existing "PT Indofood"   │ ← Fast (no inline creation)
└────┬────────────────────────────┘
     ▼
┌─────────────────────────────────┐
│ Tap "Primary Contact" dropdown  │
│ SELECT existing "Budi Hartono"  │ ← Fast (no inline creation)
└────┬────────────────────────────┘
     ▼
┌─────────────────────────────────┐
│ Fill project name, type, value  │
│ Tap "Simpan Project & Lanjut"   │
└────┬────────────────────────────┘
     ▼
┌─────────────────────────────────┐
│ Project saved to SQLite         │
│ Form collapses (200ms)          │
│ Success toast appears           │
└────┬────────────────────────────┘
     ▼
┌─────────────────────────────────┐
│ Fill report fields              │
│ (Type, Date, Notes, Photos)     │
└────┬────────────────────────────┘
     ▼
┌─────────────────────────────────┐
│ Tap "Simpan Laporan"            │
└────┬────────────────────────────┘
     ▼
┌─────────┐
│  DONE   │ Total time: 3-4 min
└─────────┘
```

---

### Flow 2: Scenario D (Rare: 2%)

Brand new customer (create everything)

```
┌─────────┐
│  START  │
└────┬────┘
     ▼
┌─────────────────────────────────┐
│ Open "Buat Laporan"             │
│ Tap "[+ Buat Project Baru]"     │
└────┬────────────────────────────┘
     ▼
┌─────────────────────────────────┐
│ Inline project form expands     │
└────┬────────────────────────────┘
     ▼
┌─────────────────────────────────┐
│ Tap "Company" dropdown          │
│ Tap "[+ Buat Company Baru]"     │ ← Nested creation starts
└────┬────────────────────────────┘
     ▼
┌─────────────────────────────────┐
│ Nested company form expands     │ (Level 3)
│ Fill company name: "PT Astra"   │
│ Tap "Simpan"                     │
└────┬────────────────────────────┘
     ▼
┌─────────────────────────────────┐
│ Company saved → Form collapses  │
│ "PT Astra" auto-selected        │
│ Toast: "Company berhasil"       │
└────┬────────────────────────────┘
     ▼
┌─────────────────────────────────┐
│ Tap "Primary Contact" dropdown  │
│ Tap "[+ Buat Contact Baru]"     │ ← Nested creation (contact)
└────┬────────────────────────────┘
     ▼
┌─────────────────────────────────┐
│ Nested contact form expands     │ (Level 3)
│ Fill name, position, phone      │
│ Tap "Simpan"                     │
└────┬────────────────────────────┘
     ▼
┌─────────────────────────────────┐
│ Contact saved → Form collapses  │
│ "Andi Wijaya" auto-selected     │
│ Toast: "Contact berhasil"       │
└────┬────────────────────────────┘
     ▼
┌─────────────────────────────────┐
│ Fill project name, type, value  │
│ Tap "Simpan Project & Lanjut"   │
└────┬────────────────────────────┘
     ▼
┌─────────────────────────────────┐
│ Project saved → Form collapses  │
│ Continue with report form       │
└────┬────────────────────────────┘
     ▼
┌─────────────────────────────────┐
│ Fill report fields → Submit     │
└────┬────────────────────────────┘
     ▼
┌─────────┐
│  DONE   │ Total time: 5-6 min
└─────────┘
```

---

## 🎨 Visual Design Specifications

### Colors

| Element | Background | Text | Border |
|---------|------------|------|--------|
| **Report Form (Level 1)** | White (#FFFFFF) | Black 87% (#212121) | Gray (#E0E0E0) |
| **Inline Project (Level 2)** | Light Gray (#F5F5F5) | Black 87% (#212121) | Gray (#BDBDBD) |
| **Nested Company/Contact (Level 3)** | Light Blue (#E3F2FD) | Black 87% (#212121) | Blue (#90CAF9) |
| **"Create New" Option** | Transparent | Green (#4CAF50) or Blue (#2196F3) | None |
| **Success State** | N/A | N/A | Green (#4CAF50, 2dp) |
| **Error State** | N/A | Red (#F44336) | Red (#F44336, 2dp) |

### Typography

| Element | Font Size | Weight | Color |
|---------|-----------|--------|-------|
| **Field Label** | 12sp | Medium (500) | Black 60% (#757575) |
| **Field Value** | 16sp | Regular (400) | Black 87% (#212121) |
| **Placeholder** | 16sp | Regular (400) | Gray 38% (#9E9E9E) |
| **"Create New" Button** | 14sp | Bold (600) | Green/Blue |
| **Section Header** | 14sp | Bold (600) | Black 87% |
| **Error Text** | 12sp | Regular (400) | Red (#F44336) |
| **Toast Message** | 14sp | Medium (500) | White (#FFFFFF) |

### Spacing

| Element | Padding/Margin | Indent |
|---------|----------------|--------|
| **Field Vertical Spacing** | 16dp between fields | - |
| **Inline Form Padding** | 8dp all sides | 5dp left |
| **Nested Form Padding** | 8dp all sides | 10dp left (total 15dp) |
| **Dropdown Item Padding** | 16dp horizontal, 12dp vertical | - |
| **Touch Target Height** | Minimum 48dp | - |

### Animation Timing

| Animation | Duration | Easing | Trigger |
|-----------|----------|--------|---------|
| **Expand Inline Form** | 300ms | Ease-out | User taps "[+ Buat...]" |
| **Collapse Inline Form** | 200ms | Ease-in | After save or cancel |
| **Toast Appear** | 200ms | Ease-out | After successful save |
| **Toast Disappear** | 200ms | Linear | After 2000ms delay |
| **Success Border Glow** | 2000ms | Linear fade | After entity saved |
| **Dropdown Open** | 200ms | Ease-out | User taps dropdown |
| **Auto-scroll** | 250ms | Ease-in-out | Form expands/collapses |

---

## 📏 Mobile Optimization (Budi's Profile)

### Considerations untuk Budi (47, basic tech)

**Screen Size:** 6.43 inch (Xiaomi Redmi Note 11 - mid-range Android)

**Optimizations:**

1. **Large Touch Targets**
   - Minimum 48dp height untuk ALL interactive elements
   - Spacing 8dp between adjacent touch targets
   - No small tap areas (Budi's fingers, tired after 4-5 visits)

2. **Clear Visual Hierarchy**
   - Use COLORS for depth (not just shadows)
   - Level 1: White, Level 2: Gray, Level 3: Blue
   - Indentation PLUS color change (double cue)
   - Budi doesn't rely on subtle shadows

3. **Breadcrumb Trail (When Deeply Nested)**
   ```
   ┌─────────────────────────────────────────────┐
   │ Buat Laporan > Buat Project > Buat Company │ ← Shows current depth
   └─────────────────────────────────────────────┘
   ```
   - Appears when Level 3 active (company/contact creation)
   - Helps Budi understand where he is

4. **Sticky Bottom Buttons**
   ```
   ┌──────────────┬──────────────────────────┐
   │ Batalkan    │ Simpan & Lanjutkan      │ ← Always visible
   └──────────────┴──────────────────────────┘   (above keyboard)
   ```
   - Stay visible even when keyboard open
   - Elevation 4dp (floating above content)

5. **One-Handed Use**
   - Primary actions at bottom (thumb-reachable)
   - No critical UI elements at top (hard to reach)
   - Back button always at top-left (standard Android)

6. **Forgiving Interactions**
   - Confirmation before discard: "Buang perubahan?"
   - Auto-save draft every 30s (in background)
   - "Batalkan" button large and clear (not hidden)

7. **Clear Feedback**
   - Toast messages with icons: ✓ (success), ⚠️ (warning)
   - Success animations (checkmark, green glow)
   - Validation errors in RED text (not subtle)

---

## ✅ Validation Rules

### Real-Time Validation (300ms debounce)

**Company Name:**
- ❌ Empty → "Nama company tidak boleh kosong"
- ❌ Duplicate (case-insensitive) → "Company 'PT ABC' sudah ada"
- ✅ Valid → Green border, checkmark

**Contact Fields:**
- ❌ Name empty → "Nama contact tidak boleh kosong"
- ❌ Phone invalid format → "Format phone tidak valid (08xxx atau +62xxx)"
- ❌ Email invalid (if filled) → "Format email tidak valid"
- ✅ All valid → Green border, checkmark

**Project Fields:**
- ❌ Name empty → "Nama project tidak boleh kosong"
- ❌ Value ≤ 0 → "Nilai estimasi harus lebih besar dari 0"
- ❌ No company selected → "Pilih company atau buat baru"
- ❌ No contact selected → "Pilih contact atau buat baru"
- ✅ All valid → Enable "Simpan Project & Lanjut" button

---

## 🚨 Edge Cases & Error Handling

### Edge Case 1: User Creates Company, Then Changes Mind

**Scenario:** User fills company name, then taps "Batalkan"

**Solution:**
```
┌─────────────────────────────────────────────┐
│ Buang Perubahan?                            │
├─────────────────────────────────────────────┤
│ Company yang baru diisi akan hilang.       │
│ Lanjutkan?                                  │
├─────────────────────────────────────────────┤
│           [Batalkan]  [Ya, Buang]           │
└─────────────────────────────────────────────┘
```

**Result:**
- If "Ya, Buang" → Collapse form, reset fields, return to dropdown
- If "Batalkan" → Stay in form, keep filled data

---

### Edge Case 2: Duplicate Company Name

**Scenario:** User types "PT Indofood", company already exists

**Solution:**
```
┌───────────────────────────────────────┐
│  🏢 COMPANY BARU                     │
├───────────────────────────────────────┤
│  Nama Company *                       │
│  ┌─────────────────────────────────┐  │
│  │ PT Indofood                    │  │ ← Red border
│  └─────────────────────────────────┘  │
│  ❌ Company 'PT Indofood' sudah ada. │ ← Error text (red)
│  Pilih dari dropdown atau gunakan    │
│  nama berbeda.                        │
│                                       │
│  [Pilih dari Dropdown] [Coba Lagi]   │ ← Action buttons
└───────────────────────────────────────┘
```

**Result:**
- "Pilih dari Dropdown" → Collapse, open dropdown, highlight "PT Indofood"
- "Coba Lagi" → Clear field, let user type different name

---

### Edge Case 3: User Changes Company After Creating Contact

**Scenario:** User creates contact for Company A, then switches to Company B

**Problem:** Contact belongs to Company A, invalid for Company B

**Solution:**
```
┌───────────────────────────────────────┐
│ ⚠️ Contact Tidak Tersedia            │
├───────────────────────────────────────┤
│ Contact 'Andi Wijaya' milik PT Astra.│
│ Anda memilih PT Indofood.            │
│                                       │
│ Pilihan:                              │
│ 1. Pilih contact lain dari PT Indofood│
│ 2. Kembalikan ke PT Astra             │
│                                       │
│ [Pilih Contact Lain] [Kembali]       │
└───────────────────────────────────────┘
```

**Result:**
- "Pilih Contact Lain" → Clear contact field, open contact dropdown (PT Indofood's contacts)
- "Kembali" → Revert company selection to PT Astra

---

## 🎯 Success Metrics (Post-Launch)

**KPIs untuk Measure Success:**

1. **Time to Create Report** (Goal: < 5 min)
   - Scenario A (existing project): < 2 min ✅
   - Scenario B (new project, existing company): < 4 min ✅
   - Scenario D (all new): < 6 min ⚠️ (acceptable, rare 2%)

2. **Inline Creation Usage Rate**
   - Target: 30-40% of reports use inline project creation
   - Track: % of reports where project created inline vs pre-existing

3. **Error Rate**
   - Duplicate company errors: < 5%
   - Abandoned inline forms (cancel without save): < 10%

4. **User Satisfaction** (Post-visit survey)
   - "Report creation was easy" > 80% agree
   - "I never needed to leave report screen" > 90% agree

---

## 📍 Navigasi

**Selesai membaca wireframes?**

➡️ **[Lanjut ke: Design Requirements (Updated) →](./DESIGN_REQUIREMENTS.md)**

**Atau kembali ke:**
← [User Stories for Designer](./USER_STORIES_FOR_DESIGNER.md)
🏠 [Designer Brief README](./README.md)

---

**Document Status:** ✅ Complete - Ready untuk design implementation
**Terakhir Diperbarui:** November 2025

---

**CATATAN UNTUK DESIGNER:**
Dokumen ini adalah hasil dari **brainstorming session** dengan Product Owner yang mengidentifikasi **critical workflow gap** antara documented flow vs. real-world sales rep behavior. Option 2 (Full Nested Inline Creation) dipilih karena **sepenuhnya menyelesaikan workflow friction** untuk semua scenarios (new customer, existing customer, mix). Wireframes di atas menunjukkan semua UI states yang diperlukan. Prioritas: **P0 - Critical untuk MVP**.
