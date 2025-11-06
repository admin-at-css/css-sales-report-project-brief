# Nested Inline Creation - Complete Wireframes
## CSS Sales Report App - Report Creation Flow (CORRECTED)

← [Sebelumnya: SCREEN_INVENTORY.md](./SCREEN_INVENTORY.md)

---

**Versi:** 2.0 (CORRECTED - Report Type First)
**Terakhir Diperbarui:** November 2025
**Target User:** Budi (47, basic tech) & Dina (32, tech-savvy)

---

## ⚠️ MAJOR UPDATE: Flow Corrected

**Versi Sebelumnya (v1.0):** Flow dimulai dengan Project dropdown
**Versi Sekarang (v2.0):** Flow dimulai dengan **Report Type** dropdown (menentukan flow selanjutnya)

---

## 📋 Corrected Flow Overview

### Key Principle: Report Type Determines Complexity

**Report Type** → **Logika Branching:**

1. **Initial Visit** → Flow lengkap 5 sections:
   - Report Type → Project → Company → Contact → Report Details

2. **Tipe lainnya (Follow-up, Technical, Price Quotation, Closing, After Sales)** → Flow cepat 3 sections:
   - Report Type → Pilih Existing Project → Report Details

---

##  🎯 UI Pattern: Hybrid Single-Screen with Progressive Disclosure

**Fitur:**
- ✅ Semua sections di satu scrollable screen
- ✅ Sections terbuka secara progressive setelah diselesaikan
- ✅ Auto-scroll ke section berikutnya setelah completion
- ✅ Dapat tap collapsed sections untuk edit
- ✅ "Create New" di TOP combo boxes (selalu visible)
- ✅ Real-time validation per section

---

## 📱 Complete Wireframes (17 Total)

---

### Wireframe 1: Initial State (Screen Load)

**State:** User membuka screen "Create Report"
**Active Section:** Report Type (saja)
**Locked Sections:** Project, Company, Contact, Report Details (grayed out)

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃ ← Back          Create Report      ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
┌─────────────────────────────────────┐
│                                     │
│ ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  │
│ 1 of 5 • Report Type                │
│ ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  │
│                                     │
│ ▼ Report Type                       │
│   ┌───────────────────────────────┐ │
│   │ Select report type...      ▾  │ │ ← Active dropdown
│   └───────────────────────────────┘ │
│                                     │
│   What type of visit is this?       │ ← Helper text
│                                     │
│                                     │
│ ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  │
│                                     │
│ ▶ Project                           │ ← Collapsed, grayed
│   Select report type first          │
│                                     │
│ ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  │
│                                     │
│ ▶ Company                           │ ← Collapsed, grayed
│   Complete project first            │
│                                     │
│ ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  │
│                                     │
│ ▶ Contact                           │ ← Collapsed, grayed
│   Complete company first            │
│                                     │
│ ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  │
│                                     │
│ ▶ Report Details                    │ ← Collapsed, grayed
│   Complete all above first          │
│                                     │
│                                     │
│ ┌───────────────────────────────┐   │
│ │      SUBMIT REPORT            │   │ ← Disabled (gray)
│ └───────────────────────────────┘   │
│                                     │
└─────────────────────────────────────┘
```

**Design Notes:**
- White background untuk main screen
- Grayed out sections punya opacity: 0.4
- Helper text warna gray (#666666)
- Progress indicator "1 of 5" membantu user memahami panjang flow

---

### Wireframe 2: Report Type Dropdown Expanded

**State:** User tap Report Type dropdown
**Shows:** Semua 6 opsi report type
**Design:** Initial Visit punya icon berbeda (🆕) untuk menandakan perbedaan

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃ ← Back          Create Report      ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
┌─────────────────────────────────────┐
│ ▼ Report Type                       │
│   ┌───────────────────────────────┐ │
│   │ Select type...             ▾  │ │
│   ├───────────────────────────────┤ │
│   │ 🆕 Initial Visit              │ │ ← First option (new project)
│   ├───────────────────────────────┤ │ ← Divider
│   │ 🔄 Follow-up Meeting          │ │
│   │ 📊 Technical Presentation     │ │
│   │ 💰 Price Quotation            │ │
│   │ ✅ Closing Visit              │ │
│   │ 🛠️ After Sales Visit          │ │
│   └───────────────────────────────┘ │
│                                     │
│ ▶ Project                           │
│   Select report type first          │
│                                     │
└─────────────────────────────────────┘
```

**Design Notes:**
- Dropdown punya subtle shadow (elevation: 2dp)
- Setiap opsi punya icon untuk quick visual scanning
- Divider memisahkan "Initial Visit" dari yang lain (flow berbeda)
- Touch target height: 48dp per opsi

---

###  Wireframe 3: After Selecting "Initial Visit"

**State:** Report Type = "Initial Visit" terpilih
**Behavior:** Project section unlock dan auto-scroll ke view
**Locked:** Company, Contact, Report Details masih grayed

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃ ← Back          Create Report      ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
┌─────────────────────────────────────┐
│ ✓ Report Type: Initial Visit     [✏️]│ ← Checkmark + edit icon
│                                     │
│ ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  │
│ 2 of 5 • Project                    │
│ ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  │
│                                     │ ← Auto-scrolled here
│ ▼ Project                           │
│   ┌───────────────────────────────┐ │
│   │ Type or scroll to search... 🔍│ │ ← Active combo box
│   └───────────────────────────────┘ │
│                                     │
│   Which project are you visiting?   │
│                                     │
│ ▶ Company                           │ ← Still locked
│   Select or create project first   │
│                                     │
│ ▶ Contact                           │
│   Complete company first            │
│                                     │
│ ▶ Report Details                    │
│   Complete all above first          │
│                                     │
│ ┌───────────────────────────────┐   │
│ │      SUBMIT REPORT            │   │ ← Still disabled
│ └───────────────────────────────┘   │
└─────────────────────────────────────┘
```

**Design Notes:**
- Completed section menampilkan: ✓ + collapsed state + edit icon
- Green checkmark (#4CAF50) menandakan completion
- Edit icon (✏️) memungkinkan user kembali untuk ubah
- Smooth scroll animation: 300ms easing

---

### Wireframe 4: Project Combo Box Expanded (No Typing)

**State:** User tap Project combo box
**Shows:** "Create New" di top (fixed) + semua existing projects (scrollable)
**Key Feature:** Create New SELALU visible bahkan sebelum typing

```
┌─────────────────────────────────────┐
│ ▼ Project                           │
│   ┌───────────────────────────────┐ │
│   │ [________________________] 🔍 │ │ ← Empty search box
│   ├───────────────────────────────┤ │
│   │ ➕ Create New Project         │ │ ← FIXED at top (green text)
│   ├───────────────────────────────┤ │ ← Solid divider line
│   │ 📄 Factory Expansion          │ │
│   │    PT ABC Manufacturing       │ │ ← Company name shown (gray)
│   ├───────────────────────────────┤ │ ← Thin divider
│   │ 📄 Office Renovation          │ │
│   │    PT XYZ Industries          │ │
│   ├───────────────────────────────┤ │
│   │ 📄 Warehouse Construction     │ │
│   │    PT ABC Manufacturing       │ │
│   ├───────────────────────────────┤ │
│   │ 📄 Factory Painting Project   │ │
│   │    PT 123 Corporation         │ │
│   │   ...scrollable (100+ items)  │ │ ← Scrollable list
│   └───────────────────────────────┘ │
└─────────────────────────────────────┘
```

**Design Notes:**
- "Create New" punya green text (#4CAF50) untuk visibility
- ➕ icon menandakan creation action
- Solid divider memisahkan "Create New" dari existing items
- Existing items menampilkan: Project name (bold) + Company name (secondary text)
- Max dropdown height: 50vh (mencegah menutupi seluruh screen)
- Dropdown punya vertical scroll jika >10 items

---

### Wireframe 5: Project Combo Box - Filtered by Typing

**State:** User mengetik "Factory" di search box
**Shows:** Filtered results + "Create New" tetap pinned di top
**Result Count:** Menampilkan berapa banyak matches ditemukan

```
┌─────────────────────────────────────┐
│ ▼ Project                           │
│   ┌───────────────────────────────┐ │
│   │ Factory______________ [X] 🔍  │ │ ← Typed "Factory" + clear X
│   ├───────────────────────────────┤ │
│   │ ➕ Create New Project         │ │ ← Still visible (pinned)
│   ├───────────────────────────────┤ │
│   │ 📄 Factory Expansion          │ │ ← Filtered results
│   │    PT ABC Manufacturing       │ │    (matches "Factory")
│   ├───────────────────────────────┤ │
│   │ 📄 Factory Painting Project   │ │
│   │    PT 123 Corporation         │ │
│   └───────────────────────────────┘ │
│                                     │
│   2 projects found                  │ ← Result count
└─────────────────────────────────────┘
```

**Design Notes:**
- Search adalah case-insensitive partial match
- Tombol [X] membersihkan search instantly
- "Create New" tetap di top bahkan saat filtering
- Result count membantu user tahu apakah perlu create new
- Jika 0 results, tampilkan: "Tidak ada hasil. Buat baru?"

---

### Wireframe 6: Create New Project - Inline Form Expanded

**State:** User tap "➕ Create New Project"
**Shows:** Inline form dengan semua project fields
**Visual:** Light blue background (#E3F2FD), 10dp left indent
**Pre-filled:** Project Name dari apa yang user ketik (jika ada)

```
┌─────────────────────────────────────┐
│ ▼ Project                           │
│   ┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓ │
│   ┃ ✏️ Creating New Project      ┃ │ ← Light blue bg (#E3F2FD)
│   ┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛ │    10dp indent
│   │                             │   │
│   │ Project Name *              │   │
│   │ ┌─────────────────────────┐ │   │
│   │ │ Factory Expansion_____  │ │   │ ← Pre-filled if typed
│   │ └─────────────────────────┘ │   │
│   │                             │   │
│   │ Project Type *              │   │
│   │ ┌─────────────────────────┐ │   │
│   │ │ Architectural        ▾  │ │   │
│   │ └─────────────────────────┘ │   │
│   │                             │   │
│   │ Segmentation * (Select 1+)  │   │
│   │ ┌─────────────────────────┐ │   │
│   │ │ ☑ Decorative            │ │   │ ← Multi-select chips
│   │ │ ☐ Protective Coating    │ │   │
│   │ │ ☑ Floor Coating         │ │   │
│   │ │ ☐ Marine Coating        │ │   │
│   │ └─────────────────────────┘ │   │
│   │                             │   │
│   │ Project Source *            │   │
│   │ ┌─────────────────────────┐ │   │
│   │ │ Canvassing           ▾  │ │   │
│   │ └─────────────────────────┘ │   │
│   │                             │   │
│   │ Estimated Value *           │   │
│   │ ┌─────────────────────────┐ │   │
│   │ │ Rp 50,000,000_________  │ │   │ ← Currency format
│   │ └─────────────────────────┘ │   │
│   │                             │   │
│   │ Expected Close Date         │   │
│   │ ┌─────────────────────────┐ │   │
│   │ │ DD/MM/YYYY          📅  │ │   │ ← Optional
│   │ └─────────────────────────┘ │   │
│   │                             │   │
│   │ ┌──────────┐ ┌────────────┐│   │
│   │ │ Cancel   │ │ Continue ➜ ││   │ ← Action buttons
│   │ └──────────┘ └────────────┘│   │
│   ┗━━━━━━━━━━━━━━━━━━━━━━━━━━━┛   │
│                                     │
│ ▶ Company                           │ ← Next section (locked)
│   Complete project first            │
└─────────────────────────────────────┘
```

**Design Notes:**
- Light blue background membedakan inline form dari main screen
- 10dp left indent menciptakan visual depth
- Required fields ditandai dengan red asterisk (*)
- Multi-select chips untuk Segmentation (bisa pilih multiple)
- Currency input auto-format dengan thousand separators
- Cancel button adalah secondary (outlined), Continue adalah primary (filled)
- "Continue" disabled sampai semua required fields valid

---

**Catatan:** Wireframes 7-17 (Company creation, Contact creation, Report Details, dan Follow-up flow) mengikuti pattern yang sama dengan progressive disclosure dan inline creation.

---

## 📱 Entry Point C: Create Report from Project Detail (Wireframes 18-22)

### Wireframe 18: Project Detail Screen with "Buat Laporan" Button

**State:** User viewing Project Detail screen
**Shows:** Project information + "Buat Laporan untuk Project Ini" button
**User:** Sales rep only (Manager doesn't see this button)

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃ ← Back    Factory Expansion    ✏️  ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
┌─────────────────────────────────────┐
│ Project Information                 │
│                                     │
│ Project Name                        │
│ Factory Expansion Q4 2025           │
│                                     │
│ Type: Architectural                 │
│ Segmentation: [Decorative][Floor]   │
│ Status: [Active] ← Green badge      │
│                                     │
│ Estimated Value                     │
│ Rp 50,000,000                       │
│                                     │
│ Expected Close Date                 │
│ 31/12/2025                          │
│                                     │
│ Primary Contact                     │
│ Budi Santoso - Project Manager →   │
│                                     │
│ Company                             │
│ PT ABC Manufacturing →              │
│                                     │
│ ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  │
│                                     │
│ ▼ Value Change Log                  │
│                                     │
│ ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  │
│                                     │
│ ┌───────────────────────────────┐   │
│ │  BUAT LAPORAN UNTUK           │   │ ← Fixed button
│ │  PROJECT INI                  │   │    Green bg (#4CAF50)
│ └───────────────────────────────┘   │    48dp height
│                                     │
└─────────────────────────────────────┘
```

**Design Notes:**
- Button fixed di bottom of screen (always visible saat scroll)
- Primary button style (green background, white text)
- Only visible untuk sales rep role
- Manager view: Tidak ada button ini (read-only access)

---

### Wireframe 19: Bottom Sheet - Report Type Picker

**State:** User tapped "Buat Laporan" button
**Shows:** Bottom sheet dengan 5 report type options (NO "Initial Visit")
**Default Selection:** "Follow-up Meeting" (highlighted)

```
┌─────────────────────────────────────┐
│                                     │
│ (Project Detail screen dimmed)      │
│                                     │
└─────────────────────────────────────┘
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃                                     ┃ ← Bottom sheet
┃ Jenis Laporan?                      ┃    slides up
┃                                     ┃    from bottom
┃ ┌─────────────────────────────────┐ ┃
┃ │ ● 🔄 Follow-up Meeting          │ ┃ ← Default
┃ │                                 │ ┃    (radio selected)
┃ ├─────────────────────────────────┤ ┃    Green highlight
┃ │ ○ 📊 Technical Presentation     │ ┃
┃ ├─────────────────────────────────┤ ┃
┃ │ ○ 💰 Price Quotation            │ ┃
┃ ├─────────────────────────────────┤ ┃
┃ │ ○ ✅ Closing Visit              │ ┃
┃ ├─────────────────────────────────┤ ┃
┃ │ ○ 🛠️ After Sales Visit          │ ┃
┃ └─────────────────────────────────┘ ┃
┃                                     ┃
┃ Catatan: "Initial Visit" tidak      ┃ ← Helper text
┃ tersedia (project sudah ada)        ┃
┃                                     ┃
┃ ┌──────────┐         ┌────────────┐ ┃
┃ │  Batal   │         │  Lanjut ➜ │ ┃
┃ └──────────┘         └────────────┘ ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Design Notes:**
- Material Design 3 bottom sheet component
- Background dimmed (scrim) dengan opacity 0.4
- Radio buttons untuk single selection
- "Follow-up Meeting" pre-selected (most common use case)
- "Initial Visit" NOT available (project exists = not initial visit)
- Helper text menjelaskan kenapa "Initial Visit" tidak ada
- "Batal" button dismisses bottom sheet
- "Lanjut" button opens Create Report screen dengan pre-fill

---

### Wireframe 20: Create Report - Pre-filled Sections (Collapsed)

**State:** User selected report type dari bottom sheet
**Shows:** Create Report screen dengan Report Type, Project, Company, Contact pre-filled dan collapsed
**Active Section:** Report Details (user starts here)

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃ ← Back     Buat Laporan Kunjungan  ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
┌─────────────────────────────────────┐
│                                     │
│ ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  │
│ 1 of 1 • Report Details             │ ← Progress indicator
│ ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  │    (skipped sections
│                                     │     not shown)
│ ✓ Report Type: Follow-up Meeting [✏️]│ ← Collapsed, read-only
│   (dari Project Detail context)     │    Green checkmark
│                                     │
│ ✓ Project: Factory Expansion Q4  [✏️]│ ← Collapsed, read-only
│   PT ABC Manufacturing              │    Pre-filled from context
│                                     │
│ ✓ Company: PT ABC Manufacturing  [✏️]│ ← Collapsed, read-only
│   (auto-filled dari project)        │    Grayed out
│                                     │
│ ✓ Contact: Budi Santoso          [✏️]│ ← Collapsed, read-only
│   (primary contact dari project)    │    Grayed out
│                                     │
│ ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  │
│ ▼ Report Details                    │ ← ACTIVE section
│ ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  │    (user starts here)
│                                     │
│ Visit Date *                        │
│ ┌───────────────────────────────┐   │
│ │ 06/11/2025 (Today)        📅  │   │ ← Auto-filled today
│ └───────────────────────────────┘   │
│                                     │
│ Attendees * (dari PT ABC Mfg)       │
│ ┌───────────────────────────────┐   │
│ │ [Pilih attendees...]       ▾  │   │ ← Multi-select
│ └───────────────────────────────┘   │    dropdown
│                                     │
│ Notes (optional)                    │
│ ┌───────────────────────────────┐   │
│ │                               │   │
│ │                               │   │
│ └───────────────────────────────┘   │
│   ... (scrollable)                  │
│                                     │
└─────────────────────────────────────┘
```

**Design Notes:**
- Collapsed sections menampilkan summary + ✓ checkmark + edit icon ✏️
- Pre-filled sections are read-only (cannot change without tapping edit)
- Progress indicator shows "1 of 1" (other sections skipped/hidden)
- User immediately starts filling Report Details
- Edit icons allow reopening sections if needed (edge case)

---

### Wireframe 21: Report Details Section - Active (User Filling Form)

**State:** User filling Report Details section
**Shows:** Expanded form fields untuk visit details, attendees, notes, photos, GPS

```
┌─────────────────────────────────────┐
│ (Collapsed sections above, scrolled │
│  out of view)                       │
│                                     │
│ ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  │
│ ▼ Report Details                    │
│ ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  │
│                                     │
│ Visit Date *                        │
│ ┌───────────────────────────────┐   │
│ │ 06/11/2025 (Today)        📅  │   │
│ └───────────────────────────────┘   │
│                                     │
│ Attendees * (Min 1)                 │
│ ┌───────────────────────────────┐   │
│ │ ☑ Budi Santoso (Primary)      │   │ ← Primary contact
│ │ ☑ Siti Rahayu                 │   │    pre-selected
│ │ ☐ Ahmad Wijaya                │   │
│ └───────────────────────────────┘   │
│                                     │
│ Notes (optional, max 5000 char)     │
│ ┌───────────────────────────────┐   │
│ │ Discussed technical specs__   │   │
│ │ for floor coating. Client     │   │
│ │ interested in water-based...  │   │
│ └───────────────────────────────┘   │
│                                     │
│ Next Action (optional)              │
│ ┌───────────────────────────────┐   │
│ │ Send quotation by Friday___   │   │
│ └───────────────────────────────┘   │
│                                     │
│ Outcome (optional)                  │
│ ┌───────────────────────────────┐   │
│ │ Positive                   ▾  │   │
│ └───────────────────────────────┘   │
│                                     │
│ Photos (optional, max 10)           │
│ ┌──────┐ ┌──────┐ ┌──────┐        │
│ │ [+]  │ │      │ │      │         │ ← Add photo
│ └──────┘ └──────┘ └──────┘         │    button
│                                     │
│ GPS Location                        │
│ ● Capturing... (10s timeout)        │ ← Auto-capture
│                                     │
│ ┌───────────────────────────────┐   │
│ │      SUBMIT REPORT            │   │ ← Enabled when
│ └───────────────────────────────┘   │    required fields
│                                     │    filled
└─────────────────────────────────────┘
```

**Design Notes:**
- Primary contact (Budi Santoso) pre-selected in Attendees
- Visit Date defaults to today
- All Report Details fields same as standard flow
- GPS auto-capture in background (non-blocking)
- Submit button enabled when: Visit Date + Attendees (min 1) filled

---

### Wireframe 22: Success + Navigate Back to Project Detail

**State:** Report successfully submitted
**Shows:** Brief success feedback + auto-navigate back to Project Detail screen

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃ ← Back    Factory Expansion    ✏️  ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
┌─────────────────────────────────────┐
│ Project Information                 │
│                                     │
│ Project Name                        │
│ Factory Expansion Q4 2025           │
│                                     │
│ ... (project details)               │
│                                     │
│ ┌───────────────────────────────┐   │
│ │  BUAT LAPORAN UNTUK           │   │
│ │  PROJECT INI                  │   │
│ └───────────────────────────────┘   │
│                                     │
└─────────────────────────────────────┘
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃ ✓ Laporan berhasil dibuat           ┃ ← Snackbar
┃                                     ┃    (green bg)
┃         [Lihat Laporan →]           ┃ ← Optional action
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Design Notes:**
- Immediately navigate back to Project Detail screen after submit
- Show success snackbar (green background, ✓ checkmark)
- Message: "Laporan berhasil dibuat"
- Optional: "Lihat Laporan" action button untuk navigate to Report Detail screen
- Snackbar auto-dismiss after 3 seconds
- Report appears in "Reports for this Project" list (if viewed)

---

## 🎨 Design System Summary

### Colors
- **Primary Green:** #4CAF50 (Submit, Create New, Checkmarks)
- **Inline Form BG:** #E3F2FD (Light blue untuk nested forms)
- **Text Primary:** #212121
- **Text Secondary:** #666666
- **Divider:** #E0E0E0
- **Error:** #F44336
- **Disabled:** #BDBDBD

### Key Measurements
- **Screen Padding:** 16dp
- **Section Spacing:** 24dp
- **Inline Form Indent:** 10dp
- **Dropdown Item Height:** 48dp
- **Button Height:** 48dp
- **Animation Duration:** 300ms (expand/collapse), 300ms (auto-scroll)

### Component Patterns
- **Combo Box:** Search box + "Create New" (pinned top) + Scrollable list
- **Inline Form:** Light blue bg + 10dp indent + Cancel/Continue buttons
- **Section Header:** Checkmark + Title + Edit icon (collapsed state)
- **Progress Indicator:** "X of Y • Section Name"

---

## 📊 Flow Comparison

| Aspect | Initial Visit | Follow-up Report |
|--------|--------------|------------------|
| **Steps** | 5 sections | 3 sections |
| **Project** | Buat baru atau pilih existing | Pilih existing saja |
| **Company** | Buat/pilih | Auto-filled (read-only) |
| **Contact** | Buat/pilih primary | Optional: tambah baru |
| **Time** | 5-10 menit (pertama kali) | 2-3 menit |
| **Use Case** | Customer baru (20%) | Repeat customer (80%) |

---

## 📝 Implementation Notes

### Flutter Widgets
- `ExpansionTile` untuk collapsible sections
- `Autocomplete` untuk combo boxes
- `ListView` dengan `ScrollController` untuk auto-scroll
- `Chips` untuk multi-select (Segmentation)
- `TextFormField` dengan `InputFormatter` untuk currency
- `ImagePicker` untuk photos
- `Geolocator` untuk GPS

### State Management (BLoC)
```dart
class ReportFormState {
  final ReportType? reportType;
  final Project? project;
  final Company? company;
  final Contact? contact;
  final ReportDetails? details;
  final Set<int> expandedSections; // Sections mana yang open
}
```

### Auto-Scroll Logic
```dart
void scrollToSection(int sectionIndex) {
  final renderBox = sectionKeys[sectionIndex].currentContext?.findRenderObject();
  scrollController.animateTo(
    renderBox.localToGlobal(Offset.zero).dy,
    duration: Duration(milliseconds: 300),
    curve: Curves.easeInOut,
  );
}
```

---

## ✅ Acceptance Criteria

- [ ] Report Type adalah field pertama
- [ ] Memilih "Initial Visit" menampilkan flow 5-section
- [ ] Memilih tipe lain menampilkan flow 3-section
- [ ] "Create New" selalu di top combo boxes
- [ ] Combo boxes bekerja tanpa typing (scrollable)
- [ ] Typing memfilter results secara real-time
- [ ] Sections unlock secara progressive
- [ ] Auto-scroll ke section berikutnya setelah completion
- [ ] Dapat tap edit icon untuk membuka ulang collapsed sections
- [ ] Semua validation bekerja secara real-time
- [ ] Draft auto-save setiap 30 detik
- [ ] Smooth animations (300ms)

---

**Document Status:** ✅ Complete
**Last Updated:** November 2025
**Version:** 2.0 (Corrected Flow)

---

**Navigation:**
- ← [Back to Screen Inventory](./SCREEN_INVENTORY.md)
- → [Next: User Stories](../developer-brief/USER_STORIES.md)
