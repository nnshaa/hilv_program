# GymLog

> Gym tracking app — mobile-first, PWA, zero dependencies.

Dimulai sebagai HILV program tracker untuk natural lifter cutting phase, berkembang menjadi full gym tracking app yang bisa dipakai siapa saja dengan program apapun.

Single file HTML, vanilla JS, localStorage. No build step.

---

## Features

- **Home** — greeting, streak tracker, today's session card, quick stats, body weight log, recent PRs
- **Workout** — set logger, previous session reference, rest timer, PR detection, progress chart per exercise
- **Program** — multiple programs, switch aktif, detail sesi, start workout langsung
- **History** — log sesi yang sudah selesai (nama program, durasi, total sets)

**Program Builder**
- Buat dari template (HILV Cutting · PPL Bulking · Full Body 3x) atau kosong
- Edit nama, ikon emoji, tujuan program (cutting/bulking/strength/general)
- Tambah/edit/hapus sesi — nama, focus, hari latihan
- Weekly planner — assign sesi ke hari via visual 7-day grid
- Exercise library — 40 built-in, search, filter by muscle, tambah custom

**Lainnya**
- Rest Timer — circular countdown, preset 1:00–3:00, haptic feedback
- PR Detection — otomatis deteksi personal record saat set di-check
- Streak Tracker — dot grid 14 hari, emoji berubah sesuai momentum
- Body Weight Log — histori 5 entry terakhir, delta per hari
- **Edit Beban & Sets** — bottom sheet stepper ±2.5kg, preset ±5%/±10%, adjust sets langsung dari workout
- **Share Progress** — generate gambar PNG dari progress chart via Canvas API, native share / download
- Backup & Restore — export/import JSON, hapus semua data
- Dark/Light mode toggle
- PWA — installable, offline-ready, iOS install hint

---

## Stack

Vanilla HTML · CSS · JavaScript — zero dependencies, zero build step.

```
index.html   ← single file (~109KB)
```

Font: `DM Sans` · `DM Mono` via Google Fonts

---

## Usage

```bash
open index.html

npx serve .
python3 -m http.server 8000
```

---

## Storage

| Key      | Isi                        |
|----------|----------------------------|
| `gl_p`   | Array of programs          |
| `gl_a`   | Active program ID          |
| `gl_l`   | Log sets per exercise      |
| `gl_lib` | Custom exercise library    |
| `gl_bw`  | Body weight entries        |
| `gl_h`   | Session history            |
| `gl_t`   | Theme preference           |

**Program structure**
```js
{
  id, name, emoji, goal,
  sessions: [{
    id, name, focus,
    day,        // 0=Sun … 6=Sat, -1=unassigned
    exercises: [{ id, lid, name, sets, reps, load, rir }]
  }]
}
```

**Log structure**
```js
{
  [exId]: [
    { _date: '2026-05-08', _lbl: 'Fri May 08 2026',
      1: { kg: 80, reps: 8 },
      2: { kg: 78, reps: 8 } }
  ]
}
```

**History structure**
```js
[{
  date: '2026-05-08',
  dateLbl: 'Jum, 08 Mei 2026',
  name: 'Upper A',
  prog: 'HILV High Frequency',
  el: 3420,       // elapsed seconds
  exs: 6,         // exercises logged
  sets: 14        // total sets done
}]
```

---

## Templates

| Template | Split | Days | Goal |
|----------|-------|------|------|
| HILV High Frequency | Upper-Lower | 5 | Cutting |
| PPL 6 Days | Push-Pull-Legs | 6 | Bulking |
| Full Body 3x | Full Body | 3 | General |

---

## Changelog

### [4.5.2] — 2026-05-09

**Fix: Cardio Block · Workout Timer · Total Waktu Editable**

- Cardio exercise render block terpisah — input Durasi · Incline · Speed + tombol ✓ Selesai
- Tidak ada countdown timer (dihapus — tidak praktis tanpa HP)
- Previous session ditampilkan: `Prev: 20min · 8% · 6km/h`
- Timer workout count-up normal (stopwatch)
- Tap **Finish** → sheet dua input besar: **menit** + **detik**, prefilled dari stopwatch aktual
- Waktu bisa diedit manual sebelum disimpan (koreksi kalau HP ditinggal)
- Tap **Lewati** → simpan waktu aktual tanpa catatan

---

### [4.4.0] — 2026-05-09

**Medium Features: Volume Heatmap · Session Notes**

**Volume Heatmap (15 Weeks)**
- Grid 15×7 hari di Home, muncul otomatis kalau ada log data
- 5 level warna (lime intensity) berdasarkan total sets hari itu relatif ke max
- Cell hari ini punya outline lime
- Tap cell → tampil info date + sets di footer
- Legend Less/More di header section

**Session Notes**
- Saat finish workout: sheet muncul dengan textarea "Catatan sesi"
- Bisa dilewati (tombol Lewati) atau diisi dulu baru Simpan & Selesai
- History card: tap "📝 Catatan" untuk expand/edit note yang ada
- History card tanpa note: tap "✏️ Tambah catatan" untuk add
- Notes tersimpan ke `gl_h` bersama data sesi, max 50 sesi

---

### [4.3.0] — 2026-05-09

**Quick Wins: Deload Mode · RIR Aktual · 1RM Estimator · Overload Suggester**

**Deload Week Mode**
- Toggle dari Settings → ⚡ Deload Week, pilih −10% (ringan) atau −15% (berat)
- Banner amber muncul di Home saat deload aktif, tap ✓ untuk selesaikan
- KG placeholder di set rows otomatis turun sesuai persentase deload saat workout

**RIR Aktual per Set**
- Kolom RIR input ditambahkan ke setiap set row (termasuk add set & warm-up)
- Data RIR disimpan ke log bersama kg & reps, dipakai untuk overload detection

**1RM Estimator (Epley)**
- Estimasi 1RM otomatis dari log: `kg × (1 + reps÷30)`
- Tampil di progress chart sheet, tap "Epley ⓘ" untuk lihat formula

**Progressive Overload Suggester**
- Section "Next Session Targets" muncul di Home jika ada suggestion
- Trigger: hit rep ceiling OR RIR aktual ≤ 0.5 → suggest +2.5kg
- Tap exercise langsung buka progress chart

---

### [4.2.0] — 2026-05-08

**Data: Nishaa Precision Cut + Library Expansion**

- Template HILV diganti dengan **Nishaa Precision Cut (HILV High Freq)** — program 5 hari Upper-Lower dengan beban & exercise aktual
  - Upper A: Incline DB Press · Cable Fly · CS Row Wide · Lateral Raise · Rope Pushdown · Incline DB Curl · Cardio
  - Lower A: Smith Squat · Single-Leg Ext · Single-Leg Curl · Adductor · Calf Raise · Hanging Leg Raise
  - Upper B: Close-Grip Pulldown · Smith Incline BP · CS Row Neutral · Pec Deck · Straight-Arm Pulldown · Rev Pec Deck · Hammer Curl · Cardio
  - Lower B: RDL · Single-Leg Curl · Abductor · High-Foot Leg Press · LP Calf Raise · Hanging Leg Raise
  - Upper C: Smith Shoulder Press · Lateral Raise · Seated Face Pull · Barbell Shrug · Preacher Curl · Reverse Curl · OH Tricep Ext · Cardio
- Exercise library: **40 → 93 exercises**
  - Chest: +5 (Smith Incline BP, Pec Deck, Low-to-High Fly, Decline BP, Cable Crossover)
  - Back: +7 (Close-Grip Pulldown, CS Row Neutral, Straight-Arm Pulldown, Barbell Row, Meadows Row, Rack Pull, CS DB Row)
  - Shoulders: +8 (Smith Seated Press, Seated Face Pull, Rev Pec Deck, Barbell Shrug, DB Shrug, Cable Lateral Raise, Arnold Press, Behind-Neck Press)
  - Triceps: +4 (Skull Crushers, Tricep Dip, Cable Kickback, Single-Arm OH Extension)
  - Biceps: +5 (Preacher Curl, Reverse Curl, Concentration Curl, Spider Curl, Cable Hammer Curl)
  - Quads: +5 (Hack Squat, Front Squat, Leg Press Bilateral, Leg Extension Bilateral, Walking Lunge)
  - Hamstrings: +3 (Nordic Curl, Good Morning, Stiff-Leg Deadlift)
  - Glutes: +3 (Cable Pull-Through, Glute Kickback, Smith Hip Thrust)
  - Abductors: +3 (Hip Abductor Machine, Cable Hip Abduction, Clamshell) — muscle group baru
  - Calves: +2 (Leg Press Calf Raise, Donkey Calf Raise)
  - Core: +4 (Ab Wheel, Decline Sit-Up, Russian Twist, Side Plank)
  - Cardio: +3 (Stairmaster, Elliptical, Jump Rope)
- Filter muscle "Abductors" ditambahkan ke Exercise Library

---

### [4.1.0] — 2026-05-08

**Bug Fixes**
- `delProg` — extra quote di onclick string menyebabkan JS error saat hapus program dengan apostrof di nama
- `pcOpen` / `makeExBlock` — double `''` di nama exercise menyebabkan progress chart tidak bisa dibuka
- `pbSlot` (weekly planner) — logika deteksi slot kosong salah pakai array index, seharusnya cek nilai `day`

**Added**
- Edit Beban & Sets — tombol `⊕` di setiap exercise header saat workout aktif; bottom sheet stepper ±2.5kg, preset ±5%/±10%, stepper sets; auto-add rows dan sync placeholder kg saat sets ditambah
- Share Progress — tombol `↑ Share` di progress chart; Canvas API 800×420px dengan stats row, line chart, footer branding; native Web Share API dengan fallback download PNG
- History view sekarang simpan dan tampilkan nama program + total sets; date label terformat

**Improved**
- Responsive breakpoint 380px: set logger grid lebih compact untuk layar kecil
- Responsive breakpoint 640px+: stepper edit beban lebih besar di tablet
- `finishWK` simpan data history lebih lengkap (program name, sets count, formatted date)

---

### [4.0.0] — 2026-05-08

**Full Rebuild — landing page → gym tracking app**

- Arsitektur dirombak total: 4-tab bottom navigation (Home · Workout · Program · History)
- Mobile-first layout dengan CSS custom properties dan DM Sans/DM Mono
- Semua konten dinamis — tidak ada hardcoded program-specific content
- Storage keys baru (`gl_*`), data structure lebih bersih
- File size: ~280KB → ~92KB
- Hapus: hero section, panduan eksekusi, volume tracker static, weekly overview static

**Added**
- Template picker saat buat program baru
- Weekly planner embedded di program builder
- Session picker di Workout tab saat tidak ada active workout
- Workout elapsed timer + progress bar
- Session history di History tab

---

### [3.2.0] — 2026-05-03

**Changed**
- Volume Tracker, Weekly Overview, Execution Guide, Footer — semua dinamis berdasarkan program aktif
- Execution Guide berubah konten sesuai goal: Cutting / Bulking / General
- Re-render otomatis saat switch program atau save

---

### [3.1.0] — 2026-05-03

**Added**
- Template system: HILV · PPL · Full Body 3x · Kosong
- Weekly Planner — 7-day grid, drag & drop (desktop), tap → day picker (mobile)

---

### [3.0.0] — 2026-05-03

**Added**
- Program Manager — buat, edit, switch, hapus program
- Program Builder — fullscreen editor: nama, emoji, sesi
- Session Editor — nama, focus, hari latihan
- Exercise Library — 40 exercise, searchable, filterable, multi-select
- Dynamic tabs & panels dirender dari data program

---

### [2.1.0] — 2026-05-03

**Added**
- Swipe antar tab (mobile)
- Streak Tracker — dot 14 hari, best streak, emoji momentum
- Body Weight Log — input + line chart, delta harian
- Share Progress — kartu PNG via Canvas API

---

### [2.0.1] — 2026-05-03

**Fixed**
- `bkClearAll` hanya hapus 2/9 localStorage key
- Backup tidak lengkap — sekarang include semua keys
- `loadAll` tidak guard data corrupt
- `saveAll` tidak handle QuotaExceededError
- Custom exercise exid pakai `Date.now()` (collision risk) → `crypto.randomUUID()`
- Deload + BW = 0 menampilkan `NaN kg`
- `rtTick` crash kalau DOM element null

---

### [2.0.0] — 2026-05-03

**Added**
- Edit beban & sets dari UI — tap di edit mode, bottom sheet stepper ±2.5kg
- Quick preset ±5% ±10%, persist ke localStorage

---

### [1.9.0] — 2026-05-03

**Added**
- Edit Mode per panel — sembunyikan, pindah, reorder exercise
- Drag & drop reorder, persist urutan

---

### [1.8.0] — 2026-05-03

**Fixed** — onboarding slides, deload rounding, custom ex delete

**Added** — Backup & Restore modal (export JSON, import, clear all)

---

### [1.7.0] — 2026-05-03

**Added** — Dark/Light mode, onboarding 4-step, custom exercise, deload week mode

---

### [1.6.0] — 2026-05-03

**Added** — iOS install hint, PR detection + toast, weekly summary section

---

### [1.5.0] — 2026-05-03

**Added** — PWA: manifest blob, service worker, offline mode, install banner

---

### [1.4.0] — 2026-05-03

**Added** — Notes per set, reset log per exercise

---

### [1.3.1] — 2026-05-02

**Fixed** — Multi-session chart bug, date storage normalization, data migration

---

### [1.3.0] — 2026-05-02

**Added** — Progress chart per exercise (KG · Reps · Volume), SVG line chart

---

### [1.2.0] — 2026-05-02

**Added** — Export log CSV

---

### [1.1.0] — 2026-05-02

**Added** — Rest Timer (circular countdown, preset, haptic)

---

### [1.0.0] — 2026-05-02

Initial release — HILV program tracker untuk natural lifter cutting phase.

- Single-file HTML, set logger, previous session reference
- localStorage persistence, same-day overwrite, max 12 sesi per exercise
- Today detection, volume tracker, recovery windows, scroll reveal
- Mobile-first responsive: 320px → desktop