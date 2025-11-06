# Designer Brief - Deliverables & Process

← [Sebelumnya: README](./README.md)

---

## 📋 Project Overview

### What You're Designing
Aplikasi mobile Android untuk sales representatives yang membuat laporan kunjungan customer. Core feature: **bekerja offline**, sync otomatis saat online.

### Why This Matters
Sales reps saat ini pakai WhatsApp untuk laporan (tidak terstruktur, sering hilang, susah di-track). Aplikasi ini memberikan structured forms, auto-save, dan manager visibility.

---

## 🎯 Design Deliverables

### 1. High-Fidelity Mockups (33 Screens - Updated)

**Format:**
- Figma file dengan 33 frames (+2 untuk Entry Point C: Create Report from Project Detail)
- Portrait orientation only
- Screen size: 360 × 800 dp (standard Android)
- Light mode only (dark mode di Phase 2)

**What to Include in Each Screen:**
- ✅ Real brand colors (from CSS Logo Guidelines)
- ✅ Real typography (Material Design 3 typography scale)
- ✅ Real components (Material buttons, text fields, cards)
- ✅ Placeholder text in **Bahasa Indonesia**
- ✅ Material Icons (from Google Material Symbols)
- ✅ Realistic content (example: company name "PT Surya Indah", not "Lorem Ipsum")

**Quality Standards:**
- Pixel-perfect alignment (gunakan 8dp grid)
- Consistent spacing (8dp, 16dp, 24dp increments)
- Proper contrast ratios (WCAG AA minimum)
- Touch targets minimum 48 × 48 dp

---

### 2. Design System (Figma Components)

**Create a separate Figma page: "Design System"**

**Must Include:**

#### **A. Color Tokens**
```
Primary Colors (from CSS Logo Guidelines)
├─ Primary-500 (main brand color)
├─ Primary-600 (dark variant)
└─ Primary-400 (light variant)

Neutral Colors (Material Design 3)
├─ Neutral-900 (text primary)
├─ Neutral-600 (text secondary)
├─ Neutral-300 (borders)
├─ Neutral-100 (backgrounds)
└─ Neutral-50 (surface)

Semantic Colors
├─ Success-500 (green - sync success)
├─ Error-500 (red - sync failed)
├─ Warning-500 (amber - draft)
└─ Info-500 (blue - offline indicator)
```

#### **B. Typography Scale**
Gunakan **default Material Design 3 typography**:
- Display Large (57sp) - rarely used
- Headline Large (32sp) - screen titles
- Headline Medium (28sp) - section headers
- Title Large (22sp) - card titles
- Title Medium (16sp) - list item titles
- Body Large (16sp) - main content
- Body Medium (14sp) - secondary content
- Label Large (14sp) - button labels

**Font:** Roboto (default Android system font)

#### **C. Component Library**
Create Figma components for:

**Form Components:**
- Text Input (empty state, filled state, error state)
- Dropdown/Select
- Date Picker (trigger button + calendar dialog)
- Multi-select (chips)
- Search Bar
- Radio Buttons
- Checkboxes

**Action Components:**
- Primary Button (enabled, disabled, loading)
- Secondary Button (outlined)
- Text Button
- FAB (Floating Action Button)
- Icon Button

**Display Components:**
- List Item (1-line, 2-line, 3-line)
- Card (elevated, outlined)
- Bottom Sheet
- Dialog/Modal
- Snackbar (success, error, info)
- Top App Bar
- Bottom Navigation Bar
- Tab Bar

**Status Indicators:**
- Sync Status Badge (synced, syncing, failed, offline)
- Upload Progress (per photo)
- Loading Spinner
- Empty State Template
- Error State Template

#### **D. Spacing System**
Document spacing units:
- 4dp - micro spacing (icon padding)
- 8dp - small spacing (between elements)
- 16dp - medium spacing (card padding)
- 24dp - large spacing (screen margins)
- 32dp - xlarge spacing (section separation)

#### **E. Elevation/Shadows**
Material Design 3 elevation levels:
- Level 0 - flat (no shadow)
- Level 1 - 2dp elevation (cards at rest)
- Level 2 - 4dp elevation (FAB at rest)
- Level 3 - 8dp elevation (dialogs)

---

### 3. Interactive Prototype (5 Main Flows)

**Create clickable Figma prototype untuk 5 user flows ini:**

#### **Flow 1: Create Report - Initial Visit (Offline)**
```
Home → Tap "Buat Laporan" →
Report Type dropdown → Pilih "Initial Visit" →
Project section unlocks + auto-scroll →
Tap "➕ Create New Project" (di TOP dropdown) →
Inline project form expands →
Fill project fields → Tap Continue →
Company section unlocks + auto-scroll →
Create company inline (if needed) →
Contact section unlocks + auto-scroll →
Create contact inline (if needed) →
Report Details section unlocks →
Fill visit date, attendees, notes →
Add photos → Submit →
Success (with "Syncing..." indicator)
```
**Why:** Ini adalah CORE feature (most complex flow - progressive disclosure dengan nested inline creation)

**Alternatif - Flow 1B: Create Report - Follow-up (Quick Flow)**
```
Home → Tap "Buat Laporan" →
Report Type dropdown → Pilih "Follow-up Meeting" →
Project section unlocks (existing projects only) →
Select existing project → Company/Contact auto-filled (read-only) →
Report Details section unlocks →
Fill visit date, attendees, notes → Add photos → Submit →
Success
```
**Why:** Tests quick flow untuk repeat customers (80% use case - 2-3 menit)

**Alternatif - Flow 1C: Create Report from Project Detail (NEW - Entry Point C)**
```
Home → Projects List → Select Project →
Project Detail Screen → Tap "Buat Laporan untuk Project Ini" →
Bottom Sheet (Report Type Picker) → Select "Follow-up Meeting" →
Create Report Screen (project pre-filled, sections collapsed) →
Report Details active (user starts here immediately) →
Fill details → Submit → Back to Project Detail + success toast
```
**Why:** Tests context-aware navigation + pre-fill behavior + bottom sheet pattern (NEW feature - US-4.5)

#### **Flow 2: Create Company**
```
Home → Companies List → Tap FAB →
Create Company Form → Fill Name/Address →
Submit → Success → Back to List (new company appears)
```
**Why:** Tests basic CRUD flow

#### **Flow 3: Edit Contact**
```
Home → Companies List → Select Company →
Contacts Tab → Select Contact →
Edit Button → Edit Form → Submit → Success
```
**Why:** Tests edit flow + navigation depth

#### **Flow 4: Sync Failed Handling**
```
Reports List (with "Failed to sync" indicator) →
Tap failed report → See error message →
Tap "Retry" → Syncing → Success
```
**Why:** Tests error recovery UX

#### **Flow 5: Manager View Reports**
```
Home (Manager Dashboard) → Team Reports Tab →
Filter by Sales Rep → View Report Detail →
See all fields (read-only, no edit button)
```
**Why:** Tests role-based access

**Prototype Requirements:**
- Add realistic transitions (200-300ms ease-in-out)
- Show loading states (use delay overlay)
- Include back navigation (back button in app bar)

---

### 4. States (15-20 Designs)

**Design these critical states:**

#### **Empty States (8 designs)**
1. Empty Companies List - "Belum ada perusahaan. Tap + untuk menambahkan."
2. Empty Contacts List - "Belum ada kontak untuk perusahaan ini."
3. Empty Projects List - "Belum ada proyek untuk perusahaan ini."
4. Empty Reports List - "Belum ada laporan. Tap + untuk membuat laporan pertama."
5. Empty Search Results - "Tidak ada hasil untuk '[search query]'."
6. Empty Notifications - "Tidak ada notifikasi."
7. No Internet + Empty List - "Anda offline dan belum ada data lokal."
8. Manager Empty Dashboard - "Tidak ada laporan dari tim. (This shouldn't happen, but design anyway)"

**Empty State Design Pattern:**
- Illustration or icon (simple, 1-color)
- Heading (16sp, medium weight)
- Body text (14sp, secondary color)
- CTA button (if applicable)

#### **Error States (5 designs)**
1. Network Error - "Tidak ada koneksi internet" (with Retry button)
2. Sync Failed - "Gagal menyinkronkan data" (with details + Retry)
3. Form Validation Error - Red text under field + error icon
4. Photo Upload Failed - "Gagal upload foto 3 dari 10" (show which photos failed)
5. Generic Error - "Terjadi kesalahan. Silakan coba lagi."

#### **Loading States (4 designs)**
1. List Loading - Shimmer/skeleton placeholders (not just spinner)
2. Form Submitting - Button shows spinner + "Menyimpan..."
3. Photo Uploading - Progress bar per photo (0-100%)
4. Sync in Progress - Top banner "Menyinkronkan 5 dari 10 item..."

#### **Success States (3 designs)**
1. Snackbar - "Perusahaan berhasil ditambahkan" (green, 3 seconds)
2. Sync Success - "Semua data tersinkronisasi" (green badge)
3. Report Submitted - Success screen with checkmark + "Laporan berhasil disimpan"

---

### 5. Icon List (Material Icons)

**Export a list of Material Icons you used:**

Create a Figma page "Icons Used" dengan list:
- Icon name (e.g., "add_circle")
- Where it's used (e.g., "FAB on Companies List")
- Size (e.g., 24dp)
- Link to Material Symbols: https://fonts.google.com/icons

**Common Icons Needed:**
- Navigation: home, arrow_back, menu, more_vert
- Actions: add, edit, delete, search, filter, refresh
- Content: business, person, folder, description, photo_camera, location_on
- Status: check_circle, error, sync, cloud_upload, wifi_off
- UI: close, expand_more, chevron_right

---

## 🔄 Review & Iteration Process

### Phase 1: First 5 Screens Review

**You Design:**
1. Create Report Form - Progressive Disclosure Flow (4-5 screens/states):
   - Initial state (Report Type active, sections locked)
   - After Initial Visit selected (Project section unlocked)
   - Inline project creation form expanded
   - Report Details section (final form)
   - Success state
2. Companies List + Create/Edit (3 screens)
3. Contacts List (1 screen)

**Total: ~8-9 screens** (termasuk states untuk progressive disclosure flow)

**Catatan Penting untuk Create Report:**
- Design harus show progressive disclosure (sections unlock satu-per-satu)
- "➕ Create New" di TOP dropdown (FIXED position, green text, always visible)
- Inline forms punya light blue background (#E3F2FD), 10dp indent
- Completed sections collapse dengan ✓ checkmark + edit icon ✏️
- Progress indicator: "X of Y • Section Name"
- Lihat NESTED_INLINE_CREATION_WIREFRAMES.md untuk detailed specifications

**Submit:**
- Figma file link (set to "Anyone with link can comment")
- Tag Product Owner as reviewer
- Add comment: "5 priority screens ready for review"

**Product Owner Reviews:**
- Via Figma comments for specific feedback
- Via Google Meet if major changes needed

**You Revise:**
- Address all feedback
- Re-tag Product Owner when done
- Wait for "APPROVED" comment

**IMPORTANT:** Don't proceed to remaining screens until these 5 are APPROVED.

---

### Phase 2: Remaining Screens (In Batches)

**Batch 1 (10 screens):**
- Projects List + Create/Edit + Detail
- Reports List (My Reports, Team Reports)
- Report Detail View
- Submit for review → Revise → Get approval

**Batch 2 (10 screens):**
- Dashboard/Home
- Sync Status Screen
- Settings & Profile
- Search Results
- Submit for review → Revise → Get approval

**Batch 3 (8 screens):**
- All states (empty, error, loading, success)
- Dialogs (confirmation, alerts)
- Onboarding/Splash
- Submit for review → Final approval

---

### Phase 3: Design System & Prototype

**After all screens approved:**
- Finalize Design System page
- Create 5 interactive prototypes
- Document all components
- Final review

---

## ✅ Definition of Done

**Before marking design as "COMPLETE", ensure:**

### Checklist:

**Screens:**
- [ ] All 33 screens designed (hi-fidelity)
- [ ] All screens use brand colors from CSS Logo Guidelines
- [ ] All screens use Material Design 3 components
- [ ] All text is placeholder Bahasa Indonesia (realistic content)
- [ ] All screens are 360 × 800 dp (portrait)
- [ ] Consistent spacing (8dp grid)
- [ ] Consistent typography (Material typography scale)

**Design System:**
- [ ] Color tokens documented with hex codes
- [ ] Typography scale documented
- [ ] All components created as Figma components (reusable)
- [ ] Spacing system documented
- [ ] Elevation levels documented

**States:**
- [ ] 8 empty states designed
- [ ] 5 error states designed
- [ ] 4 loading states designed
- [ ] 3 success states designed

**Prototype:**
- [ ] Flow 1: Create Report (clickable)
- [ ] Flow 2: Create Company (clickable)
- [ ] Flow 3: Edit Contact (clickable)
- [ ] Flow 4: Sync Failed Handling (clickable)
- [ ] Flow 5: Manager View Reports (clickable)

**Icons:**
- [ ] Icon list page created
- [ ] All icons are Material Icons (no custom icons unless approved)
- [ ] Icon sizes documented (16dp, 24dp, 32dp)

**Handoff:**
- [ ] Figma file organized (clear page names, frame names)
- [ ] All components labeled clearly
- [ ] Product Owner has reviewed and approved all screens

---

## 🚨 Common Mistakes to Avoid

### ❌ DON'T:
1. **Design custom components** that don't exist in Material Design 3
   - Stick to standard Material components (easier for developer)

2. **Use Lorem Ipsum text**
   - Use realistic Bahasa Indonesia content

3. **Design for iPhone**
   - This is Android-only (Material Design, not iOS)

4. **Ignore offline UX**
   - Offline indicator, sync status are CRITICAL features

5. **Design complex animations**
   - Keep animations simple (fade, slide) - no fancy stuff

6. **Use custom fonts**
   - Roboto only (default Android system font)

7. **Design landscape orientation**
   - Portrait only for MVP

8. **Create 100+ screens**
   - 33 screens only (including states + Entry Point C) - don't overcomplicate

### ✅ DO:
1. **Follow Material Design 3 guidelines**
   - Read: https://m3.material.io/

2. **Use 8dp grid system**
   - Everything aligned to 8dp increments

3. **Design for offline-first**
   - Show sync status, draft indicators, offline banners

4. **Keep it simple and clear**
   - This is a work tool, not a lifestyle app

5. **Use realistic content**
   - "PT Surya Indah" not "Company Name Here"

6. **Think about touch targets**
   - Minimum 48 × 48 dp for buttons/tappable areas

7. **Ask questions early**
   - Better to clarify than redesign later

---

## 🛠️ Tools & Setup

### Required:
- **Figma** (desktop app or web)
- **CSS Logo Guidelines.pdf** (in this folder)
- **NESTED_INLINE_CREATION_WIREFRAMES.md** (complete specifications untuk Create Report flow)
- **SCREEN_INVENTORY.md** (list semua 33 screens dengan details - includes Entry Point C)
- **Material Design 3 Figma Kit** (optional but helpful)
  - Download: https://www.figma.com/community/file/1035203688168086460

### Optional but Recommended:
- **Material Symbols** plugin for Figma
- **Stark** plugin (for accessibility contrast checking)
- **Figma to Android** plugin (for exporting specs later)

---

## 📞 Communication

### Questions During Design:
- Post in Figma comments (tag @ProductOwner)
- For urgent questions: Request Google Meet

### Feedback Timeline:
- Product Owner will review within 2-3 business days
- If no response after 3 days, ping via Figma comments

### Approval Process:
- Product Owner comments "APPROVED" on Figma
- Don't proceed to next phase without approval

---

## 🎯 Success Criteria

**Your design is successful if:**
1. ✅ Sales reps say: "This looks easier than WhatsApp"
2. ✅ All screens follow Material Design 3 (developer can implement quickly)
3. ✅ Offline UX is clear (users understand when they're offline)
4. ✅ Forms are simple and fast to fill
5. ✅ Manager can easily view team reports

**Your design is NOT successful if:**
1. ❌ Sales reps say: "This looks complicated"
2. ❌ Developer says: "This design is impossible to implement"
3. ❌ Users don't understand sync status
4. ❌ Forms require too many steps/screens

---

## 📍 Navigasi

**Selesai membaca Designer Brief?**

➡️ **[Lanjut ke: USER_PERSONAS.md →](./USER_PERSONAS.md)**

**Atau kembali ke:**
← [README](./README.md)
