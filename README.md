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
index.html   ← single file (~104KB)
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