# Project Overview - CSS Sales Report

**Purpose of this document:** Memberikan business context agar Anda understand **WHY** we're building this and **WHO** will use it.

---

## 🏢 About PT Cepat Service Station

**PT Cepat Service Station** adalah distributor cat industrial dan decorative coatings.

- **Industry:** Industrial coatings & protective solutions
- **Products:** Architectural, Industrial, Infrastructure, Marine coatings
- **Sales Model:** B2B, relationship-driven, long sales cycle (3-12 months)
- **Current Team Size:** 2 sales reps + 1 manager (MVP), akan expand ke 5-10 users (Phase 2)

---

## ❌ The Problem

### Current State: Reporting via WhatsApp

Sales reps currently membuat laporan kunjungan customer via WhatsApp group. **Problems:**

1. **Unstructured Data**
   - Format berantakan, banyak info yang ketinggalan
   - Sulit untuk search/filter old reports
   - Data tenggelam di chat history

2. **Manual Manager Work**
   - Manager harus copy-paste dari WhatsApp ke Excel
   - Memakan 5+ jam per minggu
   - Error-prone, data inconsistent

3. **Data Loss Risk**
   - Jika HP hilang, data ikut hilang (no backup)
   - Chat history terbatas

4. **Offline Problem**
   - Sales reps sering di area dengan sinyal jelek
   - Tidak bisa submit report kalau offline
   - Menunda reporting sampai balik ke kantor

5. **No Visibility**
   - Manager tidak bisa real-time track team activities
   - Sulit untuk forecast sales pipeline
   - Tidak ada history analysis

### Business Impact

- 🔴 Sales opportunities lost (lupa follow-up)
- 🔴 Inaccurate sales forecast
- 🔴 Manager wastes 5+ hours/week on manual data entry
- 🔴 Cannot analyze historical data for insights
- 🔴 Unprofessional image to enterprise clients

---

## ✅ The Solution

### Mobile App dengan Structured Reporting

**Core Value Proposition:**
"Sales reps bisa buat structured visit reports dalam 5 menit, bahkan tanpa internet. Manager dapat real-time visibility ke semua team activities."

### Key Features

**For Sales Reps:**
1. ✅ **Offline-First** - Kerja tanpa internet, auto-sync when online
2. ✅ **Quick Form** - Buat report dalam <5 menit
3. ✅ **Photo Capture** - Langsung ambil foto with GPS location
4. ✅ **Auto-Save Draft** - Tidak hilang jika app crash
5. ✅ **Structured Data** - Form fields ensure completeness

**For Manager:**
1. ✅ **Real-Time Dashboard** - Lihat semua team reports instantly
2. ✅ **Filter & Search** - Find reports by company, project, date
3. ✅ **Read-Only Access** - View all reports, cannot edit (audit trail)
4. ✅ **No Manual Data Entry** - Data langsung masuk ke sistem

**Technical Benefits:**
1. ✅ **Data Backup** - All reports saved in cloud (Supabase)
2. ✅ **Data Integrity** - RLS policies ensure data isolation
3. ✅ **Scalable** - Designed untuk expand from 2 to 50+ users

---

## 👥 Target Users

### Primary: Sales Representatives (2 users → 5-10 di Phase 2)

**Profile:**
- Age: 25-45 years old
- Tech Skill: Basic smartphone user (not tech-savvy)
- Work: Field sales, visit 5-10 customers per week
- Internet: Often in areas with poor/no signal
- Device: Personal Android phone (mid-range)

**Pain Points:**
- Typing long reports di WhatsApp itu melelahkan
- Sering lupa details visit kalau tidak langsung input
- Data hilang jika app crash
- Tidak bisa submit kalau offline

**Needs:**
- Bikin report cepat (tidak boleh bikin kerja lapangan jadi lama)
- Bisa offline (cannot rely on internet di customer site)
- UI simple (tidak perlu training ribet)
- Auto-save (data tidak ilang)

### Secondary: Manager (1 user)

**Profile:**
- Role: Sales Manager
- Responsibility: Track team activities, forecast pipeline
- Tech Skill: Can use web dashboard
- Work: Mostly in office, always online

**Pain Points:**
- Manual copy-paste reports dari WhatsApp ke Excel
- Sulit track which sales rep visited which customer
- Cannot forecast pipeline accurately
- Spend 5+ hours/week on data entry

**Needs:**
- Real-time visibility ke team reports
- Filter/search reports easily
- Track pipeline value
- Export data untuk analysis

---

## 🎯 Success Criteria

### MVP Success (Phase 1)

**Quantitative:**
- ✅ 90%+ daily active usage (2 sales reps using app every day)
- ✅ <5 minutes average report creation time
- ✅ 0% data loss (no reports lost)
- ✅ >95% sync success rate
- ✅ <5% app crash rate

**Qualitative:**
- ✅ Sales reps prefer app over WhatsApp (rating >4/5)
- ✅ Manager confirms data completeness & accuracy
- ✅ No critical bugs causing data loss
- ✅ Users find app easy to use

### Long-Term Success (Post-MVP)

- ✅ Expand to 5-10 users, churn rate <10%
- ✅ Manager uses app data for decision making
- ✅ Historical data provides business insights
- ✅ App becomes part of daily workflow

---

## 📊 Business Context

### Why This App is Critical

**Strategic Reasons:**

1. **Scalability:** WhatsApp system only works for 2-3 sales, cannot scale further
2. **Data-Driven Decisions:** Need structured data untuk forecast & analysis
3. **Professionalism:** Enterprise clients expect structured reporting
4. **Competitive Advantage:** Better follow-up = better relationships = more wins

### Current Sales Process

**Typical Sales Cycle:**
1. **Prospecting** - Find new customers via referrals, cold calls
2. **Visit** - Meet customer, understand project needs
3. **Proposal** - Submit quotation, negotiate price
4. **Follow-up** - Multiple visits over 3-12 months (long cycle!)
5. **Close** - Win or lost

**Report Purpose:**
- Track which customers visited
- Document project details (value, timeline, contacts)
- Record meeting notes & next actions
- Provide proof of work to manager
- Enable follow-up tracking

### Long-Term Vision

**Phase 1 (MVP):** 2 sales reps using app daily (10-12 weeks)
**Phase 2:** Enhanced features, expand to 5-10 users (5-7 weeks)
**Phase 3:** Scale to 10-50 users, advanced analytics (8-12 weeks)

**Long-Term Partnership:** Kami mencari developer untuk jangka panjang, bukan one-time job. Jika MVP sukses, there's Phase 2 & 3 work.

---

## 🤔 Key Questions for Developers

Understanding business context helps you make better technical decisions:

1. **Why offline-first?**
   - Sales reps often di tempat dengan sinyal jelek (pabrik, site konstruksi)
   - Cannot rely on internet availability

2. **Why RLS policies so strict?**
   - Sales rep A should NOT see sales rep B's data (competition & privacy)
   - Manager should see ALL team data (oversight)

3. **Why auto-save drafts?**
   - Sales reps sering distracted (phone calls, customer questions)
   - App might crash (Android fragmentation)
   - Cannot afford to lose report data

4. **Why photo upload important?**
   - Visual proof of visit (accountability)
   - Project documentation (site conditions, products used)
   - Customer trust (professional documentation)

5. **Why Clean Architecture required?**
   - This app will scale from 2 to 50+ users
   - Need maintainable codebase untuk long-term
   - Multiple developers might work on this

---

## 💡 What This Means for Your Proposal

**Understanding business context helps you:**

1. **Estimate effort accurately**
   - Offline-first is NOT just "add SQLite" - it's complex sync strategy
   - RLS policies need careful testing (data isolation is critical)
   - Photo management needs robust error handling

2. **Price competitively**
   - This is a REAL business with REAL users (not side project)
   - Long-term potential (Phase 2 & 3)
   - Quality matters more than speed

3. **Show you understand the domain**
   - Mention offline scenarios in your proposal
   - Acknowledge data security importance
   - Show you understand long sales cycle (follow-up tracking is critical)

---

## 🔗 Next Steps

After reading this overview:
1. ✅ Read **MVP_SCOPE.md** - Understand technical deliverables
2. ✅ Read **USER_STORIES.md** - Detailed requirements
3. ✅ Calculate realistic budget & timeline
4. ✅ Fill Google Form with thoughtful proposal

---

**Questions about business context?** Include them in your Google Form application.
