# Wireframes - Authentication Screens

← [Sebelumnya: SCREEN_INVENTORY.md](./SCREEN_INVENTORY.md)

---

## Overview

Dokumen ini berisi detailed ASCII wireframes untuk **EPIC 1: Authentication** (2 screens).

**Total Wireframes:** 6 wireframes
- Screen 1: Login (4 states)
- Screen 2: Splash (2 states)

---

## Design Specifications

### Platform & Dimensions
- **Platform:** Android Mobile
- **Screen Size:** 360 × 800 dp
- **Orientation:** Portrait only
- **Grid System:** 8dp base unit

### Typography
- **Font Family:** Roboto (system default)
- **Heading Large:** 32sp, Medium weight
- **Body Large:** 16sp, Regular weight
- **Label Large:** 14sp, Medium weight
- **Error Text:** 12sp, Regular weight

### Colors
- **Primary:** #2E7D32 (green, dari CSS Logo Guidelines)
- **Error:** #D32F2F (red)
- **Text Primary:** #212121 (gray-900)
- **Text Secondary:** #757575 (gray-600)
- **Surface:** #FFFFFF (white)
- **Background:** #F5F5F5 (gray-100)

---

## Screen 1: Login Screen

### User Story
[US-1.1: Login dengan Email & Password](../developer-brief/USER_STORIES.md#us-11-login-dengan-email--password)

### Tujuan
Titik masuk untuk sales rep dan manager untuk melakukan autentikasi dan mengakses aplikasi.

---

### Wireframe 1: Login - Initial State (Empty)

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃                                            ┃ ← Status Bar (24dp height)
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃              ┌─────────────┐               ┃ ← 80dp dari top
┃              │             │               ┃
┃              │  CSS LOGO   │               ┃ ← 120×120dp logo
┃              │             │               ┃
┃              └─────────────┘               ┃
┃                                            ┃
┃         CSS Sales Report                   ┃ ← Heading Large (32sp)
┃                                            ┃   Color: Primary (#2E7D32)
┃                                            ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃ ← 24dp screen margin
┃  │ Email                                │  ┃
┃  │ ┌──────────────────────────────────┐ │  ┃
┃  │ │                                  │ │  ┃ ← Text Input (Material 3)
┃  │ └──────────────────────────────────┘ │  ┃   Height: 56dp
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃ ← 16dp spacing
┃  ┌──────────────────────────────────────┐  ┃
┃  │ Password                             │  ┃
┃  │ ┌──────────────────────────────────┐ │  ┃
┃  │ │                            👁    │ │  ┃ ← Show/Hide toggle icon
┃  │ └──────────────────────────────────┘ │  ┃   Height: 56dp
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃ ← 24dp spacing
┃  ┌──────────────────────────────────────┐  ┃
┃  │           MASUK                      │  ┃ ← Primary Button
┃  └──────────────────────────────────────┘  ┃   Height: 48dp
┃                                            ┃   Background: #E0E0E0 (disabled)
┃                                            ┃   Text: #9E9E9E
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Dimensi:**
- Margin layar: 24dp kiri/kanan
- Logo: 120×120dp, diposisikan tengah
- Margin atas logo: 80dp dari atas
- Margin atas nama app: 16dp di bawah logo
- Input fields: Lebar penuh minus 48dp (24dp × 2), tinggi 56dp
- Button: Lebar penuh minus 48dp, tinggi 48dp, corner radius 4dp
- Spacing vertikal: 16dp antar field, 24dp sebelum button

**State:**
- Field email: Kosong, placeholder "Email"
- Field password: Kosong, placeholder "Password", teks ter-obscure
- Button: Disabled (background abu-abu)

**Interaksi:**
- Tap field email → Keyboard muncul (tipe input email)
- Tap field password → Keyboard muncul (tipe input password)
- Tap icon mata → Toggle visibilitas password
- Button disabled sampai kedua field terisi

---

### Wireframe 2: Login - Filled State

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃                                            ┃
┃                                            ┃
┃              ┌─────────────┐               ┃
┃              │             │               ┃
┃              │  CSS LOGO   │               ┃
┃              │             │               ┃
┃              └─────────────┘               ┃
┃                                            ┃
┃         CSS Sales Report                   ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │ Email                                │  ┃
┃  │ ┌──────────────────────────────────┐ │  ┃
┃  │ │ budi@cssgroup.co.id              │ │  ┃ ← Filled with text
┃  │ └──────────────────────────────────┘ │  ┃   Border: Primary color
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │ Password                             │  ┃
┃  │ ┌──────────────────────────────────┐ │  ┃
┃  │ │ ••••••••                     👁    │ │  ┃ ← Password obscured
┃  │ └──────────────────────────────────┘ │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │           MASUK                      │  ┃ ← Button ENABLED
┃  └──────────────────────────────────────┘  ┃   Background: #2E7D32 (primary)
┃                                            ┃   Text: White
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Perubahan dari State 1:**
- Field email: Berisi "budi@cssgroup.co.id", border focused (warna primary)
- Field password: Berisi password ter-obscure (••••••••)
- Button: ENABLED (background hijau primary, teks putih)

**Interaksi:**
- Tap button MASUK → Submit credentials, transisi ke Loading state

---

### Wireframe 3: Login - Loading State

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃                                            ┃
┃                                            ┃
┃              ┌─────────────┐               ┃
┃              │             │               ┃
┃              │  CSS LOGO   │               ┃
┃              │             │               ┃
┃              └─────────────┘               ┃
┃                                            ┃
┃         CSS Sales Report                   ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │ Email                                │  ┃
┃  │ ┌──────────────────────────────────┐ │  ┃
┃  │ │ budi@cssgroup.co.id              │ │  ┃ ← Fields DISABLED
┃  │ └──────────────────────────────────┘ │  ┃   Opacity: 50%
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │ Password                             │  ┃
┃  │ ┌──────────────────────────────────┐ │  ┃
┃  │ │ ••••••••                     👁    │ │  ┃
┃  │ └──────────────────────────────────┘ │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │     ⏳ Memverifikasi...              │  ┃ ← Loading state
┃  └──────────────────────────────────────┘  ┃   Spinner icon (16dp)
┃                                            ┃   Text: Body Large
┃                                            ┃   Background: #C5E1A5 (lighter green)
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Perubahan dari State 2:**
- Semua input fields: DISABLED (opacity 50%)
- Button: Menampilkan loading spinner + teks "Memverifikasi..."
- Background button: Hijau lebih terang (#C5E1A5)
- Tidak ada interaksi user selama loading

**Durasi:**
- State loading: 1-3 detik (waktu actual API call)
- Jika berhasil → Navigate ke Home/Dashboard
- Jika gagal → Transisi ke Error state

---

### Wireframe 4: Login - Error State

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃                                            ┃
┃                                            ┃
┃              ┌─────────────┐               ┃
┃              │             │               ┃
┃              │  CSS LOGO   │               ┃
┃              │             │               ┃
┃              └─────────────┘               ┃
┃                                            ┃
┃         CSS Sales Report                   ┃
┃                                            ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │ ⚠️ Email atau password salah          │  ┃ ← Error banner
┃  └──────────────────────────────────────┘  ┃   Background: #FFCDD2 (red-100)
┃                                            ┃   Text: #C62828 (red-700)
┃  ┌──────────────────────────────────────┐  ┃   Height: 48dp
┃  │ Email                                │  ┃
┃  │ ┌──────────────────────────────────┐ │  ┃
┃  │ │ budi@cssgroup.co.id              │ │  ┃ ← Border: Red (#D32F2F)
┃  │ └──────────────────────────────────┘ │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │ Password                             │  ┃
┃  │ ┌──────────────────────────────────┐ │  ┃
┃  │ │ ••••••••                     👁    │ │  ┃ ← Border: Red (#D32F2F)
┃  │ └──────────────────────────────────┘ │  ┃
┃  └──────────────────────────────────────┘  ┃
┃                                            ┃
┃  ┌──────────────────────────────────────┐  ┃
┃  │           MASUK                      │  ┃ ← Button re-enabled
┃  └──────────────────────────────────────┘  ┃   Background: #2E7D32
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Perubahan dari State 3:**
- Error banner muncul di atas (tinggi 48dp, 16dp di bawah nama app)
- Pesan error: "⚠️ Email atau password salah"
- Border input field: Merah (#D32F2F) untuk menandakan error
- Button: Di-enable kembali (user bisa coba lagi)
- Fields: Di-enable kembali (user bisa edit)

**Penanganan Error:**
- 401 Unauthorized: "Email atau password salah"
- 500 Server Error: "Server error. Coba lagi nanti."
- Network Error: "Tidak ada koneksi internet"

**Interaksi:**
- User bisa edit email/password dan coba lagi
- Error banner hilang saat user mulai mengetik
- Tap MASUK → Submit lagi (kembali ke Loading state)

---

## Screen 2: Splash Screen

### User Story
Tidak secara eksplisit ada di user stories, tapi required untuk app launch experience.

### Tujuan
Menampilkan branding saat app first launch, memberikan seamless experience saat app melakukan inisialisasi.

---

### Wireframe 5: Splash Screen - Loading

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃              ┌─────────────┐               ┃
┃              │             │               ┃
┃              │             │               ┃
┃              │  CSS LOGO   │               ┃ ← 160×160dp logo (larger)
┃              │             │               ┃   Centered vertically
┃              │             │               ┃
┃              └─────────────┘               ┃
┃                                            ┃
┃                                            ┃
┃         CSS Sales Report                   ┃ ← Heading Large (32sp)
┃                                            ┃   Color: Primary (#2E7D32)
┃                                            ┃   Centered, 24dp below logo
┃                                            ┃
┃                                            ┃
┃                  ●●●                       ┃ ← Loading dots animation
┃                                            ┃   Gray-400 (#BDBDBD)
┃                                            ┃   16dp size each
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Dimensi:**
- Logo: 160×160dp, diposisikan tengah horizontal dan vertikal (sedikit offset ke atas)
- Nama app: 32sp, diposisikan tengah, 24dp di bawah logo
- Loading dots: 16dp diameter masing-masing, 8dp spacing, 32dp di bawah nama app
- Background: Putih (#FFFFFF)

**Animasi:**
- Loading dots: Pulse animation (300ms per dot, sequential)
  - Dot 1: Opacity 30% → 100% → 30% (repeat)
  - Dot 2: Animasi sama, delay 100ms
  - Dot 3: Animasi sama, delay 200ms
- Total durasi loop: 1000ms

**Durasi:**
- Waktu tampil minimum: 1 detik
- Waktu tampil maksimum: 3 detik
- Inisialisasi app terjadi di background (cek auth token, load data awal)

---

### Wireframe 6: Splash Screen - Transition Out

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃              ┌─────────────┐               ┃
┃              │             │               ┃
┃              │             │               ┃
┃              │  CSS LOGO   │               ┃ ← Fade out animation
┃              │  (fading)   │               ┃   Opacity: 100% → 0%
┃              │             │               ┃   Duration: 300ms
┃              └─────────────┘               ┃
┃                                            ┃
┃                                            ┃
┃      CSS Sales Report (fading)             ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┃                                            ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

**Animasi:**
- Fade out: 300ms, ease-out curve
- Simultan dengan next screen fade in (cross-fade)

**Keputusan Navigasi:**
- Jika user memiliki valid auth token → Navigate ke Home/Dashboard (Screen 24/26)
- Jika tidak ada auth token → Navigate ke Login Screen (Screen 1)
- Jika first time user → Navigate ke Onboarding (Screen 31), kemudian Login

---

## Design System Notes

### Material Design 3 Components Used

**Text Fields (Login):**
- Component: Material 3 Filled Text Field
- States: Default, Focused, Error, Disabled
- Height: 56dp
- Corner radius: 4dp (top corners only for filled variant)
- Label: Floating label animation
- Icons: Trailing icon (eye icon for password)

**Buttons (Login):**
- Component: Material 3 Filled Button
- States: Enabled, Disabled, Loading
- Height: 48dp
- Corner radius: 4dp
- Elevation: 2dp at rest, 4dp on press
- Ripple effect on tap

**Loading Indicator (Splash):**
- Component: Material 3 Circular Progress Indicator (indeterminate)
- Alternative: Custom dot animation (as shown)
- Color: Primary or Gray-400

### Accessibility

**Login Screen:**
- All fields have clear labels (not just placeholders)
- Error messages read by screen reader
- Touch targets: Minimum 48×48dp (button meets requirement)
- Contrast ratio: Meets WCAG AA (4.5:1 for text)

**Splash Screen:**
- No interaction required (auto-transition)
- Screen reader announces: "Loading CSS Sales Report"

---

## Implementation Notes

### Login Screen
1. **Validation:**
   - Email: Must be valid email format (@cssgroup.co.id or other domains)
   - Password: Minimum 6 characters
   - Real-time validation on blur

2. **Security:**
   - Password obscured by default
   - Toggle visibility with eye icon
   - No password hints stored locally

3. **Error Handling:**
   - Network errors: Show retry option
   - Invalid credentials: Clear error message
   - Max 5 login attempts (then temp lock)

### Splash Screen
1. **Performance:**
   - Minimal rendering (logo + text only)
   - No heavy operations on UI thread
   - Auth check in background

2. **Persistence:**
   - Check SharedPreferences for auth token
   - If token exists, validate with server (quick ping)
   - If valid, skip login

---

## Navigation Flow

```
App Launch
  ↓
Splash Screen (1-3s)
  ↓
  ├─→ [No Auth Token] → Login Screen → Home/Dashboard
  ├─→ [Valid Auth Token] → Home/Dashboard (skip login)
  └─→ [First Time User] → Onboarding → Login Screen → Home/Dashboard
```

---

## Related Documents

- **Screen Inventory:** [SCREEN_INVENTORY.md](./SCREEN_INVENTORY.md) (Screens 1-2)
- **User Stories:** [USER_STORIES.md](../developer-brief/USER_STORIES.md) (US-1.1)
- **Design Requirements:** [DESIGN_REQUIREMENTS.md](./DESIGN_REQUIREMENTS.md) (Colors, Typography)

---

## Next Steps

➡️ **[Continue to: WIREFRAMES_SYNC_SETTINGS.md →](./WIREFRAMES_SYNC_SETTINGS.md)**

**Or navigate to:**
← [SCREEN_INVENTORY.md](./SCREEN_INVENTORY.md)
