# System Architecture
## CSS Sales Report - Technical Design Document

**Versi:** 3.0 (Production Ready)
**Terakhir Diperbarui:** Oktober 2025
**Status:** Siap untuk Implementasi

---

## 📋 Overview

Dokumen ini menjelaskan arsitektur teknis untuk aplikasi mobile CSS Sales Report. Sistem ini mengikuti filosofi desain **offline-first, mobile-first** dengan cloud synchronization.

### High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                     MOBILE APPLICATION (Flutter)                 │
│                                                                  │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  Presentation Layer (UI)                                  │  │
│  │  • Material Design 3 widgets                             │  │
│  │  • Screens: Login, Dashboard, Reports, Companies, etc.   │  │
│  └────────────────────┬─────────────────────────────────────┘  │
│                       │                                          │
│  ┌────────────────────▼─────────────────────────────────────┐  │
│  │  Business Logic Layer (BLoC Pattern)                      │  │
│  │  • State Management (flutter_bloc)                        │  │
│  │  • Business rules, validation                             │  │
│  └────────────────────┬─────────────────────────────────────┘  │
│                       │                                          │
│  ┌────────────────────▼─────────────────────────────────────┐  │
│  │  Data Layer (Repositories)                                │  │
│  │  • Abstract interfaces                                    │  │
│  │  • Coordinate local + remote data sources                │  │
│  └───────────┬────────────────────────────────┬──────────────┘  │
│              │                                │                  │
│  ┌───────────▼────────────┐      ┌──────────▼──────────────┐  │
│  │  Local Data Source     │      │  Remote Data Source     │  │
│  │  (Drift - SQLite)      │      │  (Supabase Client)      │  │
│  │  • Offline storage     │      │  • REST API             │  │
│  │  • Fast queries        │      │  • Realtime (Phase 2)   │  │
│  └────────────────────────┘      └─────────────────────────┘  │
│              │                                │                  │
│  ┌───────────▼────────────────────────────────▼──────────────┐  │
│  │  Sync Engine (Background Service)                          │  │
│  │  • Connectivity monitoring                                 │  │
│  │  • Transaction management                                  │  │
│  │  • Conflict resolution                                     │  │
│  └────────────────────────────────────────────────────────────┘  │
└────────────────────────────┬─────────────────────────────────────┘
                             │
                          Internet
                             │
                             ▼
┌─────────────────────────────────────────────────────────────────┐
│                    CLOUD INFRASTRUCTURE (Supabase)               │
│                                                                  │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  PostgreSQL Database                                      │  │
│  │  • 8 tables dengan RLS policies                           │  │
│  │  • 38 performance indexes                                 │  │
│  │  • Triggers untuk audit logging                           │  │
│  └──────────────────────────────────────────────────────────┘  │
│                                                                  │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  Authentication Service                                    │  │
│  │  • Email/password auth                                     │  │
│  │  • JWT tokens                                              │  │
│  │  • Session management                                      │  │
│  └──────────────────────────────────────────────────────────┘  │
│                                                                  │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  Storage Service (S3-compatible)                          │  │
│  │  • Report photos                                           │  │
│  │  • Hierarchical structure                                  │  │
│  └──────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🏗️ Technology Stack

### Mobile Application

| Component | Technology | Version | Rationale |
|-----------|-----------|---------|-----------|
| **Framework** | Flutter | 3.24+ | Cross-platform (Android sekarang, iOS Phase 3), ekosistem matang, performa excellent |
| **Language** | Dart | 3.0+ | Type-safe, fitur modern, bagus untuk async operations |
| **State Management** | flutter_bloc | 8.1+ | Pattern yang terbukti, state yang predictable, testable, industry standard |
| **Local Database** | Drift (SQLite) | 2.18+ | Type-safe queries, reactive streams, integrasi Flutter excellent |
| **HTTP Client** | Supabase Flutter SDK | 2.5.6 | Official SDK, handle auth + database + storage dalam satu package |

### Backend & Infrastructure

| Component | Technology | Rationale |
|-----------|-----------|-----------|
| **Database** | Supabase (PostgreSQL 15) | Managed service, RLS built-in, generous free tier, scalable |
| **Authentication** | Supabase Auth | JWT-based, email/password, mudah untuk token refresh |
| **File Storage** | Supabase Storage | S3-compatible, terintegrasi dengan PostgreSQL RLS |
| **Hosting** | Supabase Cloud | Fully managed, global CDN, automatic backups |

### Supporting Libraries

```yaml
dependencies:
  # Core
  flutter_bloc: ^8.1.6           # State management
  equatable: ^2.0.5              # Value equality untuk BLoC states

  # Data
  supabase_flutter: ^2.5.6       # Backend SDK
  drift: ^2.18.0                 # Local database
  sqlite3_flutter_libs: ^0.5.20  # SQLite native bindings

  # Media
  image_picker: ^1.1.2           # Camera & gallery access
  flutter_image_compress: ^2.3.0 # Photo compression

  # Location
  geolocator: ^12.0.0            # GPS coordinates
  geocoding: ^3.0.0              # Reverse geocoding (address dari GPS)

  # Network
  connectivity_plus: ^6.0.3      # Network status monitoring

  # Utilities
  uuid: ^4.4.0                   # Generate UUIDs
  intl: ^0.19.0                  # Date/currency formatting
  shared_preferences: ^2.2.3     # Settings storage

  # Monitoring
  sentry_flutter: ^7.18.0        # Error tracking & crash reports

dev_dependencies:
  build_runner: ^2.4.9           # Code generation (Drift)
  drift_dev: ^2.18.0             # Drift code generator
  flutter_test: any              # Testing framework
  mocktail: ^1.0.3               # Mocking untuk tests
```

**Total Production Dependencies:** 16 packages (lean dan focused)

---

## 🎯 Architecture Patterns

### 1. Clean Architecture (Layered)

**Prinsip:**
- Separation of concerns
- Dependency inversion (inner layers tidak tahu tentang outer layers)
- Testability (business logic terisolasi dari UI dan data sources)

**Layer Breakdown:**

```
lib/
├── presentation/          # UI Layer
│   ├── screens/          # Full-screen pages
│   ├── widgets/          # Reusable components
│   └── bloc/             # BLoC state management
│
├── domain/               # Business Logic Layer
│   ├── models/          # Business entities (immutable)
│   ├── repositories/    # Abstract interfaces
│   └── use_cases/       # Business operations
│
└── data/                # Data Layer
    ├── models/          # Data transfer objects (DTOs)
    ├── data_sources/    # Local (Drift) & Remote (Supabase)
    ├── repositories/    # Concrete implementations
    └── database/        # Drift database definition

features/                # ATAU: Feature-first structure (alternatif)
├── auth/
│   ├── presentation/
│   ├── domain/
│   └── data/
├── reports/
│   ├── presentation/
│   ├── domain/
│   └── data/
└── companies/
    └── ...
```

**Rekomendasi:** Mulai dengan layered, migrate ke feature-first jika app berkembang melebihi 20 screens.

---

### 2. BLoC Pattern (State Management)

**Mengapa BLoC?**
- ✅ Perubahan state yang predictable
- ✅ Mudah untuk test (pure functions)
- ✅ Memisahkan business logic dari UI
- ✅ Reactive (Stream-based)
- ✅ Industry standard untuk Flutter

**BLoC Flow:**

```
┌──────────┐         ┌──────────┐         ┌──────────┐
│   UI     │ Events  │   BLoC   │  State  │   UI     │
│ (Widget) ├────────>│ (Logic)  ├────────>│ (Widget) │
└──────────┘         └────┬─────┘         └──────────┘
                          │
                          │ Calls
                          ▼
                    ┌──────────┐
                    │Repository│
                    └──────────┘
```

**Contoh: Report Creation**

```dart
// Event
class CreateReportButtonPressed extends ReportEvent {
  final ReportFormData formData;
  CreateReportButtonPressed(this.formData);
}

// State
class ReportCreating extends ReportState {}
class ReportCreated extends ReportState {
  final Report report;
  ReportCreated(this.report);
}
class ReportError extends ReportState {
  final String message;
  ReportError(this.message);
}

// BLoC
class ReportBloc extends Bloc<ReportEvent, ReportState> {
  final ReportRepository _repository;

  ReportBloc(this._repository) : super(ReportInitial()) {
    on<CreateReportButtonPressed>(_onCreateReport);
  }

  Future<void> _onCreateReport(
    CreateReportButtonPressed event,
    Emitter<ReportState> emit,
  ) async {
    emit(ReportCreating());

    try {
      final report = await _repository.createReport(event.formData);
      emit(ReportCreated(report));
    } catch (e) {
      emit(ReportError(e.toString()));
    }
  }
}

// UI Widget
BlocBuilder<ReportBloc, ReportState>(
  builder: (context, state) {
    if (state is ReportCreating) {
      return CircularProgressIndicator();
    } else if (state is ReportCreated) {
      return SuccessMessage();
    } else if (state is ReportError) {
      return ErrorMessage(state.message);
    }
    return ReportForm();
  },
)
```

---

### 3. Repository Pattern (Data Access)

**Tujuan:** Mengabstraksi detail data source dari business logic.

**Interface (Domain Layer):**
```dart
abstract class ReportRepository {
  Future<List<Report>> getReports({String? userId});
  Future<Report> getReportById(String id);
  Future<Report> createReport(ReportFormData data);
  Future<void> updateReport(Report report);
  Future<void> deleteReport(String id);
  Future<int> countPendingSync();
}
```

**Implementation (Data Layer):**
```dart
class ReportRepositoryImpl implements ReportRepository {
  final LocalDataSource _localDataSource;   // Drift
  final RemoteDataSource _remoteDataSource; // Supabase
  final SyncEngine _syncEngine;

  @override
  Future<Report> createReport(ReportFormData data) async {
    // 1. Create locally terlebih dahulu (offline-first)
    final report = await _localDataSource.insertReport(data);

    // 2. Trigger background sync
    _syncEngine.scheduleSyncIfOnline();

    // 3. Return segera (tidak menunggu sync)
    return report;
  }

  @override
  Future<List<Report>> getReports({String? userId}) async {
    // Selalu fetch dari local (cepat, bekerja offline)
    return await _localDataSource.getReports(userId: userId);
  }
}
```

---

## 🔄 Offline-First Strategy

### Core Principle

**"Local database adalah source of truth sampai di-sync"**

### Implementasi

**1. Write Path (Create/Update/Delete):**
```
User Action → Write ke Drift → Mark is_synced = false → Return success
                                      ↓ (background)
                            Sync engine upload ke Supabase
                                      ↓
                            Mark is_synced = true locally
```

**2. Read Path:**
```
User Request → Read dari Drift → Return segera
                     ↓ (background, jika online)
           Fetch latest dari Supabase → Update Drift
```

**3. Sync Engine (Background Service):**
- Triggered oleh: Connectivity change, user action, periodic timer (15min)
- Uploads: Companies → Contacts → Projects → Reports → Attachments
- Conflict Resolution: Last-Write-Wins (MVP), UI-prompted (Phase 2)
- Error Handling: Transaction log, retry dengan exponential backoff

**Untuk logic sync yang detail:** Lihat `database/Sync_Strategy.md`

---

## 🔐 Security Architecture

### Authentication Flow

```
1. User memasukkan email/password
   ↓
2. App memanggil Supabase Auth
   ↓
3. Supabase mengembalikan JWT access token + refresh token
   ↓
4. App menyimpan tokens di secure storage (flutter_secure_storage)
   ↓
5. Semua API calls menyertakan access token di Authorization header
   ↓
6. Jika 401 Unauthorized → Refresh token → Retry
   ↓
7. Jika refresh gagal → Logout user → Redirect ke login
```

### Row-Level Security (RLS)

**Ditegakkan di level database (PostgreSQL):**

```sql
-- Contoh RLS Policy: Sales reps hanya melihat reports mereka sendiri
CREATE POLICY "sales_rep_own_reports"
  ON reports
  FOR ALL
  USING (auth.uid() = created_by);

-- Contoh RLS Policy: Managers melihat semua team reports (read-only)
CREATE POLICY "manager_read_all_reports"
  ON reports
  FOR SELECT
  USING (
    auth.jwt() ->> 'role' = 'manager'
  );
```

**Keuntungan:**
- ✅ Security ditegakkan di database (tidak bisa di-bypass oleh API)
- ✅ Multi-tenant by default
- ✅ Automatic filtering (tidak perlu manual WHERE clauses)

**Untuk RLS policies lengkap:** Lihat `database/Supabase_Schema.md`

---

## 📊 Data Flow Diagrams

### Report Creation Flow

```
┌─────────┐
│  User   │
│ taps    │
│"Create  │
│Report"  │
└────┬────┘
     │
     ▼
┌─────────────────────────────┐
│ Presentation Layer          │
│ • ReportFormScreen tampil   │
│ • User mengisi: project,    │
│   type, notes, photos, GPS  │
│ • User tap "Save"           │
└────────────┬────────────────┘
             │ CreateReportButtonPressed event
             ▼
┌─────────────────────────────┐
│ Business Logic (BLoC)       │
│ • ReportBloc validasi data  │
│ • Panggil repository.create()│
└────────────┬────────────────┘
             │
             ▼
┌─────────────────────────────┐
│ Data Layer (Repository)     │
│ • Write ke Drift (local DB) │
│ • Set is_synced = false     │
│ • Trigger sync engine       │
└────────────┬────────────────┘
             │
             ├──> Return success segera
             │    (UI updates, user melanjutkan)
             │
             └──> Background sync dimulai
                  (upload ke Supabase saat online)
```

### Sync Flow (Background)

```
┌────────────────────────┐
│ Connectivity Monitor   │
│ mendeteksi internet    │
└──────────┬─────────────┘
           │ Online!
           ▼
┌────────────────────────┐
│ Sync Engine            │
│ 1. Query pending items │
│    (is_synced = false) │
└──────────┬─────────────┘
           │
           ▼
┌────────────────────────┐
│ Transaction Manager    │
│ 2. Buat tx log entry   │
│ 3. Upload ke Supabase  │
│ 4. Mark tx success     │
└──────────┬─────────────┘
           │
           ├──> Success: Mark is_synced = true
           │
           └──> Failure: Retry nanti (exponential backoff)
```

---

## 🗄️ Database Architecture

### Dual Database Design

**Local (Drift - SQLite):**
- Tujuan: Offline storage, fast queries
- Schema: 9 tables (8 mirrored + 1 sync_transactions)
- Storage: Device internal storage
- Lifespan: Persists sampai app uninstall
- **Referensi:** `database/Drift_Schema.md`

**Remote (Supabase - PostgreSQL):**
- Tujuan: Central source of truth, multi-user access
- Schema: 8 tables dengan RLS policies
- Storage: Cloud (Supabase managed)
- Lifespan: Permanent (dengan backups)
- **Referensi:** `database/Supabase_Schema.md`

### Data Type Considerations

**Critical: Currency sebagai INTEGER**

| Field | Type | Storage | Alasan |
|-------|------|---------|--------|
| `estimated_value_cents` | INTEGER | 5000000000 | Presisi exact (tidak ada floating-point errors) |
| `actual_value_cents` | INTEGER | 7500000000 | Data finansial HARUS exact |

**Mengapa bukan REAL/DOUBLE?**
- Floating-point arithmetic menimbulkan precision loss
- `50000000.00` → `49999999.98` setelah operasi
- Tidak dapat diterima untuk data finansial

**Helper functions:**
```dart
int currencyToInt(double value) => (value * 100).round();
double intToCurrency(int cents) => cents / 100.0;

// Contoh:
// User input: Rp 50.000.000,00
// Simpan sebagai: 5.000.000.000 cents
// Tampilkan sebagai: Rp 50.000.000,00
```

---

## 📱 Mobile App Architecture Details

### Screen Navigation

**Navigation Pattern:** Named routes dengan arguments

```dart
// Route definitions
class AppRoutes {
  static const login = '/login';
  static const home = '/home';
  static const reportsList = '/reports';
  static const reportCreate = '/reports/create';
  static const reportDetail = '/reports/:id';
  static const companies = '/companies';
  // ... dll
}

// Navigation
Navigator.pushNamed(
  context,
  AppRoutes.reportCreate,
  arguments: {'projectId': 'proj-123'},
);
```

### State Persistence

**User Session:**
- Simpan JWT tokens di `flutter_secure_storage`
- Auto-login saat app restart jika token valid
- Token refresh sebelum expiry

**Draft Reports:**
- Auto-save ke Drift setiap 30 detik
- Saat app restart: Cek reports dengan `is_draft = true`
- Prompt user: "Lanjutkan edit report dari 10 min yang lalu?"

**Settings:**
- Simpan di `shared_preferences`
- Settings: Auto-sync on/off, WiFi-only sync, photo quality

---

## ⚡ Performance Optimization

### Strategies

**1. Pagination (Wajib untuk MVP)**
```dart
// Load 20 items sekaligus
Future<List<Report>> getReportsPaginated(int page) async {
  final offset = page * 20;
  return (select(localReports)
    ..limit(20, offset: offset)
    ..orderBy([(t) => OrderingTerm.desc(t.visitDate)]))
    .get();
}
```

**2. Lazy Loading**
- Load list items terlebih dahulu (metadata only)
- Load full details saat di-tap (images, notes)

**3. Image Optimization**
- Compress photos ke <500KB sebelum save
- Gunakan thumbnails di lists (100x100px)
- Full resolution hanya di detail view

**4. Database Indexes**
```dart
// Drift indexes
@TableIndex(name: 'idx_reports_visit_date', columns: {#visitDate})
@TableIndex(name: 'idx_reports_created_by', columns: {#createdBy})
class LocalReports extends Table { ... }
```

**5. Background Processing**
- Sync engine berjalan di isolate (tidak memblokir UI)
- Photo compression di background thread
- Query berat menggunakan function `compute()`

**Performance Targets:**
- Load report list: <500ms
- Save report creation: <1s
- Sync 10 reports: <5s (di koneksi bagus)
- Query search: <300ms

---

## 🔍 Monitoring & Observability

### Error Tracking (Sentry)

**Apa yang dilacak:**
- App crashes
- Unhandled exceptions
- Network errors (dengan retry exhausted)
- Sync failures (dengan details)

**Apa yang TIDAK dilacak:**
- Expected errors (validation failures)
- Transient network issues (akan retry)
- User-canceled operations

**Implementasi:**
```dart
Future<void> main() async {
  await SentryFlutter.init(
    (options) {
      options.dsn = 'your-sentry-dsn';
      options.environment = 'production';
      options.tracesSampleRate = 0.2; // 20% dari transactions
    },
    appRunner: () => runApp(MyApp()),
  );
}
```

### Analytics (Optional - Phase 2)

**Apa yang dilacak:**
- Screen views
- Feature usage
- Report creation time
- Sync frequency

**Privacy:**
- Tidak ada PII (Personal Identifiable Information)
- Anonymized user IDs
- User consent diperlukan

---

## 🧪 Testing Strategy

### Test Pyramid

```
        E2E (10%)
      /         \
     /Integration\
    /    (30%)    \
   /───────────────\
  /   Unit (60%)   \
 /─────────────────\
```

**Unit Tests (60%):**
- BLoC logic
- Repository implementations
- Utility functions (currency conversion, validators)
- Model serialization/deserialization

**Integration Tests (30%):**
- Database operations (Drift)
- Sync engine logic
- Network layer (mock Supabase)

**E2E Tests (10%):**
- Critical user flows
  - Login → Create Report → Sync → View Report
  - Offline Create → Go Online → Verify Sync
  - RLS policy enforcement

**Untuk test plan yang detail:** Lihat `Test_Plan.md`

---

## 🚀 Deployment Architecture

### Build Variants

**Debug:**
- Development Supabase project
- Debug logging enabled
- Sentry disabled
- Fast builds (tanpa optimization)

**Release:**
- Production Supabase project
- Logging minimal
- Sentry enabled
- Optimized build (code obfuscation)

**Configuration:**
```dart
// lib/config/environment.dart
enum Environment { dev, staging, production }

class Config {
  static Environment get environment {
    const env = String.fromEnvironment('ENV', defaultValue: 'dev');
    return Environment.values.byName(env);
  }

  static String get supabaseUrl {
    switch (environment) {
      case Environment.dev:
        return 'https://dev.supabase.co';
      case Environment.staging:
        return 'https://staging.supabase.co';
      case Environment.production:
        return 'https://prod.supabase.co';
    }
  }
}
```

### CI/CD Pipeline (Phase 2)

**Automated Build:**
1. Git push ke branch `develop`
2. GitHub Actions triggers
3. Run tests (unit + integration)
4. Build APK jika tests pass
5. Upload ke internal distribution (Firebase App Distribution)

**Manual Release:**
1. Buat release branch
2. Update version number
3. Build signed APK
4. Manual testing
5. Upload ke Google Play (internal testing track)
6. Gradual rollout (10% → 50% → 100%)

---

## 🔧 Development Guidelines

### Code Style

**Ikuti:**
- [Effective Dart](https://dart.dev/guides/language/effective-dart)
- [Flutter style guide](https://flutter.dev/docs/development/platform-integration/platform-channels)

**Linting:**
```yaml
# analysis_options.yaml
include: package:flutter_lints/flutter.yaml

linter:
  rules:
    - prefer_const_constructors
    - avoid_print
    - always_use_package_imports
```

### Commit Messages

**Format:** `<type>(<scope>): <subject>`

**Types:** feat, fix, refactor, test, docs, chore

**Contoh:**
- `feat(reports): add draft auto-save every 30s`
- `fix(sync): retry failed photo uploads`
- `refactor(database): extract sync logic to separate service`

### Branch Strategy

- `main`: Production-ready code
- `develop`: Integration branch untuk features
- `feature/*`: Individual features
- `hotfix/*`: Critical production fixes

---

## 📚 Referensi

### Dokumentasi Internal
- `PRD.md` - Product requirements dan business context
- `Functional_Spec.md` - User stories (22 stories, 8 epics)
- `Technical_Spec.md` - Non-functional requirements, edge cases
- `database/Drift_Schema.md` - Local database schema
- `database/Supabase_Schema.md` - Cloud database schema
- `database/Sync_Strategy.md` - Implementasi synchronization
- `Test_Plan.md` - Testing strategy dan test cases
- `Operations_Guide.md` - Deployment dan monitoring

### External Resources
- [Flutter Documentation](https://docs.flutter.dev)
- [Drift Documentation](https://drift.simonbinder.eu)
- [Supabase Documentation](https://supabase.com/docs)
- [BLoC Library](https://bloclibrary.dev)

---

**Document Status:** ✅ Complete - Siap untuk implementasi
**Terakhir Diperbarui:** Oktober 2025
**Next Review:** Setelah MVP completion
