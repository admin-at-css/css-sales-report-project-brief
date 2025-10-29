# How to Propose - Budget & Timeline Calculation Guide

**Purpose of this document:** Help you calculate realistic budget & timeline for your proposal.

---

## 🎯 Goal: Submit Honest, Competitive Proposal

We're looking for proposals that are:
1. ✅ **Realistic** - Based on actual effort, not wishful thinking
2. ✅ **Detailed** - Per-Epic breakdown, not just total number
3. ✅ **Honest** - If you can't do everything with Rp 4M, say so
4. ✅ **Competitive** - We'll compare all proposals

**Remember:** Budget & scope proposal is a **MAJOR factor** in candidate selection.

---

## 📊 Understanding Story Points

### What Are Story Points?

Story points = **relative complexity estimate**, not hours.

**Reference Scale:**
- **1 point** = Trivial (e.g., logout button)
- **2 points** = Simple form (e.g., login with email/password)
- **3 points** = Standard CRUD (e.g., create company with validation)
- **5 points** = Complex CRUD with relations (e.g., create project with contacts)
- **8 points** = Very complex (e.g., create project with multi-select)
- **13 points** = Extremely complex (e.g., create report with photos + GPS + auto-save)

### Story Points ≠ Hours

**Mapping depends on YOUR experience:**

| Experience Level | 1 Story Point | 13 Story Points |
|-----------------|---------------|-----------------|
| Expert (5+ years Flutter) | ~2-3 hours | ~2-3 days |
| Intermediate (2-3 years) | ~4-5 hours | ~4-5 days |
| Junior (< 2 years) | ~6-8 hours | ~1-1.5 weeks |

**Total MVP = 79 story points**

---

## 🧮 Step-by-Step Calculation

### Step 1: Calculate Your Daily Rate

**Method A: Hourly Rate**
```
Your Hourly Rate: Rp [X] / hour
Daily Rate (8 hours): Rp [X] × 8 = Rp [Y] / day
```

**Method B: Monthly Salary**
```
Desired Monthly Income: Rp [X] / month
Working Days per Month: ~20 days
Daily Rate: Rp [X] ÷ 20 = Rp [Y] / day
```

**Example:**
- Desired: Rp 10 juta/month
- Daily rate: Rp 10.000.000 ÷ 20 = **Rp 500.000/day**

### Step 2: Estimate Your Productivity (Story Points per Day)

Based on your experience with Flutter + offline-first + BLoC:

| Your Experience | Points/Day | Days for 79 Points |
|----------------|------------|--------------------|
| **Expert** (done offline-first before) | 1.5-2 pts/day | 40-53 days (8-10 weeks) |
| **Intermediate** (basic Flutter + some BLoC) | 1-1.5 pts/day | 53-79 days (10-16 weeks) |
| **Learning** (need to learn offline-first) | 0.5-1 pts/day | 79-158 days (16-32 weeks) |

**Be honest about your level!**

### Step 3: Calculate Per-Epic Effort

Review **USER_STORIES.md** and estimate days per Epic:

| Epic | Story Points | Your Estimated Days | Budget (days × rate) |
|------|-------------|---------------------|----------------------|
| Epic 1: Authentication | 5 pts | [X] days | Rp [Y] |
| Epic 2: Companies | 10 pts | [X] days | Rp [Y] |
| Epic 3: Contacts | 10 pts | [X] days | Rp [Y] |
| Epic 4: Projects | 14 pts | [X] days | Rp [Y] |
| Epic 5: Reports | 18 pts | [X] days | Rp [Y] |
| Epic 6: Dashboard | 10 pts | [X] days | Rp [Y] |
| Epic 7: Offline & Sync | 15 pts | [X] days | Rp [Y] |
| **Testing & Polish** | - | [X] days | Rp [Y] |
| **TOTAL** | **79 pts** | **[X] days** | **Rp [Y]** |

**Don't forget:**
- Testing time (add 20-30% for tests)
- Polish & bug fixes (add 10-15% buffer)
- Setup & deployment (2-3 days)

### Step 4: Add Buffer for Unknowns

**Rule of thumb:** Add 15-20% buffer

```
Base Estimate: Rp [X]
Buffer (20%): Rp [X] × 0.20 = Rp [Y]
Total Quote: Rp [X] + Rp [Y] = Rp [Z]
```

**Why buffer?**
- Edge cases you didn't anticipate
- RLS policy debugging time
- Integration issues
- Client feedback & iterations

---

## 💡 Example Calculations

### Example 1: Expert Developer

**Background:**
- 6 years Flutter experience
- Built 2 offline-first apps before
- Familiar with Clean Architecture + BLoC
- Desired income: Rp 12 juta/month

**Calculation:**
```
Daily Rate: Rp 12.000.000 ÷ 20 = Rp 600.000/day
Productivity: 1.5 story points/day
Days Needed: 79 pts ÷ 1.5 = 53 days
Testing & Polish: +15% = 8 days
Total: 61 days (12 weeks)

Base Budget: 61 days × Rp 600.000 = Rp 36.600.000
Buffer (15%): Rp 36.600.000 × 0.15 = Rp 5.490.000
Total Quote: Rp 42.090.000 (round to Rp 42 juta)
```

**Proposal for Rp 4 juta:**
"Dengan Rp 4 juta, saya bisa deliver **Epic 1-2 (Authentication + Companies)** dalam 2-3 minggu. Untuk full MVP, realistic budget adalah **Rp 42 juta dengan 12 minggu**."

### Example 2: Intermediate Developer

**Background:**
- 3 years Flutter experience
- Never built offline-first, but understand concept
- Basic BLoC knowledge
- Desired income: Rp 8 juta/month

**Calculation:**
```
Daily Rate: Rp 8.000.000 ÷ 20 = Rp 400.000/day
Productivity: 1 story point/day (accounting for learning)
Days Needed: 79 pts ÷ 1 = 79 days
Testing & Polish: +25% = 20 days (more time for learning)
Total: 99 days (20 weeks)

Base Budget: 99 days × Rp 400.000 = Rp 39.600.000
Buffer (20%): Rp 39.600.000 × 0.20 = Rp 7.920.000
Total Quote: Rp 47.520.000 (round to Rp 48 juta)
```

**Proposal for Rp 4 juta:**
"Dengan Rp 4 juta, saya bisa deliver **Epic 1 (Authentication)** + sebagian **Epic 2 (Companies - Create & View only)** dalam 3-4 minggu. Untuk full MVP dengan offline-first yang saya perlu pelajari, realistic budget adalah **Rp 48 juta dengan 20 minggu**."

### Example 3: Competitive Proposal (Lower Rate, Faster)

**Background:**
- 4 years Flutter experience
- Built 1 offline-first app
- Expert in BLoC
- Lives in smaller city (lower cost of living)
- Desired income: Rp 6 juta/month

**Calculation:**
```
Daily Rate: Rp 6.000.000 ÷ 20 = Rp 300.000/day
Productivity: 1.2 story points/day
Days Needed: 79 pts ÷ 1.2 = 66 days
Testing & Polish: +20% = 13 days
Total: 79 days (16 weeks)

Base Budget: 79 days × Rp 300.000 = Rp 23.700.000
Buffer (15%): Rp 23.700.000 × 0.15 = Rp 3.555.000
Total Quote: Rp 27.255.000 (round to Rp 27 juta)
```

**Proposal for Rp 4 juta:**
"Dengan Rp 4 juta, saya bisa deliver **Epic 1-2-3 (Authentication + Companies + Contacts)** dalam 4-5 minggu. Untuk full MVP, realistic budget adalah **Rp 27 juta dengan 16 minggu**."

**Why this is competitive:**
- Lower daily rate (lower cost of living)
- Good productivity (experienced)
- Still realistic estimate

---

## ✅ What to Include in Your Proposal

### Required Information

**1. With Rp 4 Million Budget:**
```
Dengan Rp 4 juta, saya bisa deliver:
- Epic 1: Authentication (5 pts)
- Epic 2: Companies (10 pts)
- Total: 15 story points dalam 3-4 minggu

Deliverables:
✅ Login/logout functionality
✅ Companies CRUD (Create, View, Edit)
✅ Basic offline support untuk companies
✅ Unit tests untuk Epic 1-2
✅ flutter analyze clean
```

**2. Full MVP Quote:**
```
Untuk full MVP (79 story points, 19 user stories):

Budget: Rp [X] juta
Timeline: [Y] minggu

Breakdown per Epic:
- Epic 1 (Auth): Rp [X] - [Y] minggu
- Epic 2 (Companies): Rp [X] - [Y] minggu
- Epic 3 (Contacts): Rp [X] - [Y] minggu
- Epic 4 (Projects): Rp [X] - [Y] minggu
- Epic 5 (Reports): Rp [X] - [Y] minggu
- Epic 6 (Dashboard): Rp [X] - [Y] minggu
- Epic 7 (Offline & Sync): Rp [X] - [Y] minggu
- Testing & Polish: Rp [X] - [Y] minggu

Deliverables:
✅ All 19 user stories implemented
✅ Clean Architecture + BLoC
✅ >60% test coverage
✅ flutter analyze clean
✅ APK release signed
✅ 30 hari bug fix warranty
```

**3. Timeline Breakdown:**
```
Best case: [X] minggu (if everything goes smoothly)
Realistic case: [Y] minggu (accounting for normal issues)
Worst case: [Z] minggu (if major blockers encountered)
```

**4. Key Assumptions:**
```
Assumptions in my estimate:
1. Supabase project already setup (database + storage)
2. Design assets provided (or use Material Design 3 default)
3. RLS policies will be tested before development starts
4. Weekly check-ins untuk discuss blockers
5. No major scope changes during development
```

**5. Why I'm a Good Fit:**
```
Relevant Experience:
- [App Name] - Offline-first app dengan [X] users, di Play Store
- [App Name] - BLoC pattern implementation, [X] features
- [Specific technical achievement]

Why you should choose me:
- [Unique value proposition]
- [Portfolio quality indicator]
- [Communication/reliability indicator]
```

---

## 🚫 Common Mistakes to Avoid

### ❌ Mistake #1: Underestimating Complexity
```
"4 juta cukup untuk semua, 3 minggu selesai"
```
**Why wrong:** Offline-first + RLS + 79 points cannot be done in 3 weeks, even by experts.

### ❌ Mistake #2: Vague Estimate
```
"Tergantung kompleksitas, mungkin 2-6 bulan"
```
**Why wrong:** Too vague, shows you didn't review documentation carefully.

### ❌ Mistake #3: No Breakdown
```
"Total budget Rp 20 juta, 10 minggu"
```
**Why wrong:** No per-Epic breakdown = cannot evaluate if you understand scope.

### ❌ Mistake #4: Ignoring RLS Complexity
```
No mention of RLS policies atau testing strategy
```
**Why wrong:** RLS is CRITICAL untuk data isolation. Must address this.

### ❌ Mistake #5: Copy-Paste Generic Proposal
```
Generic proposal tanpa reference spesifik ke project ini
```
**Why wrong:** Shows you didn't read documentation.

---

## ✅ Good Proposal Examples

### Example A: Partial Delivery (Honest)

```
Dengan budget Rp 4 juta:

Saya bisa deliver Epic 1-3 (Authentication, Companies, Contacts) = 25 story points.
Timeline: 4-5 minggu.

Detail deliverables:
✅ Login/logout dengan Supabase Auth
✅ Companies CRUD (Create, View, Edit)
✅ Contacts CRUD (Create, View, Edit)
✅ Basic offline support untuk companies & contacts
✅ RLS policies tested dengan 2 test users
✅ Unit tests untuk business logic (>80%)
✅ Flutter analyze clean
✅ APK debug untuk testing

Yang TIDAK termasuk:
❌ Projects, Reports, Dashboard, Full offline sync
❌ Photo upload management
❌ Background sync dengan retry logic

Untuk full MVP (79 points):
Budget realistis: Rp 18 juta
Timeline: 11-12 minggu

Saya recommend kita start dengan Rp 4 juta untuk Epic 1-3 dulu.
Jika hasil memuaskan, lanjut dengan Epic 4-7.
```

**Why this is good:**
- Honest about limitations
- Clear deliverables
- Clear exclusions
- Proposes phased approach

### Example B: Full MVP (Competitive)

```
Review dokumentasi: Saya sudah review semua 19 user stories, database schema dengan 31 RLS policies, dan sync strategy.

Dengan budget Rp 4 juta:
Tidak realistis untuk full MVP. Saya bisa deliver Epic 1-2 (15 points) dalam 3 minggu.

Untuk full MVP (79 story points):

Budget: Rp 24 juta
Timeline: 12 minggu (best case 10 minggu, worst case 14 minggu)

Breakdown:
- Week 1-2: Epic 1 (Auth) + Setup - Rp 2 juta
- Week 3-4: Epic 2 (Companies) - Rp 2 juta
- Week 5-6: Epic 3 (Contacts) + Epic 6 (Dashboard) - Rp 3 juta
- Week 7-8: Epic 4 (Projects) - Rp 3 juta
- Week 9-10: Epic 5 (Reports) + Photos - Rp 6 juta
- Week 11: Epic 7 (Offline & Sync) - Rp 5 juta
- Week 12: Testing & Polish - Rp 3 juta

Key technical approach:
1. RLS policies akan saya test dengan Supabase CLI + 2 test users sebelum coding
2. Offline sync: Implement transaction log table untuk rollback capability
3. Photo upload: Per-photo status tracking dengan retry logic (max 3x)
4. Clean Architecture: lib/features/[feature]/data|domain|presentation
5. Testing: Unit tests untuk BLoC + UseCases, widget tests untuk UI

Experience relevan:
- [App X]: Offline-first dengan 50K+ downloads (Play Store link)
- [App Y]: Supabase + RLS implementation untuk multi-tenant SaaS

Saya bisa start minggu depan, available full-time untuk project ini.
```

**Why this is good:**
- Shows deep understanding of technical challenges
- Per-week breakdown with budget
- Addresses key technical points (RLS testing, sync strategy)
- Relevant portfolio
- Clear availability

---

## 🎯 Final Checklist Before Submitting

- [ ] I've read ALL documentation (3-5 hours)
- [ ] I've calculated my realistic daily rate
- [ ] I've estimated effort per Epic
- [ ] I've included 15-20% buffer
- [ ] I've provided per-Epic breakdown
- [ ] I've addressed RLS testing strategy
- [ ] I've addressed offline-first sync complexity
- [ ] I've provided timeline (best/realistic/worst)
- [ ] I've included relevant portfolio links
- [ ] I've been HONEST about limitations

---

## 📋 Google Form Submission

After calculation, fill the Google Form with:
1. ✅ Portfolio links (Play Store)
2. ✅ Experience summary
3. ✅ Rp 4M proposal (what you can deliver)
4. ✅ Full MVP quote (with breakdown)
5. ✅ Timeline estimate (best/realistic/worst)
6. ✅ Technical approach (RLS testing, sync strategy)
7. ✅ Why you're a good fit

**Form Link:** [GOOGLE_FORM_LINK_AKAN_DI-INSERT]

---

**Good luck!** 🚀

Remember: **Honest + detailed + competitive** proposals win. We're evaluating value-for-money, not just lowest price.
