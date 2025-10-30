# User Personas - Siapa Yang Akan Menggunakan Aplikasi Ini?

← [Sebelumnya: DESIGNER_BRIEF](./DESIGNER_BRIEF.md)

---

> **📝 NOTE UNTUK PRODUCT OWNER:**
> Template ini akan diisi oleh Product Owner dengan detail user personas yang akurat.
> Designer akan mendesain berdasarkan personas yang Anda berikan di sini.

---

## 🎯 Primary User: Sales Representative

### Persona 1: [Nama Sales Rep]

**Demographics:**
- Usia: [e.g., 35 tahun]
- Jenis Kelamin: [e.g., Laki-laki]
- Pendidikan: [e.g., S1 Teknik]
- Lokasi: [e.g., Jakarta, Tangerang]

**Tech Profile:**
- Device: [e.g., Android mid-range, Xiaomi Redmi Note 11]
- Screen Size: [e.g., 6.4 inch]
- Apps yang sering digunakan: [e.g., WhatsApp, Instagram, Google Maps, Tokopedia]
- Tech comfort level: [e.g., Basic - bisa install apps, pakai social media, tapi tidak terlalu paham technical terms]

**Work Context:**
- Job role: [e.g., Sales Representative untuk produk industrial coatings]
- Typical day: [e.g., 2-3 customer visits per hari, 10-15 visits per minggu]
- Work location: [e.g., Di lapangan 70% waktu, di kantor 30%]
- Internet access: [e.g., Sering di area dengan sinyal lemah (pabrik, warehouse)]

**Pain Points dengan WhatsApp:**
1. [e.g., Lupa detail visit kalau tidak langsung tulis di WhatsApp]
2. [e.g., Ketik report panjang di HP itu melelahkan]
3. [e.g., Susah cari old reports (tenggelam di chat history)]
4. [e.g., Kalau HP ganti, data hilang]

**Goals:**
1. [e.g., Submit laporan dengan cepat (< 5 menit per report)]
2. [e.g., Tidak perlu khawatir data hilang]
3. [e.g., Bisa buat report meskipun tidak ada internet]
4. [e.g., Tidak perlu ingat-ingat detail (app save otomatis)]

**Behaviors:**
- [e.g., Biasanya buat report saat istirahat makan siang atau di mobil setelah visit]
- [e.g., Jarang baca instructions - prefer trial & error]
- [e.g., Sering terganggu (phone calls, chat) saat buat report]
- [e.g., Prefer foto daripada ketik notes panjang]

**Quote:**
> "[e.g., "Kalau bisa cepat dan mudah, kenapa harus ribet? WhatsApp aja udah ribet, apalagi app baru."]"

---

### Persona 2: [Nama Sales Rep 2]

**Demographics:**
- Usia: [Isi di sini]
- Jenis Kelamin: [Isi di sini]
- Pendidikan: [Isi di sini]
- Lokasi: [Isi di sini]

**Tech Profile:**
- Device: [Isi di sini]
- Screen Size: [Isi di sini]
- Apps yang sering digunakan: [Isi di sini]
- Tech comfort level: [Isi di sini]

**Work Context:**
- Job role: [Isi di sini]
- Typical day: [Isi di sini]
- Work location: [Isi di sini]
- Internet access: [Isi di sini]

**Pain Points:**
1. [Isi di sini]
2. [Isi di sini]
3. [Isi di sini]

**Goals:**
1. [Isi di sini]
2. [Isi di sini]
3. [Isi di sini]

**Behaviors:**
- [Isi di sini]
- [Isi di sini]

**Quote:**
> "[Isi di sini]"

---

## 🎯 Secondary User: Manager

### Persona 3: [Nama Manager]

**Demographics:**
- Usia: [e.g., 45 tahun]
- Jenis Kelamin: [e.g., Laki-laki]
- Pendidikan: [e.g., S1 Management]
- Lokasi: [e.g., Jakarta - kantor pusat]

**Tech Profile:**
- Device: [e.g., Android flagship (Samsung S23) + Laptop untuk dashboard]
- Screen Size: [e.g., 6.8 inch]
- Apps yang sering digunakan: [e.g., Gmail, Google Sheets, WhatsApp Web, LinkedIn]
- Tech comfort level: [e.g., Intermediate - comfortable dengan Excel, dashboards, CRM systems]

**Work Context:**
- Job role: [e.g., Sales Manager - mengawasi 2 sales reps (akan expand ke 5-10)]
- Typical day: [e.g., 80% di kantor, 20% di lapangan. Review reports pagi hari, follow-up dengan sales reps via chat]
- Work location: [e.g., Office-based]
- Internet access: [e.g., Selalu online (WiFi kantor + 4G)]

**Pain Points dengan WhatsApp:**
1. [e.g., Harus scroll banyak chat untuk cari reports]
2. [e.g., Data tidak terstruktur - susah untuk analysis]
3. [e.g., Copy-paste dari WhatsApp ke Excel memakan 5+ jam per minggu]
4. [e.g., Tidak bisa real-time track team activities]
5. [e.g., Sulit forecast sales pipeline (data berantakan)]

**Goals:**
1. [e.g., Real-time visibility ke semua team activities]
2. [e.g., Filter/search reports by company, date, sales rep]
3. [e.g., No manual data entry (data langsung masuk system)]
4. [e.g., Bisa analyze historical data untuk insights]

**Behaviors:**
- [e.g., Check reports setiap pagi (8-9 AM)]
- [e.g., Prefer dashboard view (overview) sebelum drill-down ke details]
- [e.g., Sering filter by date range (e.g., "Laporan minggu ini")]
- [e.g., Tidak perlu create/edit - cukup view (read-only)]

**Quote:**
> "[e.g., "Saya butuh visibility. Kalau saya tidak tahu apa yang terjadi di lapangan, saya tidak bisa make decisions."]"

---

## 📊 Persona Summary (Design Implications)

### For Sales Reps:
**Design untuk user yang:**
- ✅ **Low tech comfort** → Simple, clear UI. No jargon.
- ✅ **Time-constrained** → Fast to fill forms (< 5 min per report)
- ✅ **Interrupted frequently** → Auto-save drafts every 30s
- ✅ **Often offline** → Clear offline indicators, sync status
- ✅ **Visual learners** → Prefer photos over long text

**Design Decisions:**
- Large buttons (48dp minimum touch target)
- Clear labels ("Buat Laporan" not "Submit Report")
- Minimal steps (don't make 10-step wizard)
- Auto-save indicators ("Terakhir disimpan 2 menit yang lalu")
- Photo-first (easy to add photos, not mandatory to write long notes)

---

### For Manager:
**Design untuk user yang:**
- ✅ **Data-driven** → Show numbers, summaries, filters
- ✅ **Desktop-mindset** → Familiar dengan dashboards, tables
- ✅ **Always online** → No need for offline-first (nice-to-have, not critical)
- ✅ **Read-only** → No edit buttons, clear "view-only" mode

**Design Decisions:**
- Dashboard view (overview cards: "10 laporan minggu ini")
- Filter/search prominent (by sales rep, date range, company)
- List view with sorting (newest first, oldest first, by company)
- No "Create" FAB (manager cannot create reports)
- Clearly differentiated from Sales Rep view (different home screen)

---

## 🎯 Key Takeaways for Designer

### User Context:
1. **Sales Reps are NOT tech-savvy** → Don't assume they understand tech terms
2. **They're BUSY** → Every extra tap = frustration
3. **They're OFFLINE often** → Offline UX is critical, not optional
4. **They're using PHONE** → Design for one-handed use when possible

### Manager Context:
1. **Manager is office-based** → Desktop-like experience OK
2. **Manager wants OVERVIEW first** → Dashboard > individual reports
3. **Manager is READ-ONLY** → No edit/delete options
4. **Manager has TIME** → Can handle more dense information

---

## 📍 Navigasi

**Selesai memahami user personas?**

➡️ **[Lanjut ke: CSS Logo Guidelines.pdf →](./CSS%20Logo%20Guidelines.pdf)**
(Review brand identity: logo, colors, personality)

Setelah itu:
➡️ **[Lanjut ke: SCREEN_INVENTORY.md →](./SCREEN_INVENTORY.md)**

**Atau kembali ke:**
← [DESIGNER_BRIEF](./DESIGNER_BRIEF.md)
