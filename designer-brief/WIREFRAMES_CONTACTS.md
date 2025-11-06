# Wireframes - Contacts Screens

← [Sebelumnya: WIREFRAMES_COMPANIES.md](./WIREFRAMES_COMPANIES.md)

---

## Overview

Dokumen ini berisi detailed ASCII wireframes untuk **EPIC 3: Manage Contacts** (5 screens).

**Total Wireframes:** 10 wireframes
- Screen 8: Contacts List (2 states)
- Screen 9: Contact Detail (1 wireframe)
- Screen 10: Create Contact Form (3 states)
- Screen 11: Edit Contact Form (1 wireframe)
- Screen 12: Contact Search Results (2 states)

---

## Design Specifications

### Platform & Dimensions
- **Platform:** Android Mobile
- **Screen Size:** 360 × 800 dp
- **Orientation:** Portrait only
- **Grid System:** 8dp base unit

### Typography
- **Title Large:** 22sp, Medium weight
- **Title Medium:** 16sp, Medium weight
- **Body Large:** 16sp, Regular weight
- **Body Medium:** 14sp, Regular weight

### Colors
- **Primary:** #2E7D32 (green)
- **Text Primary:** #212121 (gray-900)
- **Text Secondary:** #757575 (gray-600)
- **Surface:** #FFFFFF (white)
- **Background:** #F5F5F5 (gray-100)

---

## Screen 8: Contacts List (per Company)

### User Story
[US-3.1: Lihat List Contacts](../developer-brief/USER_STORIES.md#us-31-lihat-list-contacts)

### Tujuan
Menampilkan daftar contacts untuk perusahaan tertentu. Ditampilkan dalam tab "Kontak" di Company Detail screen.

---

### Wireframe 1: Contacts List - Loaded State

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  PT Surya Indah              ✏️       ┃ ← Company Detail App Bar
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃
┃                                            ┃
┃  INFORMASI PERUSAHAAN                      ┃ ← Collapsed company info
┃  ┌──────────────────────────────────────┐  ┃   (scroll to expand)
┃  │  PT Surya Indah • Jakarta           │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌─────────────────┬─────────────────────┐ ┃ ← Tabs
┃  │  KONTAK (3)     │     PROYEK (2)      │ ┃   KONTAK active
┃  └─────────────────┴─────────────────────┘ ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  👤  Budi Santoso                    │  ┃ ← Contact Card
┃  │      [PRIMARY CONTACT]               │  ┃   Name + badge
┃  │                                      │  ┃   Height: 88dp
┃  │      Manager Procurement             │  ┃   Position
┃  │      📞 0812-3456-7890               │  ┃   Phone
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  👤  Siti Aminah                     │  ┃
┃  │                                      │  ┃
┃  │      Direktur Operasional            │  ┃
┃  │      📞 0813-9876-5432               │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  👤  Andi Wijaya                     │  ┃
┃  │                                      │  ┃
┃  │      Staff Purchasing                │  ┃
┃  │      📞 0821-2468-1357               │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                 ┌────┐     ┃
┃                                 │ +  │     ┃ ← FAB (add contact)
┃                                 └────┘     ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Dimensi:**
- Contact card: Lebar penuh minus 32dp, tinggi 88dp
- Card padding: 16dp all sides
- Card elevation: 2dp
- Badge: Tinggi 20dp, padding 4dp horizontal

**Elemen Contact Card:**
- Baris 1: Icon person (24dp) + Nama contact (Title Medium, 16sp)
- Badge: "[PRIMARY CONTACT]" jika is_primary = true
  - Background: #E8F5E9 (green-50)
  - Text: #2E7D32 (primary green), 10sp
- Baris 2: Posisi (Body Medium, 14sp, secondary color)
- Baris 3: Icon phone (16dp) + Nomor telepon (Body Medium, 14sp)

**Interaksi:**
- Tap contact card → Navigate ke Contact Detail (Screen 9)
- Tap FAB (+) → Navigate ke Create Contact Form (Screen 10)

---

### Wireframe 2: Contacts List - Empty State

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  PT Surya Indah              ✏️       ┃
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃
┃                                            ┃
┃  INFORMASI PERUSAHAAN                      ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  PT Surya Indah • Jakarta           │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌─────────────────┬─────────────────────┐ ┃
┃  │  KONTAK (0)     │     PROYEK (2)      │  ┃
┃  └─────────────────┴─────────────────────┘ ┃
┃                                            ┃
┃                                            ┃
┃              ┌─────────┐                   ┃ ← Empty state illustration
┃              │         │                   ┃   120×120dp
┃              │  👥     │                   ┃   Gray-400 color
┃              │         │                   ┃
┃              └─────────┘                   ┃
┃                                            ┃
┃     Belum ada kontak untuk perusahaan ini  ┃ ← Title Medium (16sp)
┃                                            ┃
┃     Tap tombol + untuk menambahkan         ┃ ← Body Medium (14sp)
┃     kontak pertama                         ┃   Secondary color
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                 ┌────┐     ┃
┃                                 │ +  │     ┃
┃                                 └────┘     ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**State:**
- Tidak ada contacts untuk perusahaan ini
- Tab counter: "KONTAK (0)"
- FAB visible (sales rep bisa create)

---

## Screen 9: Contact Detail

### User Story
[US-3.1: Lihat List Contacts](../developer-brief/USER_STORIES.md#us-31-lihat-list-contacts)

### Tujuan
Menampilkan informasi lengkap contact dengan quick action buttons (call, WhatsApp, email).

---

### Wireframe 3: Contact Detail - Full View

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Budi Santoso                ✏️       ┃ ← Top App Bar
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃   Name + edit icon
┃                                            ┃
┃  INFORMASI KONTAK                          ┃ ← Section header
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Nama                                │  ┃
┃  │  Budi Santoso                        │  ┃ ← Read-only fields
┃  │                                      │  ┃   Label + value pairs
┃  │  [PRIMARY CONTACT]                   │  ┃ ← Badge (if is_primary)
┃  │                                      │  ┃
┃  │  Posisi                              │  ┃
┃  │  Manager Procurement                 │  ┃
┃  │                                      │  ┃
┃  │  No. Telepon                         │  ┃
┃  │  0812-3456-7890                      │  ┃
┃  │                                      │  ┃
┃  │  Email                               │  ┃
┃  │  budi.santoso@suryaindah.co.id       │  ┃
┃  │                                      │  ┃
┃  │  Perusahaan                          │  ┃
┃  │  PT Surya Indah                      │  ┃ ← Link to company
┃  │                                      │  ┃
┃  │  Dibuat oleh                         │  ┃
┃  │  Budi Wijaya (Sales Rep)             │  ┃
┃  │                                      │  ┃
┃  │  Dibuat pada                         │  ┃
┃  │  10 Jan 2025, 14:30                  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  AKSI CEPAT                                ┃ ← Section header
┃  ┌──────┬──────────┬──────────┬─────────┐  ┃ ← Action buttons (grid)
┃  │  📞  │    💬    │    📧    │   👤   │  ┃   Icon buttons
┃  │ Call │ WhatsApp │  Email   │ Company│  ┃   48×48dp each
┃  └──────┴──────────┴──────────┴─────────┘  ┃
┃                                            ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Dimensi:**
- Info card: Lebar penuh minus 32dp, padding 16dp
- Action buttons: 4-column grid, masing-masing 48×48dp
- Button spacing: 8dp horizontal gap

**Aksi Cepat Buttons:**
1. **Call** (📞): Open phone dialer dengan nomor ter-fill
2. **WhatsApp** (💬): Open WhatsApp dengan nomor ter-fill
3. **Email** (📧): Open email app dengan email ter-fill
4. **Company** (👤): Navigate ke Company Detail

**Interaksi:**
- Tap edit icon (✏️) → Navigate ke Edit Contact Form (Screen 11)
- Tap "PT Surya Indah" → Navigate ke Company Detail
- Tap action buttons → Open respective apps

---

## Screen 10: Create Contact Form

### User Story
[US-3.2: Buat Contact Baru](../developer-brief/USER_STORIES.md#us-32-buat-contact-baru)

### Tujuan
Memungkinkan sales rep membuat contact baru untuk perusahaan tertentu.

---

### Wireframe 4: Create Contact - Empty Form

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Tambah Kontak               💾       ┃ ← Top App Bar
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃   Back + save icon
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Nama *                              │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃ ← Text input (required)
┃  │  │                                │  │  ┃   Height: 56dp
┃  │  └────────────────────────────────┘  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Posisi                              │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃ ← Text input (optional)
┃  │  │                                │  │  ┃
┃  │  └────────────────────────────────┘  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  No. Telepon                         │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃ ← Phone input (optional)
┃  │  │                                │  │  ┃   Type: tel
┃  │  └────────────────────────────────┘  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Email                               │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃ ← Email input (optional)
┃  │  │                                │  │  ┃   Type: email
┃  │  └────────────────────────────────┘  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  ☐  Kontak Utama                     │  ┃ ← Checkbox
┃  └──────────────────────────────────────┘  ┃   Height: 48dp
┃                                            ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  BATAL              SIMPAN           │  ┃ ← Bottom action bar
┃  └──────────────────────────────────────┘  ┃   SIMPAN disabled (gray)
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Form Fields:**
1. **Nama*** (required) - Text input, max 255 characters
2. **Posisi** (optional) - Text input, max 100 characters
3. **No. Telepon** (optional) - Phone input, format validation
4. **Email** (optional) - Email input, format validation
5. **Kontak Utama** (optional) - Checkbox, default: unchecked

**Validasi:**
- Nama: Tidak boleh kosong
- Phone: Format 08xx-xxxx-xxxx atau 62xx-xxxx-xxxx
- Email: Format valid email

**State:**
- Button SIMPAN: Disabled sampai nama terisi
- Auto-save: Setiap 30 detik

---

### Wireframe 5: Create Contact - Validation Error

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Tambah Kontak               💾       ┃
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Nama *                              │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃
┃  │  │                                │  │  ┃ ← Empty (error)
┃  │  └────────────────────────────────┘  │  ┃   Border: Red
┃  │  ⚠️ Nama kontak wajib diisi          │  ┃ ← Error message
┃  └──────────────────────────────────────┘  ┃   Body Small (12sp), red
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Posisi                              │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃
┃  │  │ Manager                        │  │  ┃
┃  │  └────────────────────────────────┘  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  No. Telepon                         │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃
┃  │  │ 12345                          │  │  ┃ ← Invalid format
┃  │  └────────────────────────────────┘  │  ┃   Border: Red
┃  │  ⚠️ Format nomor tidak valid          │  ┃ ← Error message
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Email                               │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃
┃  │  │ budi.santoso@suryaindah.co.id  │  │  ┃ ← Valid
┃  │  └────────────────────────────────┘  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  ☐  Kontak Utama                     │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  BATAL              SIMPAN           │  ┃ ← SIMPAN disabled
┃  └──────────────────────────────────────┘  ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Penanganan Error:**
- Nama empty: "Nama kontak wajib diisi"
- Phone format salah: "Format nomor tidak valid (contoh: 0812-3456-7890)"
- Email format salah: "Format email tidak valid"
- Error muncul on blur atau saat tap SIMPAN

---

### Wireframe 6: Create Contact - Saving State

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Tambah Kontak               💾       ┃
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Nama *                              │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃
┃  │  │ Budi Santoso                   │  │  ┃ ← Fields DISABLED
┃  │  └────────────────────────────────┘  │  ┃   Opacity: 50%
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Posisi                              │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃
┃  │  │ Manager Procurement            │  │  ┃
┃  │  └────────────────────────────────┘  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  No. Telepon                         │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃
┃  │  │ 0812-3456-7890                 │  │  ┃
┃  │  └────────────────────────────────┘  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Email                               │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃
┃  │  │ budi.santoso@suryaindah.co.id  │  │  ┃
┃  │  └────────────────────────────────┘  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  ☑  Kontak Utama                     │  ┃ ← Checked
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │              ⏳ Menyimpan...         │  ┃ ← Loading state
┃  └──────────────────────────────────────┘  ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**State:**
- Semua fields disabled (opacity 50%)
- Button loading: Spinner + "Menyimpan..."
- Durasi: 1-3 detik

**Setelah Success:**
- Navigate back ke Contacts List (Company Detail > Kontak tab)
- Show success snackbar: "Kontak berhasil ditambahkan"

---

## Screen 11: Edit Contact Form

### User Story
[US-3.3: Edit Contact](../developer-brief/USER_STORIES.md#us-33-edit-contact)

### Tujuan
Memungkinkan sales rep mengedit contact yang sudah ada (hanya contact miliknya).

---

### Wireframe 7: Edit Contact - Pre-filled Form

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  Edit Kontak                 💾       ┃ ← Top App Bar
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃   Title: "Edit Kontak"
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Nama *                              │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃
┃  │  │ Budi Santoso                   │  │  ┃ ← Pre-filled
┃  │  └────────────────────────────────┘  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Posisi                              │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃
┃  │  │ Manager Procurement            │  │  ┃
┃  │  └────────────────────────────────┘  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  No. Telepon                         │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃
┃  │  │ 0812-3456-7890                 │  │  ┃
┃  │  └────────────────────────────────┘  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  Email                               │  ┃
┃  │  ┌────────────────────────────────┐  │  ┃
┃  │  │ budi.santoso@suryaindah.co.id  │  │  ┃
┃  │  └────────────────────────────────┘  │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  ☑  Kontak Utama                     │  ┃ ← Checked (is_primary)
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃                                            ┃
┃  Terakhir disimpan baru saja               ┃ ← Auto-save indicator
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  BATAL              SIMPAN           │  ┃
┃  └──────────────────────────────────────┘  ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Perbedaan dari Create:**
- Title: "Edit Kontak" (bukan "Tambah Kontak")
- Semua fields pre-filled dengan data existing
- Validasi sama seperti Create
- Auto-save behavior sama

**Permissions:**
- Hanya sales rep yang membuat contact bisa edit
- Manager: Read-only (tidak ada edit icon di detail screen)

---

## Screen 12: Contact Search Results

### User Story
[US-3.1: Lihat List Contacts](../developer-brief/USER_STORIES.md#us-31-lihat-list-contacts)

### Tujuan
Menampilkan hasil pencarian contacts (search by nama, posisi, atau phone).

---

### Wireframe 8: Search Results - Found

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  ┌────────────────────────────┐  X   ┃ ← Search bar
┃     │ manager                    │       ┃   Query: "manager"
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃
┃                                            ┃
┃  2 kontak ditemukan                        ┃ ← Result count
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  👤  Budi Santoso                    │  ┃
┃  │                                      │  ┃
┃  │      Manager Procurement             │  ┃ ← "Manager" highlighted
┃  │      PT Surya Indah                  │  ┃   Bold text
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │  👤  Siti Aminah                     │  ┃
┃  │                                      │  ┃
┃  │      Manager Operasional             │  ┃
┃  │      PT Maju Jaya                    │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                 ┌────┐     ┃
┃                                 │ +  │     ┃
┃                                 └────┘     ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Search Behavior:**
- Real-time filtering (debounce 300ms)
- Search scope: Nama, Posisi, No. Telepon
- Result count: "X kontak ditemukan"
- Matching text highlighted (bold)
- Shows company name per contact

---

### Wireframe 9: Search Results - No Results

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  ←  ┌────────────────────────────┐  X   ┃
┃     │ xyz999                     │       ┃ ← Query dengan no matches
┃━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃              ┌─────────┐                   ┃
┃              │         │                   ┃
┃              │   🔍    │                   ┃ ← Search icon (gray)
┃              │         │                   ┃   120×120dp
┃              └─────────┘                   ┃
┃                                            ┃
┃    Tidak ada hasil untuk "xyz999"          ┃ ← Title Medium (16sp)
┃                                            ┃
┃    Coba kata kunci lain atau buat          ┃ ← Body Medium (14sp)
┃    kontak baru.                            ┃   Secondary color
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                 ┌────┐     ┃
┃                                 │ +  │     ┃
┃                                 └────┘     ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

---

## Design System Notes

### Material Design 3 Components

**Contacts List:**
- Cards: Elevated card (2dp elevation)
- Badge: Custom chip component
- Icons: Material icons (person, phone)

**Contact Detail:**
- Action buttons: Icon buttons (48×48dp)
- Info card: Outlined card

**Form Screens:**
- Text fields: Material 3 filled text field
- Checkbox: Material 3 checkbox
- Phone input: Text field dengan input type "tel"
- Email input: Text field dengan input type "email"

### Accessibility

- Contact cards: Minimum 88dp height
- Action buttons: 48×48dp (touch target met)
- Labels: Selalu visible (bukan hanya placeholders)
- Error messages: Dibaca oleh screen reader

---

## Implementation Notes

### Contacts List
1. **Primary Contact:**
   - Hanya 1 contact per company bisa jadi primary
   - Jika user set contact lain jadi primary, yang lama auto-unset
   - Badge hijau untuk visibility

2. **Sorting:**
   - Primary contact selalu di atas
   - Sisanya alphabetical by nama

### Form Screens
1. **Phone Format:**
   - Accept: 08xx-xxxx-xxxx atau +62xx-xxxx-xxxx
   - Auto-format saat user typing
   - Validasi on blur

2. **Email Validation:**
   - Basic format check: xxx@xxx.xxx
   - Real validation saat submit (server-side)

3. **Primary Contact Toggle:**
   - Jika checked, show confirmation jika sudah ada primary contact lain
   - Dialog: "Anda sudah memiliki kontak utama (Nama Lama). Jadikan (Nama Baru) sebagai kontak utama?"

---

## Related Documents

- **Screen Inventory:** [SCREEN_INVENTORY.md](./SCREEN_INVENTORY.md) (Screens 8-12)
- **User Stories:** [USER_STORIES.md](../developer-brief/USER_STORIES.md) (US-3.1, US-3.2, US-3.3)
- **Design Requirements:** [DESIGN_REQUIREMENTS.md](./DESIGN_REQUIREMENTS.md)

---

## Next Steps

➡️ **[Continue to: WIREFRAMES_PROJECTS.md →](./WIREFRAMES_PROJECTS.md)**

**Or navigate to:**
← [WIREFRAMES_COMPANIES.md](./WIREFRAMES_COMPANIES.md)
