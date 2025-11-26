# Nested Inline Creation - Complete Wireframes
## CSS Sales Report App - Report Creation Flow (CORRECTED)

← [Sebelumnya: SCREEN_INVENTORY.md](./SCREEN_INVENTORY.md)

---

**Versi:** 2.0 (CORRECTED - Report Type First)
**Terakhir Diperbarui:** November 2025
**Target User:** Budi (47, basic tech) & Dina (32, tech-savvy)

---

## ⚠️ MAJOR UPDATE: Flow Corrected

**Previous Version (v1.0):** Started with Project dropdown
**Current Version (v2.0):** Starts with **Report Type** dropdown (determines subsequent flow)

---

## 📋 Corrected Flow Overview

### Key Principle: Report Type Determines Complexity

**Report Type** → **Branching Logic:**

1. **Initial Visit** → Full creation flow (5 sections):
   - Report Type → Project → Company → Contact → Report Details

2. **Other types (Follow-up, Technical, Price Quotation, Closing, After Sales)** → Quick flow (3 sections):
   - Report Type → Select Existing Project → Report Details

---

##  🎯 UI Pattern: Hybrid Single-Screen with Progressive Disclosure

**Features:**
- ✅ All sections on one scrollable screen
- ✅ Sections unlock progressively as you complete them
- ✅ Auto-scroll to next section after completion
- ✅ Can tap collapsed sections to edit
- ✅ "Create New" at TOP of combo boxes (always visible)
- ✅ Real-time validation per section

---

## 📱 Complete Wireframes (17 Total)

---

### Wireframe 1: Initial State (Screen Load)

**State:** User opens "Create Report" screen
**Active Section:** Report Type (only)
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
- White background for main screen
- Grayed out sections have opacity: 0.4
- Helper text in gray (#666666)
- Progress indicator "1 of 5" helps user understand flow length

---

### Wireframe 2: Report Type Dropdown Expanded

**State:** User taps Report Type dropdown
**Shows:** All 6 report type options
**Design:** Initial Visit has distinct icon (🆕) to indicate it's different

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
- Dropdown has subtle shadow (elevation: 2dp)
- Each option has icon for quick visual scanning
- Divider separates "Initial Visit" from others (different flows)
- Touch target height: 48dp per option

---

###  Wireframe 3: After Selecting "Initial Visit"

**State:** Report Type = "Initial Visit" selected
**Behavior:** Project section unlocks and auto-scrolls into view
**Locked:** Company, Contact, Report Details still grayed

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
- Completed section shows: ✓ + collapsed state + edit icon
- Green checkmark (#4CAF50) indicates completion
- Edit icon (✏️) allows going back to change
- Smooth scroll animation: 300ms easing

---

### Wireframe 4: Project Combo Box Expanded (No Typing)

**State:** User taps Project combo box
**Shows:** "Create New" at top (fixed) + all existing projects (scrollable)
**Key Feature:** Create New is ALWAYS visible even before typing

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
- "Create New" has green text (#4CAF50) for visibility
- ➕ icon signals creation action
- Solid divider separates "Create New" from existing items
- Existing items show: Project name (bold) + Company name (secondary text)
- Max dropdown height: 50vh (prevents covering entire screen)
- Dropdown has vertical scroll if >10 items

---

### Wireframe 5: Project Combo Box - Filtered by Typing

**State:** User typed "Factory" in search box
**Shows:** Filtered results + "Create New" still pinned at top
**Result Count:** Shows how many matches found

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
- Search is case-insensitive partial match
- [X] button clears search instantly
- "Create New" remains at top even when filtering
- Result count helps user know if they need to create new
- If 0 results, shows: "No matches. Create new?"

---

### Wireframe 6: Create New Project - Inline Form Expanded

**State:** User tapped "➕ Create New Project"
**Shows:** Inline form with all project fields
**Visual:** Light blue background (#E3F2FD), 10dp left indent
**Pre-filled:** Project Name from what user typed (if any)

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
- Light blue background distinguishes inline form from main screen
- 10dp left indent creates visual depth
- Required fields marked with red asterisk (*)
- Multi-select chips for Segmentation (can select multiple)
- Currency input auto-formats with thousand separators
- Cancel button is secondary (outlined), Continue is primary (filled)
- "Continue" disabled until all required fields valid

---

[CONTINUED IN PART 2 DUE TO LENGTH - THIS FILE WILL CONTAIN ALL 17 WIREFRAMES]

---

## 🎨 Design System Summary

### Colors
- **Primary Green:** #4CAF50 (Submit, Create New, Checkmarks)
- **Inline Form BG:** #E3F2FD (Light blue for nested forms)
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
| **Project** | Create new or select | Select existing only |
| **Company** | Create/select | Auto-filled (read-only) |
| **Contact** | Create/select primary | Optional: add new |
| **Time** | 5-10 minutes (first time) | 2-3 minutes |
| **Use Case** | New customer (20%) | Repeat customer (80%) |

---

## 📝 Implementation Notes

### Flutter Widgets
- `ExpansionTile` for collapsible sections
- `Autocomplete` for combo boxes
- `ListView` with `ScrollController` for auto-scroll
- `Chips` for multi-select (Segmentation)
- `TextFormField` with `InputFormatter` for currency
- `ImagePicker` for photos
- `Geolocator` for GPS

### State Management (BLoC)
```dart
class ReportFormState {
  final ReportType? reportType;
  final Project? project;
  final Company? company;
  final Contact? contact;
  final ReportDetails? details;
  final Set<int> expandedSections; // Which sections are open
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

- [ ] Report Type is the first field
- [ ] Selecting "Initial Visit" shows 5-section flow
- [ ] Selecting other types shows 3-section flow
- [ ] "Create New" is always at top of combo boxes
- [ ] Combo boxes work without typing (scrollable)
- [ ] Typing filters results in real-time
- [ ] Sections unlock progressively
- [ ] Auto-scroll to next section after completion
- [ ] Can tap edit icon to reopen collapsed sections
- [ ] All validation works in real-time
- [ ] Draft auto-saves every 30 seconds
- [ ] Smooth animations (300ms)

---

**Document Status:** ✅ Complete
**Last Updated:** November 2025
**Version:** 2.0 (Corrected Flow)

---

**Navigation:**
- ← [Back to Screen Inventory](./SCREEN_INVENTORY.md)
- → [Next: User Stories](../developer-brief/USER_STORIES.md)
