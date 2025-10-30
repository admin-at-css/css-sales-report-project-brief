# Dokumentasi Database Schema
## CSS Sales Report App - Supabase & Drift

← [Sebelumnya: User Stories](./USER_STORIES.md)

---

**Project:** CSS Sales Report MVP
**Database:** Supabase (PostgreSQL) + Drift (SQLite untuk offline)
**Tanggal:** Oktober 2025

---

## 📋 Ringkasan

Dokumen ini berisi schema database lengkap untuk:
1. **Supabase (Server)** - PostgreSQL cloud database
2. **Drift (Local)** - SQLite offline database di perangkat mobile

---

## 🗄️ Supabase Database Schema

### 8 Tables Utama (Normalized Design)

---

### 1. user_profiles

Informasi user yang diperluas dari auth.users

| Field Name | Type | Description |
|------------|------|-------------|
| `id` | UUID | Primary key, referensi ke auth.users |
| `full_name` | VARCHAR(255) | Nama lengkap user |
| `role` | VARCHAR(20) | 'sales_rep' atau 'manager' |
| `is_active` | BOOLEAN | Status aktif user |
| `created_at` | TIMESTAMPTZ | Timestamp creation |
| `updated_at` | TIMESTAMPTZ | Timestamp last update (auto-updated via trigger) |

**Constraints:**
- PRIMARY KEY: `id`
- FOREIGN KEY: `id` → `auth.users(id)` ON DELETE CASCADE
- CHECK: `role IN ('sales_rep', 'manager')`

**Indexes:**
- `idx_user_profiles_role`
- `idx_user_profiles_is_active`

**RLS Policies:**
- Users dapat membaca profile mereka sendiri
- Managers dapat membaca semua profiles
- Users dapat update profile sendiri (kecuali role)

---

### 2. companies

Perusahaan dan organisasi customer

| Field Name | Type | Description |
|------------|------|-------------|
| `id` | UUID | Primary key |
| `company_name` | VARCHAR(255) | Nama perusahaan (UNIQUE setelah normalisasi, case-insensitive) |
| `created_by` | UUID | Foreign key ke auth.users (sales rep yang create) |
| `created_at` | TIMESTAMPTZ | Timestamp creation |
| `updated_at` | TIMESTAMPTZ | Timestamp last update (auto-updated via trigger) |
| `deleted_at` | TIMESTAMPTZ | Soft delete timestamp (NULL = active) |
| `search_vector` | TSVECTOR | Full-text search vector (auto-generated via trigger) |

**Constraints:**
- PRIMARY KEY: `id`
- FOREIGN KEY: `created_by` → `auth.users(id)` ON DELETE RESTRICT
- UNIQUE INDEX: `normalize_company_name(company_name)` WHERE `deleted_at IS NULL`
  - Mencegah duplikat: "PT ABC", "PT ABC ", "pt abc" dianggap sama
- TRIGGER: Auto-update `search_vector` pada INSERT/UPDATE
- TRIGGER: Mencegah duplikat nama perusahaan (case-insensitive, trim whitespace)

**Indexes:**
- `idx_companies_name` - Untuk autocomplete
- `idx_companies_created_by` - Filter berdasarkan creator
- `idx_companies_search` (GIN) - Full-text search
- `idx_companies_deleted_at` (partial) - Hanya record yang belum di-delete

**RLS Policies:**
- Users melihat perusahaan mereka sendiri, managers melihat semua
- Users dapat membuat perusahaan
- Users dapat update/delete perusahaan mereka sendiri

**Catatan:**
- Nama perusahaan bersifat unique (dinormalisasi: lowercase, trimmed, spaces collapsed)
- Soft delete: Set `deleted_at` alih-alih hard delete
- Full-text search diaktifkan melalui `search_vector`

---

### 3. contacts

Contact persons di dalam perusahaan customer

| Field Name | Type | Description |
|------------|------|-------------|
| `id` | UUID | Primary key |
| `company_id` | UUID | Foreign key ke companies |
| `contact_name` | VARCHAR(255) | Nama lengkap contact person |
| `position` | VARCHAR(255) | Jabatan/posisi |
| `phone` | VARCHAR(50) | Nomor telepon |
| `email` | VARCHAR(255) | Email address (optional) |
| `is_active` | BOOLEAN | Status aktif (false jika person sudah resign dari company) |
| `created_by` | UUID | Foreign key ke auth.users |
| `created_at` | TIMESTAMPTZ | Timestamp creation |
| `updated_at` | TIMESTAMPTZ | Timestamp last update (auto-updated via trigger) |
| `deleted_at` | TIMESTAMPTZ | Soft delete timestamp |

**Constraints:**
- PRIMARY KEY: `id`
- FOREIGN KEY: `company_id` → `companies(id)` ON DELETE RESTRICT
- FOREIGN KEY: `created_by` → `auth.users(id)` ON DELETE RESTRICT

**Indexes:**
- `idx_contacts_company_id` - Pencarian cepat berdasarkan company
- `idx_contacts_name` - Pencarian berdasarkan nama
- `idx_contacts_phone` - Pencarian berdasarkan telepon
- `idx_contacts_is_active` - Filter contacts yang aktif
- `idx_contacts_deleted_at` (partial)

**RLS Policies:**
- Users melihat contacts dari perusahaan mereka, managers melihat semua
- Users dapat membuat contacts untuk perusahaan mereka
- Users dapat update/delete contacts yang mereka buat

---

### 4. projects

Sales projects dan opportunities

| Field Name | Type | Description |
|------------|------|-------------|
| `id` | UUID | Primary key |
| `company_id` | UUID | Foreign key ke companies |
| `primary_contact_id` | UUID | Foreign key ke contacts (PIC utama project) |
| `project_name` | VARCHAR(255) | Nama project |
| `project_type` | VARCHAR(50) | Tipe project: 'Architectural' \| 'Industrial' \| 'Infrastructure' \| 'Marine' |
| `project_segmentation` | VARCHAR(50)[] | PostgreSQL array: 'Decorative' \| 'Protective Coating' \| 'Floor Coating' \| 'Marine Coating' |
| `project_source` | VARCHAR(50) | Sumber lead: 'Canvassing' \| 'Referral from Customer' \| 'Referral from Principal' \| 'Website Inquiry' \| 'Exhibition' \| 'Repeat Customer' |
| `estimated_value` | NUMERIC(15,2) | Nilai estimasi project (Rupiah, presisi tepat) |
| `actual_value` | NUMERIC(15,2) | Nilai aktual jika project menang (nullable) |
| `status` | VARCHAR(20) | Status: 'active' \| 'won' \| 'lost' \| 'on_hold' (default: 'active') |
| `expected_close_date` | DATE | Expected closing date (nullable) |
| `won_date` | DATE | Tanggal project menang (nullable) |
| `lost_reason` | TEXT | Alasan jika project kalah (nullable) |
| `created_by` | UUID | Foreign key ke auth.users (sales rep owner) |
| `created_at` | TIMESTAMPTZ | Timestamp creation |
| `updated_at` | TIMESTAMPTZ | Timestamp last update (auto-updated via trigger) |
| `deleted_at` | TIMESTAMPTZ | Soft delete timestamp |

**Constraints:**
- PRIMARY KEY: `id`
- FOREIGN KEY: `company_id` → `companies(id)` ON DELETE RESTRICT
- FOREIGN KEY: `primary_contact_id` → `contacts(id)` ON DELETE RESTRICT
- FOREIGN KEY: `created_by` → `auth.users(id)` ON DELETE RESTRICT
- CHECK: `project_type IN ('Architectural', 'Industrial', 'Infrastructure', 'Marine')`
- CHECK: `status IN ('active', 'won', 'lost', 'on_hold')`
- CHECK: `primary_contact_id` harus dari company yang sama dengan `company_id`
- TRIGGER: Validasi `project_segmentation` array tidak boleh kosong (minimum 1 diperlukan)
- TRIGGER: Auto-log perubahan nilai ke `project_value_log` ketika `estimated_value` berubah

**Indexes:**
- `idx_projects_company_id`
- `idx_projects_primary_contact_id`
- `idx_projects_created_by`
- `idx_projects_status`
- `idx_projects_project_type`
- `idx_projects_segmentation` (GIN) - Untuk pencarian array
- `idx_projects_expected_close_date`
- `idx_projects_deleted_at` (partial)

**RLS Policies:**
- Users melihat projects mereka sendiri, managers melihat semua
- Users dapat membuat projects untuk perusahaan mereka
- Users dapat update/delete projects mereka sendiri

**Catatan:**
- `project_segmentation` adalah PostgreSQL array (multi-select field)
- Perubahan nilai otomatis di-log ke table `project_value_log`
- Primary contact harus dari company yang sama (enforced by CHECK constraint)

---

### 5. reports

Visit reports yang dibuat oleh sales reps

| Field Name | Type | Description |
|------------|------|-------------|
| `id` | UUID | Primary key |
| `project_id` | UUID | Foreign key ke projects |
| `report_type` | VARCHAR(50) | Tipe kunjungan: 'Initial Visit' \| 'Follow-up Meeting' \| 'Technical Presentation' \| 'Price Quotation' \| 'Closing Visit' \| 'After Sales Visit' |
| `visit_date` | DATE | Tanggal kunjungan (validated: tidak boleh tanggal masa depan via trigger) |
| `notes` | TEXT | Catatan meeting (nullable) |
| `next_action` | TEXT | Follow-up action (nullable) |
| `outcome` | VARCHAR(20) | Hasil meeting: 'positive' \| 'neutral' \| 'negative' (nullable) |
| `latitude` | NUMERIC(10,8) | GPS latitude dengan presisi tepat (nullable - GPS bersifat OPTIONAL) |
| `longitude` | NUMERIC(11,8) | GPS longitude dengan presisi tepat (nullable - GPS bersifat OPTIONAL) |
| `location_address` | TEXT | Alamat yang dapat dibaca manusia dari reverse geocoding (nullable) |
| `created_by` | UUID | Foreign key ke auth.users (sales rep yang visit) |
| `created_at` | TIMESTAMPTZ | Timestamp creation |
| `updated_at` | TIMESTAMPTZ | Timestamp last update (auto-updated via trigger) |
| `deleted_at` | TIMESTAMPTZ | Soft delete timestamp |

**Constraints:**
- PRIMARY KEY: `id`
- FOREIGN KEY: `project_id` → `projects(id)` ON DELETE RESTRICT
- FOREIGN KEY: `created_by` → `auth.users(id)` ON DELETE RESTRICT
- CHECK: `report_type IN ('Initial Visit', 'Follow-up Meeting', 'Technical Presentation', 'Price Quotation', 'Closing Visit', 'After Sales Visit')`
- CHECK: `outcome IN ('positive', 'neutral', 'negative')` OR NULL
- TRIGGER: Validasi `visit_date` tidak boleh di masa depan

**Indexes:**
- `idx_reports_project_id`
- `idx_reports_created_by`
- `idx_reports_visit_date` (DESC)
- `idx_reports_report_type`
- `idx_reports_outcome`
- `idx_reports_created_at` (DESC)
- `idx_reports_user_date` (composite: created_by, visit_date)
- `idx_reports_deleted_at` (partial)

**RLS Policies:**
- Users melihat reports mereka sendiri, managers melihat semua
- Users dapat membuat reports untuk projects mereka
- Users dapat update/delete reports mereka sendiri

**Catatan Penting:**
- Koordinat GPS bersifat **OPTIONAL** - reports dapat disimpan tanpa lokasi
- `visit_date` tidak boleh di masa depan (divalidasi di database level)
- Sync status (`is_synced`, `synced_at`) dikelola di local Drift database saja, TIDAK di Supabase

---

### 6. report_attendees

Junction table untuk multi-attendee meetings (many-to-many relationship)

| Field Name | Type | Description |
|------------|------|-------------|
| `id` | UUID | Primary key |
| `report_id` | UUID | Foreign key ke reports |
| `contact_id` | UUID | Foreign key ke contacts |
| `is_primary` | BOOLEAN | Flag untuk primary contact (tepat SATU per report wajib ada) |
| `created_at` | TIMESTAMPTZ | Timestamp creation |

**Constraints:**
- PRIMARY KEY: `id`
- FOREIGN KEY: `report_id` → `reports(id)` ON DELETE CASCADE
- FOREIGN KEY: `contact_id` → `contacts(id)` ON DELETE RESTRICT
- UNIQUE: `(report_id, contact_id)` - Mencegah duplikat attendees di report yang sama
- UNIQUE INDEX (partial): Hanya satu `is_primary = true` per `report_id`

**Indexes:**
- `idx_report_attendees_report_id`
- `idx_report_attendees_contact_id`
- `idx_report_attendees_is_primary` (partial)
- `unique_primary_attendee` (partial unique index)

**RLS Policies:**
- Access mengikuti parent report
- Users dapat menambah/update/delete attendees di reports mereka

**Catatan:**
- Mendukung multiple contacts per meeting
- Tepat satu contact harus ditandai sebagai primary per report
- Mencegah contact yang sama ditambahkan dua kali ke report yang sama

---

### 7. project_value_log

Audit trail untuk perubahan estimated_value project (read-only)

| Field Name | Type | Description |
|------------|------|-------------|
| `id` | UUID | Primary key |
| `project_id` | UUID | Foreign key ke projects |
| `old_value` | NUMERIC(15,2) | Nilai estimated sebelumnya |
| `new_value` | NUMERIC(15,2) | Nilai estimated baru |
| `updated_by` | UUID | Foreign key ke auth.users |
| `updated_at` | TIMESTAMPTZ | Timestamp perubahan |

**Constraints:**
- PRIMARY KEY: `id`
- FOREIGN KEY: `project_id` → `projects(id)` ON DELETE CASCADE
- FOREIGN KEY: `updated_by` → `auth.users(id)` ON DELETE RESTRICT
- **AUTO-POPULATED:** Database trigger otomatis insert ketika `estimated_value` berubah di table `projects`

**Indexes:**
- `idx_project_value_log_project_id`
- `idx_project_value_log_updated_at` (DESC)
- `idx_project_value_log_updated_by`

**RLS Policies:**
- Audit log read-only
- Users melihat history nilai untuk projects mereka, managers melihat semua
- Hanya system trigger yang dapat insert (tidak ada manual INSERT oleh users)

**Catatan:**
- Otomatis di-populate oleh database trigger - TIDAK perlu manual inserts
- Menyediakan audit trail lengkap untuk perubahan nilai project sepanjang waktu
- Immutable setelah creation (tidak ada UPDATE atau DELETE yang diperbolehkan)

---

### 8. attachments

Photo attachments untuk visit reports

| Field Name | Type | Description |
|------------|------|-------------|
| `id` | UUID | Primary key |
| `report_id` | UUID | Foreign key ke reports |
| `file_path` | TEXT | Storage bucket path: `{user_id}/{year}/{month}/{report_id}/{photo_id}.jpg` |
| `file_size` | INTEGER | Ukuran file dalam bytes |
| `mime_type` | VARCHAR(100) | Image type (misal, 'image/jpeg', 'image/png') |
| `latitude` | NUMERIC(10,8) | GPS latitude dimana foto diambil (nullable) |
| `longitude` | NUMERIC(11,8) | GPS longitude dimana foto diambil (nullable) |
| `taken_at` | TIMESTAMPTZ | Timestamp ketika foto ditangkap |
| `created_at` | TIMESTAMPTZ | Timestamp ketika record dibuat |

**Constraints:**
- PRIMARY KEY: `id`
- FOREIGN KEY: `report_id` → `reports(id)` ON DELETE CASCADE

**Indexes:**
- `idx_attachments_report_id`
- `idx_attachments_taken_at` (DESC)
- `idx_attachments_mime_type`

**RLS Policies:**
- Access mengikuti parent report
- Users dapat menambah/delete photos di reports mereka
- Tidak ada update policy (attachments bersifat immutable setelah creation)

**Catatan:**
- Photos disimpan dalam struktur hierarkis: `{user_id}/{year}/{month}/{report_id}/{photo_id}.jpg`
- Sync status dikelola di local Drift database saja, TIDAK di Supabase
- Photo metadata (GPS, timestamp) ditangkap dari device pada saat foto diambil
- Immutable setelah upload - tidak ada modifikasi yang diperbolehkan

---

## 🔒 Security Model (Row Level Security)

Semua table memiliki RLS enabled. Security model:

| User Role | Access Rights |
|-----------|---------------|
| **Sales Rep** | • Read/Write data mereka sendiri saja<br>• Tidak dapat melihat data sales reps lain<br>• Dapat membuat companies, contacts, projects, reports |
| **Manager** | • Read SEMUA data team<br>• Tidak dapat edit/delete data orang lain<br>• Akses read-only untuk monitoring<br>• Dapat export reports |

**Helper Functions:**
- `get_user_role()` - Mengembalikan role user saat ini
- `is_manager()` - Mengembalikan true jika user saat ini adalah manager
- `is_not_deleted(deleted_at)` - Filter records yang soft-deleted

**Total: 31 RLS policies di semua tables**

---

## 📝 Database Migration Files

Semua setup database tersedia di `/supabase/migrations/`:

1. **001_create_tables.sql** - Membuat semua 8 tables dengan constraints
2. **002_add_indexes.sql** - Membuat 38 performance indexes
3. **003_add_constraints_and_triggers.sql** - Menambahkan triggers dan business rules
4. **004_enable_rls_and_policies.sql** - Enable RLS + membuat 31 security policies
5. **005_seed_test_data.sql** - Seed test data (DEV/STAGING saja!)

**Eksekusi dengan urutan yang tepat: 001 → 002 → 003 → 004 → (005 optional)**

---

## 🗂️ Local Database (Drift/SQLite)

Mobile app menggunakan Drift (SQLite) untuk offline storage. Local schema mencerminkan Supabase dengan tambahan sync state fields.

**Dokumentasi lengkap Drift schema:** Lihat `DRIFT_SCHEMA.md`

### Perbedaan Utama dari Supabase:

| Feature | Supabase (Server) | Drift (Local) |
|---------|-------------------|---------------|
| Sync fields | ❌ Tidak ada `is_synced`, `synced_at` | ✅ Memiliki `is_synced`, `synced_at`, `local_updated_at` |
| Draft support | ❌ Tidak ada `is_draft` | ✅ Memiliki `is_draft` untuk reports |
| UUID storage | UUID native type | TEXT (limitasi SQLite) |
| Decimal storage | NUMERIC | REAL (limitasi SQLite) |
| Arrays | PostgreSQL array | JSON string (misal, `["Decorative","Floor Coating"]`) |

**Strategi Sync:**
- Local database memiliki flag boolean `is_synced`
- `is_synced = false` → Pending upload ke Supabase
- `is_synced = true` → Sudah sync, read-only
- Conflict resolution: Last-Write-Wins (untuk MVP)

---

## 🎯 Keputusan Design Utama

### 1. Normalized Structure
- Companies dipisah dari contacts untuk integritas data
- Many-to-many relationships melalui junction tables
- Foreign key constraints yang tepat

### 2. Soft Delete
- Field `deleted_at` di tables utama
- Kapabilitas data recovery
- Filters mengecualikan soft-deleted secara default

### 3. Audit Trail
- `project_value_log` melacak semua perubahan nilai
- Auto-populated oleh triggers
- Audit records yang immutable

### 4. Performance
- 38 indexes untuk query yang cepat
- GIN indexes untuk full-text search dan array fields
- Partial indexes untuk soft-deleted records

### 5. Security
- Row Level Security di semua tables
- Sales reps terisolasi satu sama lain
- Managers memiliki akses read-only team

### 6. Offline-First
- Sync status dikelola di local DB saja
- Server memiliki data model yang bersih
- Separation of concerns yang tepat

---

## ✅ Database Validation Checklist

Setelah menjalankan migrations, verifikasi:

- [ ] Duplikat nama company ditolak oleh database
- [ ] Sales rep A tidak dapat membaca data sales rep B (test RLS)
- [ ] Manager dapat membaca semua data team
- [ ] Perubahan nilai project otomatis di-log
- [ ] Visit date di masa depan ditolak
- [ ] GPS bersifat optional (dapat menyimpan report tanpa koordinat)
- [ ] Soft delete berfungsi (deleted_at di-set alih-alih hard delete)
- [ ] Semua 38 indexes ada (cek dengan `\di` di psql)
- [ ] Full-text search berfungsi di nama company
- [ ] Photo upload menggunakan hierarchical path

---

## 📚 Referensi

- **Supabase Documentation:** https://supabase.com/docs
- **Drift Documentation:** https://drift.simonbinder.eu
- **PostgreSQL Arrays:** https://www.postgresql.org/docs/current/arrays.html
- **Row Level Security:** https://supabase.com/docs/guides/auth/row-level-security

---

**Untuk schema lengkap Drift (local SQLite), lihat: `DRIFT_SCHEMA.md`**

**Untuk migration SQL files, lihat: `/supabase/migrations/`**
# Drift (SQLite) Schema - Production Ready
## CSS Sales Report - Local Database

**Versi:** 3.0 (Production Ready)
**Tanggal:** Oktober 2025
**Tujuan:** Offline-first data storage untuk mobile app dengan jaminan data integrity

---

## 📋 Overview

Schema ini meng-mirror struktur Supabase (PostgreSQL) dengan fields tambahan untuk sync state management dan offline operation. Semua critical edge cases telah ditangani untuk production deployment.

### Key Principles:
1. **UUID Primary Keys** - UUIDs yang sama dengan Supabase untuk seamless sync
2. **Sync State Tracking** - Fields untuk `is_synced`, `synced_at`, transaction logging
3. **Draft Support** - Flag `is_draft` untuk incomplete reports
4. **Conflict Detection** - Timestamps untuk mendeteksi sync conflicts
5. **Offline-First** - Semua CRUD operations bekerja tanpa network
6. **Data Integrity** - INTEGER untuk currency (tidak ada floating-point precision loss)

### Critical Changes dari v2.0:
- ✅ **Currency fields diubah dari REAL ke INTEGER** (menyimpan cents/sen)
- ✅ **Ditambahkan table `sync_transactions`** untuk sync rollback capability
- ✅ **Enhanced attachment sync tracking** dengan status dan retry logic
- ✅ **Ditambahkan helper functions** untuk currency conversion

---

## 🗂️ Tables

### 1. LocalUserProfiles

Meng-mirror `user_profiles` dari Supabase.

```dart
@DataClassName('LocalUserProfile')
class LocalUserProfiles extends Table {
  TextColumn get id => text()(); // UUID as TEXT
  TextColumn get fullName => text().named('full_name')();
  TextColumn get role => text()(); // 'sales_rep' or 'manager'
  BoolColumn get isActive => boolean().named('is_active').withDefault(const Constant(true))();
  DateTimeColumn get createdAt => dateTime().named('created_at').withDefault(currentDateAndTime)();
  DateTimeColumn get updatedAt => dateTime().named('updated_at').withDefault(currentDateAndTime)();

  @override
  Set<Column> get primaryKey => {id};
}
```

**Catatan:**
- User profiles di-sync saat login
- Read-only setelah initial sync (users tidak mengedit profiles mereka sendiri)

---

### 2. LocalCompanies

Meng-mirror `companies` dengan sync state.

```dart
@DataClassName('LocalCompany')
class LocalCompanies extends Table {
  TextColumn get id => text()();
  TextColumn get companyName => text().named('company_name')();
  TextColumn get createdBy => text().named('created_by')();
  DateTimeColumn get createdAt => dateTime().named('created_at').withDefault(currentDateAndTime)();
  DateTimeColumn get updatedAt => dateTime().named('updated_at').withDefault(currentDateAndTime)();
  DateTimeColumn get deletedAt => dateTime().named('deleted_at').nullable()();

  // Sync state fields (LOCAL ONLY - tidak di Supabase)
  BoolColumn get isSynced => boolean().named('is_synced').withDefault(const Constant(false))();
  DateTimeColumn get syncedAt => dateTime().named('synced_at').nullable()();
  DateTimeColumn get localUpdatedAt => dateTime().named('local_updated_at').withDefault(currentDateAndTime)();

  @override
  Set<Column> get primaryKey => {id};
}
```

**Logic Sync:**
- `is_synced = false` → Pending upload ke Supabase
- `is_synced = true` → Berhasil di-sync
- `synced_at` → Last successful sync timestamp
- `local_updated_at` → Last local modification (untuk conflict detection)

---

### 3. LocalContacts

Meng-mirror `contacts` dengan sync state.

```dart
@DataClassName('LocalContact')
class LocalContacts extends Table {
  TextColumn get id => text()();
  TextColumn get companyId => text().named('company_id')();
  TextColumn get contactName => text().named('contact_name')();
  TextColumn get position => text()();
  TextColumn get phone => text()();
  TextColumn get email => text().nullable()();
  BoolColumn get isActive => boolean().named('is_active').withDefault(const Constant(true))();
  TextColumn get createdBy => text().named('created_by')();
  DateTimeColumn get createdAt => dateTime().named('created_at').withDefault(currentDateAndTime)();
  DateTimeColumn get updatedAt => dateTime().named('updated_at').withDefault(currentDateAndTime)();
  DateTimeColumn get deletedAt => dateTime().named('deleted_at').nullable()();

  // Sync state fields (LOCAL ONLY)
  BoolColumn get isSynced => boolean().named('is_synced').withDefault(const Constant(false))();
  DateTimeColumn get syncedAt => dateTime().named('synced_at').nullable()();
  DateTimeColumn get localUpdatedAt => dateTime().named('local_updated_at').withDefault(currentDateAndTime)();

  @override
  Set<Column> get primaryKey => {id};
}
```

---

### 4. LocalProjects

Meng-mirror `projects` dengan sync state. **CRITICAL: Currency disimpan sebagai INTEGER.**

```dart
@DataClassName('LocalProject')
class LocalProjects extends Table {
  TextColumn get id => text()();
  TextColumn get companyId => text().named('company_id')();
  TextColumn get primaryContactId => text().named('primary_contact_id').nullable()();
  TextColumn get projectName => text().named('project_name')();
  TextColumn get projectType => text().named('project_type')();
  TextColumn get projectSegmentation => text().named('project_segmentation')(); // JSON array as TEXT
  TextColumn get projectSource => text().named('project_source')();

  // ⚠️ CRITICAL CHANGE: Currency sebagai INTEGER (cents/sen)
  // Rp 50.000.000,00 → 5.000.000.000 (5 miliar cents)
  IntColumn get estimatedValueCents => integer().named('estimated_value_cents')();
  IntColumn get actualValueCents => integer().named('actual_value_cents').nullable()();

  TextColumn get status => text().withDefault(const Constant('active'))();
  DateTimeColumn get expectedCloseDate => dateTime().named('expected_close_date').nullable()();
  DateTimeColumn get wonDate => dateTime().named('won_date').nullable()();
  TextColumn get lostReason => text().named('lost_reason').nullable()();
  TextColumn get createdBy => text().named('created_by')();
  DateTimeColumn get createdAt => dateTime().named('created_at').withDefault(currentDateAndTime)();
  DateTimeColumn get updatedAt => dateTime().named('updated_at').withDefault(currentDateAndTime)();
  DateTimeColumn get deletedAt => dateTime().named('deleted_at').nullable()();

  // Sync state fields (LOCAL ONLY)
  BoolColumn get isSynced => boolean().named('is_synced').withDefault(const Constant(false))();
  DateTimeColumn get syncedAt => dateTime().named('synced_at').nullable()();
  DateTimeColumn get localUpdatedAt => dateTime().named('local_updated_at').withDefault(currentDateAndTime)();

  @override
  Set<Column> get primaryKey => {id};
}
```

**Currency Helper Functions:**

```dart
// Extension untuk LocalProject
extension LocalProjectExtension on LocalProject {
  // Convert INTEGER cents ke double currency
  double get estimatedValue => estimatedValueCents / 100.0;
  double? get actualValue => actualValueCents != null ? actualValueCents! / 100.0 : null;

  // Buat project dengan currency
  static LocalProject createWithCurrency({
    required String id,
    required String companyId,
    required String projectName,
    required double estimatedValue, // User menyediakan: 50000000.00
    // ... fields lain
  }) {
    return LocalProject(
      id: id,
      companyId: companyId,
      projectName: projectName,
      estimatedValueCents: (estimatedValue * 100).round(), // Disimpan: 5000000000
      // ... fields lain
    );
  }
}

// Helper functions
int currencyToInt(double value) => (value * 100).round();
double intToCurrency(int cents) => cents / 100.0;

// Contoh penggunaan:
// User input: Rp 50.000.000,00
// Simpan sebagai: 5.000.000.000 cents
// Tampilkan sebagai: Rp 50.000.000,00
```

**Mengapa INTEGER untuk Currency:**
- Floating-point (REAL/double) memiliki precision loss: `50000000.00` → `49999999.98` setelah operasi
- Data finansial HARUS exact
- INTEGER menyimpan nilai exact: `5000000000` cents = `50000000.00` rupiah
- Lihat Edge Cases doc line 417-445 untuk penjelasan detail

**Catatan:**
- `project_segmentation` disimpan sebagai JSON array string di SQLite
- Contoh: `'["Decorative","Floor Coating"]'`
- Convert to/from `List<String>` di Dart menggunakan `jsonEncode`/`jsonDecode`

---

### 5. LocalReports

Meng-mirror `reports` dengan sync state dan draft support.

```dart
@DataClassName('LocalReport')
class LocalReports extends Table {
  TextColumn get id => text()();
  TextColumn get projectId => text().named('project_id')();
  TextColumn get reportType => text().named('report_type')();
  DateTimeColumn get visitDate => dateTime().named('visit_date')();
  TextColumn get notes => text().nullable()();
  TextColumn get nextAction => text().named('next_action').nullable()();
  TextColumn get outcome => text().nullable()();
  RealColumn get latitude => real().nullable()();
  RealColumn get longitude => real().nullable()();
  TextColumn get locationAddress => text().named('location_address').nullable()();
  TextColumn get createdBy => text().named('created_by')();
  DateTimeColumn get createdAt => dateTime().named('created_at').withDefault(currentDateAndTime)();
  DateTimeColumn get updatedAt => dateTime().named('updated_at').withDefault(currentDateAndTime)();
  DateTimeColumn get deletedAt => dateTime().named('deleted_at').nullable()();

  // Sync state fields (LOCAL ONLY)
  BoolColumn get isSynced => boolean().named('is_synced').withDefault(const Constant(false))();
  DateTimeColumn get syncedAt => dateTime().named('synced_at').nullable()();
  DateTimeColumn get localUpdatedAt => dateTime().named('local_updated_at').withDefault(currentDateAndTime)();

  // Draft support (LOCAL ONLY)
  BoolColumn get isDraft => boolean().named('is_draft').withDefault(const Constant(false))();

  @override
  Set<Column> get primaryKey => {id};
}
```

**Behavior Draft:**
- `is_draft = true` → Auto-saved setiap 30 detik, incomplete report
- `is_draft = false` → Finalized report, siap untuk sync
- Draft reports TIDAK di-sync sampai finalized
- Mencegah data loss saat app crashes (US-5.1 requirement)

**Catatan:** Koordinat GPS (latitude/longitude) tetap sebagai REAL - presisi tidak critical untuk location

---

### 6. LocalReportAttendees

Meng-mirror junction table `report_attendees`.

```dart
@DataClassName('LocalReportAttendee')
class LocalReportAttendees extends Table {
  TextColumn get id => text()();
  TextColumn get reportId => text().named('report_id')();
  TextColumn get contactId => text().named('contact_id')();
  BoolColumn get isPrimary => boolean().named('is_primary').withDefault(const Constant(false))();
  DateTimeColumn get createdAt => dateTime().named('created_at').withDefault(currentDateAndTime)();

  // Sync state fields (LOCAL ONLY)
  BoolColumn get isSynced => boolean().named('is_synced').withDefault(const Constant(false))();

  @override
  Set<Column> get primaryKey => {id};
}
```

---

### 7. LocalProjectValueLog

Meng-mirror audit trail `project_value_log`. **CRITICAL: Currency sebagai INTEGER.**

```dart
@DataClassName('LocalProjectValueLog')
class LocalProjectValueLog extends Table {
  TextColumn get id => text()();
  TextColumn get projectId => text().named('project_id')();

  // ⚠️ CRITICAL CHANGE: Currency sebagai INTEGER (cents/sen)
  IntColumn get oldValueCents => integer().named('old_value_cents')();
  IntColumn get newValueCents => integer().named('new_value_cents')();

  TextColumn get updatedBy => text().named('updated_by')();
  DateTimeColumn get updatedAt => dateTime().named('updated_at').withDefault(currentDateAndTime)();

  // Sync state fields (LOCAL ONLY)
  BoolColumn get isSynced => boolean().named('is_synced').withDefault(const Constant(false))();

  @override
  Set<Column> get primaryKey => {id};
}
```

**Catatan:**
- Perubahan value di-log secara lokal dan di-sync ke server
- Server trigger juga membuat logs untuk server-side changes
- INTEGER memastikan audit trail yang exact

---

### 8. LocalAttachments

Meng-mirror `attachments` dengan **enhanced sync tracking** untuk production.

```dart
@DataClassName('LocalAttachment')
class LocalAttachments extends Table {
  TextColumn get id => text()();
  TextColumn get reportId => text().named('report_id')();
  TextColumn get localFilePath => text().named('local_file_path')(); // Path di device
  TextColumn get remoteFilePath => text().named('remote_file_path').nullable()(); // Supabase Storage path
  IntColumn get fileSize => integer().named('file_size')();
  TextColumn get mimeType => text().named('mime_type')();
  RealColumn get latitude => real().nullable()();
  RealColumn get longitude => real().nullable()();
  DateTimeColumn get takenAt => dateTime().named('taken_at').withDefault(currentDateAndTime)();
  DateTimeColumn get createdAt => dateTime().named('created_at').withDefault(currentDateAndTime)();

  // ⚠️ ENHANCED: Per-photo sync tracking (Production requirement)
  TextColumn get syncStatus => text().named('sync_status').withDefault(const Constant('pending'))();
  // Values: 'pending', 'uploading', 'success', 'failed'

  DateTimeColumn get syncedAt => dateTime().named('synced_at').nullable()();
  IntColumn get uploadAttempts => integer().named('upload_attempts').withDefault(const Constant(0))();
  TextColumn get uploadError => text().named('upload_error').nullable()();
  DateTimeColumn get lastAttemptAt => dateTime().named('last_attempt_at').nullable()();

  @override
  Set<Column> get primaryKey => {id};
}
```

**Photo Sync States:**

| Status | Arti | Action |
|--------|------|--------|
| `pending` | Belum di-upload | Queue untuk upload |
| `uploading` | Sedang uploading | Tampilkan progress |
| `success` | Berhasil di-upload | Tampilkan dari server |
| `failed` | Upload gagal (cek error) | Izinkan retry |

**Logic Photo Sync:**
- `local_file_path` → Device path (contoh: `/data/.../photo_123.jpg`)
- `remote_file_path` → Supabase Storage path (contoh: `user_id/2025/10/report_id/photo_id.jpg`)
- `upload_attempts` → Retry counter (max 3 attempts)
- `upload_error` → Last error message untuk debugging

**Mengapa Enhanced Tracking:**
- Mencegah photo loss (Edge Cases line 19-42)
- Per-photo retry (bukan all-or-nothing)
- Visibilitas user ke failed uploads
- Idempotent uploads (cek `remote_file_path` sebelum re-upload)

---

### 9. SyncTransactions (BARU - Production Critical)

**Table baru untuk sync rollback capability.** Menangani Edge Cases line 75-91.

```dart
@DataClassName('SyncTransaction')
class SyncTransactions extends Table {
  TextColumn get id => text()();
  TextColumn get entityType => text().named('entity_type')(); // 'company', 'contact', 'project', 'report', 'attachment'
  TextColumn get entityId => text().named('entity_id')();
  TextColumn get operation => text()(); // 'create', 'update', 'delete'
  TextColumn get status => text().withDefault(const Constant('pending'))();
  // Values: 'pending', 'syncing', 'success', 'failed', 'rolled_back'

  TextColumn get errorMessage => text().named('error_message').nullable()();
  IntColumn get attemptCount => integer().named('attempt_count').withDefault(const Constant(0))();
  DateTimeColumn get createdAt => dateTime().named('created_at').withDefault(currentDateAndTime)();
  DateTimeColumn get lastAttemptAt => dateTime().named('last_attempt_at').nullable()();
  DateTimeColumn get completedAt => dateTime().named('completed_at').nullable()();

  @override
  Set<Column> get primaryKey => {id};
}
```

**Tujuan:**
- Melacak setiap sync operation untuk rollback capability
- Jika app crash mid-sync, resume dari last successful transaction
- Mencegah data stuck in limbo (MVP critical requirement)
- Retry logic dengan exponential backoff

**Contoh Transaction Log:**
```
ID: tx-001 | Type: report | ID: rpt-123 | Status: success
ID: tx-002 | Type: attachment | ID: att-456 | Status: failed (Network timeout)
ID: tx-003 | Type: attachment | ID: att-457 | Status: pending (menunggu retry)
```

---

## 🔄 Sync Strategy

### Sync States

| State | `is_synced` | `synced_at` | Arti |
|-------|-------------|-------------|------|
| **Pending** | `false` | `NULL` | Dibuat secara lokal, belum di-upload |
| **Synced** | `true` | Timestamp | Berhasil di-upload ke server |
| **Failed** | `false` | `NULL` | Upload dicoba tetapi gagal (cek sync_transactions) |

### Sync Order (Critical untuk Referential Integrity)

**HARUS sync dalam urutan ini untuk menghindari foreign key violations:**

1. ✅ **Companies** (tanpa dependencies)
2. ✅ **Contacts** (depends on companies)
3. ✅ **Projects** (depends on companies + contacts)
4. ✅ **Reports** (depends on projects)
5. ✅ **Report Attendees** (depends on reports + contacts)
6. ✅ **Project Value Log** (depends on projects)
7. ✅ **Attachments** (depends on reports, upload files ke Storage)

**Batch Sync dengan Transaction Safety:**

```dart
Future<void> syncAll() async {
  final txId = Uuid().v4();

  try {
    // Buat batch transaction
    await db.into(db.syncTransactions).insert(
      SyncTransaction(
        id: txId,
        entityType: 'batch',
        entityId: 'all',
        status: 'syncing',
      )
    );

    // Sync dalam urutan
    await syncCompanies();
    await syncContacts();
    await syncProjects();
    await syncReports();
    await syncReportAttendees();
    await syncProjectValueLog();
    await syncAttachments();

    // Tandai success
    await db.update(db.syncTransactions)
      ..where((tbl) => tbl.id.equals(txId))
      ..write(SyncTransactionsCompanion(
        status: Value('success'),
        completedAt: Value(DateTime.now()),
      ));

  } catch (e) {
    // Tandai failed, akan retry
    await db.update(db.syncTransactions)
      ..where((tbl) => tbl.id.equals(txId))
      ..write(SyncTransactionsCompanion(
        status: Value('failed'),
        errorMessage: Value(e.toString()),
      ));

    rethrow;
  }
}
```

### Conflict Resolution

Cek `local_updated_at` vs server `updated_at`:

```dart
if (serverUpdatedAt > localUpdatedAt) {
  // Server memiliki data yang lebih baru → Overwrite local (server menang)
  await localRepo.update(serverData);
} else if (localUpdatedAt > serverUpdatedAt) {
  // Local memiliki data yang lebih baru → Upload ke server (local menang)
  await supabaseClient.update(localData);
} else {
  // Same timestamp → Sudah sync, skip
}
```

**Untuk MVP:** Gunakan strategi **Last-Write-Wins** (sederhana, dapat diterima untuk tim kecil).
**Untuk Phase 2:** Implementasi conflict UI untuk user memilih versi.

---

## 📝 Example Queries (Diperbarui untuk Production)

### Dapatkan Semua Pending Sync Reports

```dart
Future<List<LocalReport>> getPendingSyncReports() {
  return (select(localReports)
    ..where((tbl) => tbl.isSynced.equals(false))
    ..where((tbl) => tbl.isDraft.equals(false))
    ..orderBy([(tbl) => OrderingTerm.asc(tbl.createdAt)]))
    .get();
}
```

### Dapatkan Report dengan Attendees (JOIN)

```dart
Future<List<ReportWithAttendees>> getReportWithAttendees(String reportId) {
  final query = select(localReports).join([
    leftOuterJoin(
      localReportAttendees,
      localReportAttendees.reportId.equalsExp(localReports.id),
    ),
    leftOuterJoin(
      localContacts,
      localContacts.id.equalsExp(localReportAttendees.contactId),
    ),
  ])..where(localReports.id.equals(reportId));

  return query.map((row) {
    return ReportWithAttendees(
      report: row.readTable(localReports),
      attendees: row.readTable(localContacts),
    );
  }).get();
}
```

### Dapatkan Active Projects User dengan Currency Display

```dart
Future<List<ProjectWithValue>> getUserActiveProjects(String userId) async {
  final projects = await (select(localProjects)
    ..where((tbl) => tbl.createdBy.equals(userId))
    ..where((tbl) => tbl.status.equals('active'))
    ..where((tbl) => tbl.deletedAt.isNull())
    ..orderBy([(tbl) => OrderingTerm.desc(tbl.updatedAt)]))
    .get();

  // Convert INTEGER cents ke currency untuk display
  return projects.map((p) => ProjectWithValue(
    project: p,
    estimatedValue: intToCurrency(p.estimatedValueCents),
    actualValue: p.actualValueCents != null ? intToCurrency(p.actualValueCents!) : null,
  )).toList();
}
```

### Count Pending Syncs berdasarkan Entity Type

```dart
Future<Map<String, int>> countPendingSyncs() async {
  final reports = await (selectOnly(localReports)
    ..addColumns([localReports.id.count()])
    ..where(localReports.isSynced.equals(false) & localReports.isDraft.equals(false)))
    .getSingle();

  final attachments = await (selectOnly(localAttachments)
    ..addColumns([localAttachments.id.count()])
    ..where(localAttachments.syncStatus.isIn(['pending', 'failed'])))
    .getSingle();

  return {
    'reports': reports.read(localReports.id.count()) ?? 0,
    'attachments': attachments.read(localAttachments.id.count()) ?? 0,
  };
}
```

### Dapatkan Failed Photo Uploads untuk Retry

```dart
Future<List<LocalAttachment>> getFailedPhotoUploads() {
  return (select(localAttachments)
    ..where((tbl) => tbl.syncStatus.equals('failed'))
    ..where((tbl) => tbl.uploadAttempts.isSmallerThanValue(3)) // Max 3 retries
    ..orderBy([(tbl) => OrderingTerm.asc(tbl.lastAttemptAt)]))
    .get();
}
```

### Dapatkan Sync Transactions untuk Resume Setelah Crash

```dart
Future<List<SyncTransaction>> getIncompleteSyncTransactions() {
  return (select(syncTransactions)
    ..where((tbl) => tbl.status.isIn(['pending', 'syncing', 'failed']))
    ..orderBy([(tbl) => OrderingTerm.asc(tbl.createdAt)]))
    .get();
}
```

### Query dengan Pagination (MVP Performance Requirement)

```dart
Future<List<LocalReport>> getReportsPaginated({
  required int page,
  required int pageSize,
  String? userId,
}) {
  final offset = page * pageSize;

  return (select(localReports)
    ..where((tbl) => userId != null ? tbl.createdBy.equals(userId) : tbl.id.isNotNull())
    ..orderBy([(tbl) => OrderingTerm.desc(tbl.visitDate)])
    ..limit(pageSize, offset: offset))
    .get();
}

// Penggunaan:
// Page 1: getReportsPaginated(page: 0, pageSize: 20) → reports 1-20
// Page 2: getReportsPaginated(page: 1, pageSize: 20) → reports 21-40
```

---

## 🗑️ Data Cleanup (Optional - Phase 2)

### Hapus Old Synced Data

Untuk devices dengan limited storage:

```dart
Future<void> cleanupOldSyncedData({int daysToKeep = 90}) async {
  final cutoffDate = DateTime.now().subtract(Duration(days: daysToKeep));

  // Hapus old synced reports (pertahankan yang baru)
  await (delete(localReports)
    ..where((tbl) =>
      tbl.isSynced.equals(true) &
      tbl.syncedAt.isSmallerThanValue(cutoffDate)
    )).go();

  // Hapus orphaned attachments (reports sudah dihapus)
  await (delete(localAttachments)
    ..where((tbl) => tbl.reportId.isNotIn(
      selectOnly(localReports)..addColumns([localReports.id])
    ))).go();
}
```

**Rekomendasi:** Hanya enable untuk users dengan storage issues. Kebanyakan users dapat menyimpan semua data secara lokal.

---

## 📊 Database Migrations

Drift mendukung schema versioning untuk app updates:

```dart
@DriftDatabase(tables: [
  LocalUserProfiles,
  LocalCompanies,
  LocalContacts,
  LocalProjects,
  LocalReports,
  LocalReportAttendees,
  LocalProjectValueLog,
  LocalAttachments,
  SyncTransactions, // BARU di v3.0
])
class AppDatabase extends _$AppDatabase {
  AppDatabase(QueryExecutor e) : super(e);

  @override
  int get schemaVersion => 3; // Increment untuk currency fix dan sync transactions

  @override
  MigrationStrategy get migration => MigrationStrategy(
    onCreate: (Migrator m) async {
      await m.createAll(); // Buat semua tables
    },
    onUpgrade: (Migrator m, int from, int to) async {
      // Migration v2 → v3: Fix currency precision
      if (from < 3) {
        // Tambahkan kolom INTEGER currency baru
        await m.addColumn(localProjects, localProjects.estimatedValueCents);
        await m.addColumn(localProjects, localProjects.actualValueCents);

        // Migrate data dari REAL ke INTEGER (kalikan 100)
        await customStatement('''
          UPDATE local_projects
          SET estimated_value_cents = CAST(estimated_value * 100 AS INTEGER),
              actual_value_cents = CAST(actual_value * 100 AS INTEGER)
        ''');

        // Drop old REAL columns (jika ada)
        // Catatan: SQLite tidak support DROP COLUMN, jadi gunakan recreate strategy

        // Tambahkan sync transactions table
        await m.createTable(syncTransactions);

        // Tambahkan sync status columns ke attachments
        await m.addColumn(localAttachments, localAttachments.syncStatus);
        await m.addColumn(localAttachments, localAttachments.lastAttemptAt);
      }
    },
  );
}
```

---

## 🧪 Testing Considerations

### Critical Test Scenarios

1. **Currency Precision Test**
   ```dart
   test('Currency survive multiple edits tanpa precision loss', () {
     final value = 50000000.00; // 50 juta rupiah
     final cents = currencyToInt(value); // 5.000.000.000 cents

     // Simulasikan 100 edits
     var currentCents = cents;
     for (var i = 0; i < 100; i++) {
       currentCents = (currentCents * 1.1).round(); // Increase 10%
       currentCents = (currentCents / 1.1).round(); // Decrease kembali
     }

     expect(currentCents, equals(cents)); // Harus exact
   });
   ```

2. **Sync Transaction Rollback Test**
   ```dart
   test('Sync rolls back saat partial failure', () async {
     // Buat 5 reports offline
     // Simulasikan network failure di report #3
     // Verify: reports 1-2 TIDAK ditandai sebagai synced
     // Verify: sync_transactions menampilkan failed state
     // Retry: Semua 5 reports sync dengan sukses
   });
   ```

3. **Photo Upload Retry Test**
   ```dart
   test('Failed photo uploads retry sampai 3 kali', () async {
     // Buat report dengan 10 photos
     // Simulasikan failure di photo #5 (3 kali)
     // Verify: Photos 1-4, 6-10 synced dengan sukses
     // Verify: Photo #5 ditandai sebagai 'failed' setelah 3 attempts
     // Verify: User dapat manually retry photo #5
   });
   ```

4. **Draft Auto-Save Recovery Test**
   ```dart
   test('Draft report recovered setelah app crash', () async {
     // Isi report form 50%
     // Auto-save triggers (30 detik)
     // Simulasikan app crash (kill process)
     // Restart app
     // Verify: Draft report ditampilkan dengan "Continue?" prompt
     // Verify: Semua filled data intact
   });
   ```

5. **Pagination Performance Test**
   ```dart
   test('Pagination load dengan cepat dengan 1000 reports', () async {
     // Insert 1000 reports ke DB
     // Ukur waktu untuk page 1 (20 reports)
     // Assert: Query time < 100ms
   });
   ```

---

## 📚 Referensi

- [Drift Documentation](https://drift.simonbinder.eu/)
- [Drift Migration Guide](https://drift.simonbinder.eu/docs/advanced-features/migrations/)
- [Supabase Flutter SDK](https://supabase.com/docs/reference/dart/introduction)
- Edge Cases Document (line 417-445: Currency precision)
- Edge Cases Document (line 75-91: Sync transaction log)

---

## 🔍 Schema Verification Checklist

Sebelum deployment, verify:

- [ ] Semua currency fields menggunakan INTEGER (estimatedValueCents, actualValueCents, oldValueCents, newValueCents)
- [ ] Helper functions untuk currency conversion telah ditest
- [ ] SyncTransactions table dibuat dengan proper indexes
- [ ] Attachment sync_status enum values validated
- [ ] Migration script dari v2 ke v3 ditest di staging data
- [ ] Semua example queries ditest dengan production-scale data (1000+ records)
- [ ] Foreign key relationships cocok dengan Supabase schema secara exact
- [ ] Pagination queries mengembalikan correct page sizes
- [ ] Draft auto-save menulis ke DB dengan sukses

---

**Terakhir Diperbarui:** Oktober 2025 (v3.0 - Production Ready)

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | Initial | Basic schema |
| 2.0 | Oct 2025 | Ditambahkan sync state fields |
| 3.0 | Oct 2025 | **Production Ready**: Currency sebagai INTEGER, sync transactions table, enhanced attachment tracking, pagination support |

---

## 📍 Navigasi

**Selesai membaca Database Schema?**
- ✅ [Lanjut ke: Sync Strategy →](./SYNC_STRATEGY.md)

**Atau kembali ke:**
- ← [User Stories](./USER_STORIES.md)
- 🏠 [README](./README.md)
