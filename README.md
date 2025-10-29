# CSS Sales Report - Project Brief for Developer Candidates

> **📋 Dokumentasi ini untuk membantu Anda mempersiapkan proposal budget & timeline yang akurat.**
>
> Silakan review semua dokumentasi di repo ini **SEBELUM** mengisi Google Form aplikasi.

---

## 🎯 Tentang Repo Ini

Repo ini berisi **dokumentasi lengkap** untuk project CSS Sales Report MVP yang akan Anda kerjakan jika terpilih sebagai developer.

**Tujuan repo ini:**
- Membantu Anda **memahami scope project** secara detail
- Memberikan **informasi teknis** yang cukup untuk menghitung effort
- Memperjelas **ekspektasi kualitas** code yang diharapkan
- Menunjukkan **long-term potential** (Phase 2 & 3)

**Yang TIDAK ada di repo ini:**
- Setup environment (akan diberikan setelah hired)
- Credential/access (akan diberikan setelah hired)
- Detailed implementation guide (untuk hired developer)

---

## 📱 Tentang Project

**CSS Sales Report** adalah mobile app (Android) untuk sales representatives PT Cepat Service Station. App ini menggantikan sistem reporting via WhatsApp dengan structured reporting system yang offline-capable.

### Quick Facts
- **Platform:** Android only (Flutter)
- **Users:** 2 sales reps + 1 manager (MVP phase)
- **Key Feature:** Offline-first architecture dengan sync ke cloud
- **MVP Scope:** 19 user stories, 79 story points
- **Estimated Timeline:** 10-12 minggu
- **Tech Stack:** Flutter + Supabase + Drift (SQLite)

### Business Context
PT Cepat Service Station adalah distributor cat industrial/decorative. Sales reps membuat 5-10 visit reports per minggu, currently via WhatsApp (unstructured, sulit tracking).

**Problem:** Manager sulit mendapat visibility ke sales activities, data tidak terstruktur, sulit untuk reporting & analysis.

**Solution:** Mobile app dengan structured forms, offline capability, real-time sync, dan manager dashboard.

---

## 📚 Apa yang Ada di Repo Ini?

### 📖 Wajib Baca (Untuk Memahami Scope & Kompleksitas)

1. **[MVP_SCOPE.md](./MVP_SCOPE.md)** - MVP deliverables, 79 story points breakdown
2. **[USER_STORIES.md](./USER_STORIES.md)** - 19 user stories lengkap dengan acceptance criteria
3. **[DATABASE_SCHEMA.md](./DATABASE_SCHEMA.md)** - 8 tables, 31 RLS policies, Drift + Supabase
4. **[SYNC_STRATEGY.md](./SYNC_STRATEGY.md)** - Offline-first sync logic, conflict resolution
5. **[TECHNICAL_CHALLENGES.md](./TECHNICAL_CHALLENGES.md)** - Kenapa project ini kompleks

### 📚 Bacaan Opsional (Deep Dive)

- **[PROJECT_OVERVIEW.md](./PROJECT_OVERVIEW.md)** - Business context lengkap
- **[ARCHITECTURE.md](./ARCHITECTURE.md)** - Clean Architecture, BLoC pattern, tech stack
- **[TECHNICAL_SPEC.md](./TECHNICAL_SPEC.md)** - Edge cases, non-functional requirements
- **[FUTURE_PHASES.md](./FUTURE_PHASES.md)** - Phase 2 & 3 scope (long-term potential)

---

## 🚀 Cara Menggunakan Repo Ini

**Ikuti alur dokumen ini secara berurutan (3-5 jam total):**

Setiap dokumen memiliki navigasi di bagian bawah untuk memandu Anda ke dokumen berikutnya.

### ✅ Mulai dari Sini:

**➡️ [Mulai: MVP Scope →](./MVP_SCOPE.md)**

**Urutan Bacaan (Wajib):**
1. [MVP_SCOPE.md](./MVP_SCOPE.md) - High-level deliverables
2. [USER_STORIES.md](./USER_STORIES.md) - Detail requirements per feature
3. [DATABASE_SCHEMA.md](./DATABASE_SCHEMA.md) - Database complexity
4. [SYNC_STRATEGY.md](./SYNC_STRATEGY.md) - Offline-first sync logic
5. [TECHNICAL_CHALLENGES.md](./TECHNICAL_CHALLENGES.md) - Why this is complex
6. [Google Form](https://forms.gle/ZjHRjnxggkZ8a6Qb8) - Submit proposal ✅

**Bacaan Opsional** tersedia di akhir alur dokumen (setelah TECHNICAL_CHALLENGES.md).

---

### 📋 Setelah Selesai Membaca

**Isi Google Form Application:**
- 📋 https://forms.gle/ZjHRjnxggkZ8a6Qb8

Yang perlu Anda submit:
- Portfolio (Play Store links)
- Experience summary
- **Budget proposal:** Apa yang bisa Anda deliver dengan Rp 4 juta?
- **Full MVP quote:** Berapa budget untuk 79 story points?
- Timeline estimate
- Technical questions

**Wait for Response:**
Kami akan review semua aplikasi setelah deadline dan contact top 3-5 kandidat via email untuk technical interview.

---

## ⚠️ Hal Penting Sebelum Apply

### Ini Bukan CRUD App Biasa

**Key Complexity:**
- ✅ **Offline-first architecture** - Bukan just "add SQLite"
- ✅ **Row-Level Security** - 31 RLS policies untuk data isolation
- ✅ **Background sync** - Retry logic, conflict resolution, transaction log
- ✅ **Photo management** - Compression, individual upload status tracking
- ✅ **Clean Architecture + BLoC** - Strict code organization required
- ✅ **Test coverage** - >60% overall, >80% business logic

Jika Anda hanya familiar dengan basic CRUD apps, project ini mungkin terlalu kompleks.

### Budget Reality Check

**Budget yang kami posting:** Rp 4.000.000

Ini adalah budget yang kami miliki untuk project ini. Kami understand bahwa ini adalah project kompleks dengan 79 story points. Dalam proposal Anda:
- **Opsi A:** Dengan Rp 4 juta, Epic mana yang bisa Anda deliver? (kami accept partial delivery)
- **Opsi B:** Berapa budget yang Anda butuhkan untuk full MVP? (kami will consider jika proposal Anda convincing)

**Honesty > Overpromising.** Kami prefer proposal yang realistis daripada overpromise yang tidak bisa di-deliver.

### Long-Term Opportunity

Jika MVP sukses, ada **Phase 2** (enhanced features) dan **Phase 3** (scale to 10-50 users). Kami mencari **long-term partner**, bukan one-time job.

---

## 📞 Next Steps

1. ✅ **Review semua dokumentasi di repo ini** (3-5 jam total) - Ikuti alur dari MVP_SCOPE.md
2. ✅ **Fill Google Form** dengan honest proposal
3. ⏳ **Wait for email** (top 3-5 kandidat akan di-contact untuk interview)

**Questions?** Silakan include di Google Form atau message via FastWork.

---

## 🔗 Links

- **Job Posting:** https://jobboard.fastwork.id/jobs/c4fd9ba2-9203-45bf-b275-764943c3c18c
- **Application Form:** https://forms.gle/ZjHRjnxggkZ8a6Qb8

---

**Good luck dengan proposal Anda!** 🚀

Kami appreciate Anda meluangkan waktu untuk review dokumentasi ini secara menyeluruh. Kandidat yang paling prepared biasanya yang paling sukses.

---

**Product Owner**
**PT Cepat Service Station**
