# HILV — High Frequency Program

> *"Optimal bukan yang terbanyak. Optimal yang bisa disustain."*

Program tracking app untuk **natural lifter** di fase cutting — dirancang di minimum effective dose, bukan maksimum toleransi.

---

## Overview

| | |
|---|---|
| **Phase** | Cutting |
| **Frequency** | 5 Days / Week |
| **Split** | Upper A · Lower A · Upper B · Lower B · Upper C |
| **Rep Range** | 6–8 compound · 6–10 isolation |
| **Volume Target** | MED (Minimum Effective Dose) |
| **Cardio** | Incline treadmill walk 20 min × 3 sesi/week |

---

## Schedule

| Day | Session | Focus |
|-----|---------|-------|
| SAT | Upper A | Chest · Upper Back · Lat Delt · Tricep · Bicep |
| SUN | Lower A | Quad Focus · Hamstring · Adductor · Calf · Core |
| MON | Rest | Full recovery |
| TUE | Upper B | Back · Chest Balance · Rear Delt · Bicep |
| WED | Lower B | Hamstring · Glute · Quad · Calf · Core |
| THU | Upper C | Delts · Arms · Traps |
| FRI | Rest | Buffer sebelum siklus ulang |

---

## Features

- **Today detection** — auto-highlight sesi hari ini dan langsung jump ke tab yang relevan
- **Set logger** — input KG & Reps per set, checkmark saat selesai
- **Previous session** — data sesi terakhir ditampilkan di kolom Previous sebagai referensi
- **Persistent storage** — semua log tersimpan di `localStorage`, aman refresh
- **Add Set** — tambah set ekstra mid-session tanpa reload
- **Volume tracker** — sets per muscle group per minggu dengan progress bar
- **Recovery windows** — kalkulasi gap antar sesi upper/lower
- **Scroll reveal** — elemen muncul smooth saat di-scroll
- **Fully responsive** — mobile-first, tested dari 320px ke desktop

---

## Stack

Vanilla HTML · CSS · JavaScript — zero dependencies, zero build step.

```
index.html   ← single file, self-contained
```

Font: `Bebas Neue` · `Outfit` · `JetBrains Mono` via Google Fonts

---

## Local Usage

```bash
# Cukup buka langsung di browser
open index.html

# Atau serve lokal
npx serve .
python3 -m http.server 8000
```

---

## Storage

Data log tersimpan di `localStorage` dengan key `hilv_sets_v2`.

```js
// struktur per exercise
{
  "p1-01": [
    { _date: "Sat May 02 2026", 1: { kg: 80, reps: 8 }, 2: { kg: 80, reps: 7 } },
    ...
  ]
}
```

Maksimum 12 sesi terakhir per exercise disimpan. Same-day session akan overwrite entry hari yang sama.

---

## Changelog

### [1.3.0] — 2026-05-02

**Added**
- Progress Chart — modal grafik per exercise, muncul saat tap nama exercise
- Line chart SVG dengan area fill gradient, dot per sesi, label nilai
- 3 mode tampilan: KG · Reps · Volume (kg×reps total)
- Stat strip: Best KG · Sessions · Trend (% perubahan dari sesi pertama)
- Grid Y-axis dengan label nilai, X-axis dengan tanggal (M/D)
- Last session dot lebih besar + selalu diberi label
- Backdrop blur + card slide-in animation
- Nama exercise berubah jadi clickable (hover: lime + ↗)
- Empty state jika belum ada log

---

### [1.2.0] — 2026-05-02

**Added**
- Export Log CSV — tombol di footer, download otomatis ke file `hilv-log-YYYY-MM-DD.csv`
- Kolom CSV: Date · Session · Exercise · Set · KG · Reps
- Nama exercise diambil langsung dari DOM — selalu sinkron dengan program
- Toast notification saat export berhasil (jumlah set yang diexport)
- Toast amber jika belum ada log tersimpan
- Haptic feedback saat export

---

### [1.1.0] — 2026-05-02

**Added**
- Rest Timer — bottom sheet otomatis muncul setelah set di-check
- SVG circular progress ring dengan countdown real-time
- Preset waktu: 1:00 · 1:30 · 2:00 · 3:00 (bisa ganti mid-countdown)
- Haptic feedback saat timer mulai, urgent (10 detik terakhir), dan selesai
- Auto-close setelah rest selesai + pola vibrate berbeda tiap state
- Tombol Skip dan Restart di dalam sheet
- Backdrop blur saat timer aktif
- Timer tidak muncul untuk warm-up row (WU)

---

### [1.0.0] — 2026-05-02

Initial release.

**Added**
- Single-file HTML app, zero dependencies, zero build step
- 5 training days: Upper A · Lower A · Upper B · Lower B · Upper C
- Per-exercise data: nama, muscle note, sets × reps, load (kg), RIR badge
- Warm-up rows (WU) dengan styling tersendiri
- Incline treadmill cardio block di setiap upper session
- Set logger: input KG & Reps, checkmark animasi, done state (lime tint)
- Kolom Previous — referensi beban dari sesi terakhir
- Add Set — tambah set ekstra mid-session
- `localStorage` persistence key `hilv_sets_v2`, maks 12 sesi/exercise
- Same-day session overwrite — tidak duplikat entry
- Today detection — auto-tab dan banner sesi hari ini
- Volume tracker per muscle group dengan progress bar (IntersectionObserver)
- Recovery windows — gap antar sesi upper/lower dalam jam
- Execution guide — 4 panel panduan fisiologi cutting
- Scroll reveal animation pada semua card dan section
- Mobile-first responsive: 320px → 400px → 560px → 640px → desktop