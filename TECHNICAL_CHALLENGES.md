# Technical Challenges - Why This Project is Complex

**Purpose of this document:** Explain why this is NOT a basic CRUD app and help you estimate effort accurately.

---

## ⚠️ TL;DR - This is NOT a Basic CRUD App

If you're thinking "it's just forms + database", **think again**.

**Key Complexity Factors:**
1. 🔴 **Offline-first architecture** (not just "add SQLite")
2. 🔴 **31 Row-Level Security policies** (strict data isolation)
3. 🔴 **Background sync with conflict resolution** (not trivial)
4. 🔴 **Per-photo upload status tracking** (robust error handling)
5. 🔴 **Clean Architecture + BLoC** (strict code organization)
6. 🔴 **Test coverage requirements** (>60% overall, >80% business logic)

---

## 🧩 Challenge #1: Offline-First Architecture

### What It Means

**NOT** "just add SQLite as cache"

**YES** "complete local-first system with sync strategy"

### Technical Requirements

**Local Database (Drift/SQLite):**
- ✅ All 8 tables replicated locally
- ✅ Full CRUD operations work offline
- ✅ Draft auto-save every 30 seconds
- ✅ Query performance (pagination 20 items/page)
- ✅ Data persistence across app restarts

**Sync Queue:**
- ✅ Transaction log for all local changes
- ✅ Retry logic with exponential backoff
- ✅ Per-record sync status tracking
- ✅ Rollback capability if sync fails
- ✅ Handle network interruptions gracefully

**Conflict Resolution:**
- ✅ Last-Write-Wins for MVP (automatic)
- ✅ Timestamp comparison (`updated_at` field)
- ✅ Handle edge cases (edit while syncing, etc)

### Why It's Hard

1. **State Management Complexity**
   - Need to track: local state, sync status, network status
   - BLoC pattern required untuk manage complex state

2. **Edge Cases Galore**
   - What if user edits while offline, then online sync fails?
   - What if user creates record offline, then another user creates same record online?
   - What if photo upload succeeds but metadata sync fails?
   - What if app crashes mid-sync?

3. **Testing Difficulty**
   - Need to simulate offline/online transitions
   - Need to test all edge cases
   - Integration tests are critical

### Estimated Effort

- **Basic CRUD with SQLite:** 2-3 days
- **Proper offline-first with sync:** 2-3 weeks

**10x complexity difference.**

---

## 🔐 Challenge #2: Row-Level Security (RLS) Policies

### What It Means

**31 RLS policies** di Supabase untuk ensure strict data isolation.

**Requirements:**
- ✅ Sales Rep A cannot see Sales Rep B's data (companies, contacts, projects, reports)
- ✅ Manager can see ALL team data (read-only)
- ✅ Users can only edit their own records
- ✅ RLS policies must be tested **before** development

### Technical Requirements

**Supabase RLS Policies (8 tables × ~4 policies each):**
```sql
-- Example: Reports table
CREATE POLICY "Users can view own reports"
  ON reports FOR SELECT
  USING (auth.uid() = created_by);

CREATE POLICY "Managers can view all reports"
  ON reports FOR SELECT
  USING (
    EXISTS (
      SELECT 1 FROM users
      WHERE users.id = auth.uid()
      AND users.role = 'manager'
    )
  );

CREATE POLICY "Users can insert own reports"
  ON reports FOR INSERT
  WITH CHECK (auth.uid() = created_by);

CREATE POLICY "Users can update own reports"
  ON reports FOR UPDATE
  USING (auth.uid() = created_by);
```

**Testing Requirements:**
- ✅ Create 2 test users (sales rep A & B)
- ✅ Verify sales rep A cannot see B's data
- ✅ Verify manager can see both A & B's data
- ✅ Test all CRUD operations per role
- ✅ Test edge cases (shared contacts, etc)

### Why It's Hard

1. **SQL Complexity**
   - RLS policies use PostgreSQL functions
   - Need to understand `auth.uid()`, `EXISTS`, subqueries
   - Policy debugging is not straightforward

2. **Testing Overhead**
   - Cannot just "test in production"
   - Need dedicated test environment
   - Must test with multiple user accounts

3. **Integration with Local DB**
   - Local Drift DB does NOT enforce RLS
   - Client-side filtering required
   - Potential for data leakage if implemented wrong

### Estimated Effort

- **Basic CRUD (no RLS):** 1 week
- **With RLS policies + testing:** 2-3 weeks

**2-3x complexity difference.**

---

## 📸 Challenge #3: Photo Upload Management

### What It Means

**NOT** just "upload file to storage"

**YES** "robust multi-photo upload with individual status tracking"

### Technical Requirements

**Per-Photo Status Tracking:**
- ✅ Each photo has status: `pending`, `uploading`, `success`, `failed`
- ✅ Retry failed uploads (max 3 attempts)
- ✅ Show progress per photo (not just "uploading...")
- ✅ Continue other photos if one fails (don't block)
- ✅ Compression before upload (<500KB per photo)

**Sync Logic:**
```dart
Future<void> uploadReportPhotos(String reportId, List<Photo> photos) async {
  await Future.wait(
    photos.map((photo) async {
      try {
        // Mark as uploading
        await db.updatePhotoStatus(photo.id, 'uploading');

        // Compress
        final compressed = await compress(photo);

        // Upload to Supabase Storage
        await supabase.storage.uploadBinary(compressed);

        // Mark as success
        await db.updatePhotoStatus(photo.id, 'success');
      } catch (e) {
        // Mark as failed
        await db.updatePhotoStatus(photo.id, 'failed');
        await db.incrementRetryCount(photo.id);
      }
    }),
    eagerError: false, // Don't stop if one fails
  );
}
```

**UI Requirements:**
- ✅ Show upload progress per photo
- ✅ Allow retry failed photos
- ✅ Gracefully handle network errors
- ✅ Work offline (queue for later)

### Why It's Hard

1. **Error Handling Complexity**
   - Network can fail mid-upload
   - Storage might be full
   - File might be corrupted
   - Need to handle ALL edge cases

2. **UX Considerations**
   - User might close app while uploading
   - Need background upload service
   - Show clear status to user

3. **Storage Management**
   - Need to cleanup failed uploads
   - Handle large files (compression required)
   - Supabase storage limits

### Estimated Effort

- **Basic photo upload:** 2-3 days
- **Robust with status tracking + retry:** 1-2 weeks

**5x complexity difference.**

---

## 🏗️ Challenge #4: Clean Architecture + BLoC

### What It Means

**NOT** "put all code in one file"

**YES** "strict 3-layer separation with dependency injection"

### Required Structure

```
lib/
├── core/                         # Shared utilities
│   ├── error/                    # Exceptions & failures
│   ├── usecases/                 # Base UseCase interface
│   └── network/                  # Network connectivity check
├── features/
│   ├── auth/
│   │   ├── data/                 # Models, datasources, repo impl
│   │   ├── domain/               # Entities, repo interfaces, usecases
│   │   └── presentation/         # BLoC, pages, widgets
│   ├── companies/
│   ├── contacts/
│   ├── projects/
│   ├── reports/
│   └── sync/
└── main.dart
```

**Dependency Rule:**
```
Presentation → Data → Domain
```
- Presentation can import Data & Domain
- Data can import Domain only
- Domain imports NOTHING (pure Dart)

### Why It's Hard

1. **Learning Curve**
   - If you've never done Clean Architecture, steep learning curve
   - BLoC pattern has boilerplate

2. **More Files**
   - Simple feature = 10+ files (entity, model, repo interface, repo impl, usecase, bloc, page, widgets)
   - Folder structure must be consistent

3. **Testing Requirements**
   - Each layer needs separate tests
   - Mocking dependencies required
   - Test coverage >60% overall, >80% business logic

### Estimated Effort

- **Basic MVC structure:** 1 week for all features
- **Clean Architecture + BLoC:** 3-4 weeks for all features

**3-4x complexity difference.**

---

## ✅ Challenge #5: Code Quality Standards

### Requirements

**Flutter Analyze:**
- ✅ `flutter analyze` must be 100% clean (0 errors, 0 warnings)
- ✅ No ignored warnings/lints

**Test Coverage:**
- ✅ Unit tests for all BLoCs (>80%)
- ✅ Unit tests for all UseCases (>80%)
- ✅ Unit tests for all Repositories (>80%)
- ✅ Widget tests for key UI components
- ✅ Integration tests for critical flows
- ✅ Overall coverage >60%

**Code Review:**
- ✅ Functions <20 lines where possible
- ✅ Clear variable/function names
- ✅ No code duplication
- ✅ Dartdoc comments on public APIs
- ✅ No `TODO` comments in committed code

### Why It's Hard

1. **Time Investment**
   - Writing tests takes time (sometimes 2x implementation time)
   - Maintaining clean code requires discipline

2. **Skill Required**
   - Need to know how to write good tests
   - Need to understand mocking, dependency injection

### Estimated Effort

- **No tests, basic code:** 10 weeks
- **With tests + clean code:** 12-14 weeks

**20% time overhead.**

---

## 📊 Complexity Comparison

| Feature | Basic CRUD App | This Project | Multiplier |
|---------|---------------|--------------|------------|
| **Database** | 1 table, no relations | 8 tables, 31 RLS policies | 8x |
| **Offline** | Not required | Full offline-first + sync | 10x |
| **Photo Upload** | Basic upload | Per-photo status + retry | 5x |
| **Architecture** | Any structure | Clean Arch + BLoC | 3x |
| **Testing** | Optional | >60% coverage required | 2x |
| **Code Quality** | "Works" is enough | `flutter analyze` clean | 1.5x |

**Overall Complexity:** This project is **20-30x more complex** than a basic CRUD app.

---

## 💡 What This Means for Your Estimate

### If You've Only Built Basic CRUD Apps

**Your initial estimate is probably 3-5x too low.**

**Example:**
- You think: "8 tables × 3 days = 24 days = Rp 4 juta"
- Reality: "Offline-first + RLS + sync + tests = 12 weeks = Rp 18 juta"

**Advice:** Be honest about your experience level. If you've never done offline-first, say so and price accordingly (include learning time).

### If You've Built Offline-First Apps Before

**You're in a good position, but don't underestimate:**
- 31 RLS policies need careful testing
- Clean Architecture adds structure overhead
- Photo management with status tracking is non-trivial

**Advice:** Reference your past projects. Show you understand the complexity.

### Red Flags in Proposals

- ❌ "4 juta cukup untuk semua, 3 minggu selesai"
- ❌ No mention of offline-first complexity
- ❌ No mention of RLS testing strategy
- ❌ Generic estimate without per-Epic breakdown

### Good Proposals

- ✅ "4 juta cukup untuk Epic 1-3 (Authentication + Companies + Contacts), estimated 3-4 weeks"
- ✅ "Full MVP (79 points) realistic budget adalah Rp 16 juta, 11 weeks, breakdown terlampir"
- ✅ Mentions specific challenges: "RLS testing akan saya lakukan dengan 2 test users..."

---

## 🔗 Next Steps

After understanding technical challenges:
1. ✅ Read **DATABASE_SCHEMA.md** - See the 31 RLS policies
2. ✅ Read **SYNC_STRATEGY.md** - Understand offline-first sync logic
3. ✅ Read **USER_STORIES.md** - Full requirements with edge cases
4. ✅ Read **HOW_TO_PROPOSE.md** - Guide untuk calculate realistic budget
5. ✅ Fill Google Form with honest, detailed proposal

---

**Remember:** Underpromising & overdelivering >> Overpromising & underdelivering.

We'd rather hear "I can only do 40 points with Rp 4M" than "I can do all 79 points" and then see poor quality or missed deadline.

**Good luck! 🚀**
