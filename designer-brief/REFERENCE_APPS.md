# Reference Apps - Contoh UX Yang Bagus

← [Sebelumnya: DESIGN_REQUIREMENTS](./DESIGN_REQUIREMENTS.md)

---

## 📋 Overview

Dokumen ini berisi reference apps dengan **good offline-first UX patterns** yang bisa Anda pelajari.

**Tujuan:** Inspirasi untuk design patterns, BUKAN copy-paste entire design.

---

## 🎯 Apps dengan Offline-First UX yang Bagus

### 1. **Google Keep** (Notes App)

**Why Study This:**
- ✅ Excellent offline UX (create notes tanpa internet)
- ✅ Clear sync indicators per note
- ✅ Draft auto-save (never lose data)
- ✅ Simple Material Design implementation

**What to Learn:**
- Sync status badge di note cards (small, subtle)
- "Saving..." indicator saat type (bottom of screen, small text)
- Offline banner (yellow, at top: "No connection. Changes will be synced when online.")
- Color coding untuk status (yellow = pending, tidak menggangu UX)

**Download:** Google Play Store (free)

---

### 2. **Trello** (Project Management)

**Why Study This:**
- ✅ Works offline (create cards, move cards, edit)
- ✅ Clear "Offline" indicator (prominent banner)
- ✅ Sync conflict resolution UI (rare, but handled well)

**What to Learn:**
- Offline banner design (red banner at top, clear message)
- Draft indicators (small text below card title: "Saved locally")
- Sync queue visibility (show how many items pending)
- Error handling (if sync fails, show retry button on THAT item, not global error)

**Download:** Google Play Store (free)

---

### 3. **Pocket** (Read Later)

**Why Study This:**
- ✅ Offline reading (download articles)
- ✅ Sync status per article (cloud icon, download icon, checkmark)
- ✅ Clear download progress

**What to Learn:**
- Icon-based status indicators (cloud, download arrow, checkmark)
- Progress bars untuk download (minimal, not blocking UI)
- Queue management (show "3 articles waiting to sync")

**Download:** Google Play Store (free)

---

### 4. **Evernote** (Notes App)

**Why Study This:**
- ✅ Draft auto-save dengan timestamp
- ✅ Sync conflicts handled gracefully
- ✅ Clear messaging untuk offline state

**What to Learn:**
- "Last edited X minutes ago" indicator (builds trust)
- Sync error notification dengan actionable button ("Retry" or "View Details")
- Multi-device sync status (show which device made last edit)

**Download:** Google Play Store (free tier)

---

### 5. **Microsoft To Do**

**Why Study This:**
- ✅ Simple Material Design implementation
- ✅ Offline task creation
- ✅ Minimal, non-intrusive sync indicators

**What to Learn:**
- Sync icon di app bar (small, tappable to see status detail)
- Snackbar notifications untuk sync events ("All tasks synced" - auto-dismiss 3 seconds)
- No aggressive "you're offline!" warnings (respects user)

**Download:** Google Play Store (free)

---

## 🎨 Specific UX Patterns to Study

### **Offline Indicators**

**Good Examples:**
- **Gmail:** Small gray text "No connection" di bottom (tidak mengganggu)
- **WhatsApp:** Gray banner at top "Waiting for connection..." dengan spinner
- **Google Drive:** Cloud icon dengan X badge (small, di app bar)

**What to Learn:**
- Persistent but not obtrusive (jangan full-screen blocking modals)
- Clear actionable message ("Changes will sync when online" - not just "No internet")
- Color: Gray atau amber (bukan red, red = error, offline bukan error)

---

### **Sync Status Per Item**

**Good Examples:**
- **Google Photos:** Small cloud icon dengan progress circle per photo
- **Dropbox:** Icon badge per file (synced, syncing, pending)
- **Notion:** Dot indicators (green = synced, gray = pending)

**What to Learn:**
- Icons > Text (save space)
- Color coding (green = synced, blue = syncing, amber = pending, red = failed)
- Tappable for details (tap icon → show full sync details)

---

### **Draft Auto-Save**

**Good Examples:**
- **Google Docs:** "Saving..." → "All changes saved to Drive" (small text, top center)
- **Medium (writing app):** "Draft saved" (subtle, auto-dismiss)
- **WordPress app:** "Last saved X minutes ago" (always visible, bottom)

**What to Learn:**
- Always show last save time (builds trust)
- Animate state change: "Saving..." (spinner) → "Saved" (checkmark) → fade to timestamp
- Position: Bottom of form (tidak blocking content)

---

### **Photo Upload Progress**

**Good Examples:**
- **Instagram:** Progress circle per photo, tap to retry if failed
- **Google Photos:** Grid view, each photo shows upload %
- **WhatsApp:** Linear progress bar per photo di chat

**What to Learn:**
- Show % complete (0-100%)
- Per-item status (don't show "Uploading 5 photos" - show status EACH photo)
- Retry button jika failed (on THAT photo, not global retry)
- Continue other uploads if one fails (don't block everything)

---

### **Error Handling**

**Good Examples:**
- **Spotify Download Failed:** "Couldn't download. Check connection and try again." + Retry button
- **Slack Message Failed:** Red indicator on failed message + tap to retry
- **Gmail Send Failed:** "Message sending failed" snackbar + "Retry" action

**What to Learn:**
- Tell user WHAT went wrong + HOW to fix it
- Provide action button (Retry, Details, Dismiss)
- Don't just say "Error" (useless)
- Keep failed items visible dengan indicator (don't hide them)

---

## 📱 Material Design 3 Examples

### **Official Material Design 3 Apps**

Study these for proper Material Design implementation:

1. **Google Tasks** - Simple MD3 implementation
2. **Google Calendar** - Cards, FABs, navigation
3. **Google Contacts** - Lists, forms, detail views
4. **Gmail** - Complex list views dengan filtering

**What to Study:**
- Button styles (filled, outlined, text)
- Card elevations (level 1 for cards at rest)
- Typography scale (headline, title, body, label)
- Color system (primary, secondary, tertiary)
- Touch targets (48x48dp minimum)

---

## 🎯 What NOT to Copy

### ❌ **Don't Copy These Patterns:**

1. **Aggressive Offline Warnings**
   - Example: Full-screen modal blocking everything dengan "NO INTERNET"
   - Why bad: Annoying, user knows they're offline
   - Better: Small banner at top, non-blocking

2. **Sync Without Feedback**
   - Example: Apps yang sync di background tanpa indicator
   - Why bad: User tidak tahu if data saved or not (anxiety)
   - Better: Always show sync status (subtle but visible)

3. **Blocking Sync Operations**
   - Example: "Syncing... please wait" (full-screen spinner)
   - Why bad: User cannot continue work
   - Better: Sync di background, allow user to continue

4. **No Draft Save Indicators**
   - Example: Forms yang save tanpa feedback
   - Why bad: User tidak tahu if data saved (data loss anxiety)
   - Better: Show "Saved 1 minute ago"

---

## 📋 Action Items for You

### **Before Designing, Do This:**

1. **Download & Test These Apps** (1-2 hours)
   - Google Keep
   - Trello
   - Microsoft To Do
   - At least one of the above

2. **Take Screenshots** (30 min)
   - Offline indicators
   - Sync status badges
   - Error messages
   - Loading states
   - Empty states
   - Create Figma board dengan collected screenshots

3. **Identify Patterns** (30 min)
   - What's common across good apps?
   - What patterns work well for offline UX?
   - What can you adapt untuk CSS Sales Report?

---

## 🎨 Design Inspiration (Optional)

### **UI Design Portfolios:**

- **Dribbble:** Search "offline app", "sync indicator", "mobile dashboard"
- **Behance:** Search "Material Design 3", "Android app design"
- **Mobbin:** Mobile app design patterns library (requires subscription)

### **Material Design Gallery:**

- https://m3.material.io/components - Official MD3 components
- https://material.io/design/material-studies - Case studies

---

## ⚠️ Important Reminder

**Reference apps = Inspiration for PATTERNS, not entire designs**

- ✅ DO: Study how they handle offline UX, sync status, error states
- ✅ DO: Adapt patterns to fit CSS Sales Report needs
- ❌ DON'T: Copy entire UI design (that's plagiarism)
- ❌ DON'T: Copy brand colors, specific layouts

**Your design harus:**
- Follow CSS brand identity (see CSS Logo Guidelines.pdf)
- Use Material Design 3 components
- Be unique to CSS Sales Report (not clone of other apps)

---

## 📍 Navigasi

**Selesai membaca semua dokumentasi?**

**✅ Anda sudah siap untuk mulai design!**

**Next Steps:**
1. Download & test reference apps (1-2 jam)
2. Review CSS Logo Guidelines.pdf sekali lagi
3. Buka Figma → Start dengan 5 priority screens
4. Submit ke Product Owner untuk review

**Atau kembali ke:**
← [README](./README.md) (review alur baca dokumentasi)

---

**Good luck designing! 🎨**

Jika ada questions, post di Figma comments atau request Google Meet dengan Product Owner.
