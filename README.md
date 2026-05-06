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

### [2.0.0] — 2026-05-03

**Added**
- Edit beban & sets langsung dari UI — tap `.xload` atau `.xsets` saat edit mode aktif
- Bottom sheet dengan stepper ±2.5kg untuk beban, input sets & rep range
- Quick preset buttons: −10%, −5%, +5%, +10% dari beban saat ini (atau fixed value untuk BW)
- Perubahan tersimpan di localStorage (`hilv_ex_loads`), persist setelah reload
- Auto-apply ke deload mode — kalau deload aktif, beban baru langsung dihitung -40%
- Flash animation pada `.xload` setelah simpan
- Toast konfirmasi dengan nilai baru

---

### [1.9.0] — 2026-05-03

**Added**
- Edit Mode per panel — tombol ✎ Edit di setiap sesi, aktifkan untuk masuk mode edit
- Sembunyikan exercise — tombol − per row, exercise hilang dari tampilan tapi data tetap ada
- Restore exercise — tombol ↩ Restore muncul di edit mode, tampilkan semua exercise yang disembunyikan beserta asal sesinya
- Pindah exercise antar sesi — tombol ↗ per row, pilih sesi tujuan dari bottom sheet
- Drag & drop reorder — handle ⠿ muncul di edit mode, seret untuk ubah urutan exercise
- Urutan custom tersimpan di localStorage (`hilv_ex_order`), persist setelah reload
- Status moved tersimpan di `hilv_ex_moved`, exercise tetap di posisi baru setelah reload
- Status hidden tersimpan di `hilv_ex_hidden`, persist setelah reload
- Exercise bawaan bisa di-restore kapanpun, data log tidak ikut terhapus

---

### [1.8.0] — 2026-05-03

**Fixed**
- Onboarding slide 2, 3, 4 tidak tampil karena missing closing `</div>` tag — semua slide sekarang punya konten penuh dan markup yang benar
- Deload rounding: hasil 60% kini dibulatkan ke kelipatan 2.5 terdekat dengan minimum 5kg (contoh: 85kg → 52.5kg bukan 51kg)
- Custom exercise tidak bisa dihapus — sekarang ada tombol ✕ kecil di setiap custom exercise row

**Added**
- Backup & Restore modal — tombol di footer
- Export backup: download seluruh log + custom exercise + settings sebagai `.json`
- Import backup: pilih file atau drag & drop, merge dengan data yang sudah ada (tidak overwrite)
- Clear all data: hapus seluruh localStorage dengan konfirmasi
- Backup menyimpan: log sesi, custom exercise, theme preference, deload state

---

### [1.7.0] — 2026-05-03

**Added**
- Dark/Light mode toggle — tombol ☀️/🌙 di bar, state tersimpan di localStorage
- Light mode full: background, card, input, modal, semua elemen ikut berubah
- Onboarding — 4-step walkthrough muncul saat pertama buka app, bisa di-skip
- Custom Exercise — tambah exercise sendiri ke sesi manapun (nama, muscle, sets, load)
- Custom exercise tersimpan di localStorage, muncul kembali saat reload
- Badge "custom" di setiap exercise yang ditambah sendiri
- Custom exercise punya tracker, chart progress, dan reset log seperti exercise bawaan
- Tombol "+ Custom Exercise" muncul di bawah setiap panel sesi
- Deload Week mode — aktifkan via footer, beban otomatis dikurangi 40%
- Banner deload muncul di atas panel saat mode aktif
- Strikethrough beban asli + tampilkan beban deload di sebelahnya
- State deload tersimpan di localStorage, persist setelah reload

---

### [1.6.0] — 2026-05-03

**Added**
- iOS Install Hint — banner muncul di Safari iPhone dengan instruksi "Tap ↑ Share → Add to Home Screen", tidak muncul kalau sudah standalone
- PR Detection — otomatis deteksi Personal Record saat set di-check, bandingkan vs seluruh history sebelum hari ini
- PR Toast — notifikasi slide-in di atas layar dengan nama exercise + nilai PR baru, haptic pola khusus
- Weekly Summary section — tampil di bawah program, auto-hitung dari localStorage
- 4 stat cards: Sets Done · Total Volume · Sessions · New PRs, masing-masing ada delta % vs minggu lalu
- PR list minggu ini — daftar exercise yang tembus PR, sorted by exercise
- Week range label (M/D — M/D) auto-update setiap hari

---

### [1.5.0] — 2026-05-03

**Added**
- PWA support — app bisa diinstall ke homescreen Android & iOS
- Manifest inline sebagai Blob URL — zero file tambahan, tetap single file
- Service Worker via Blob URL — cache-first strategy, fallback ke index.html
- Offline mode — app tetap bisa diakses tanpa koneksi, toast "⚡ Offline mode" muncul otomatis
- Install banner slide-down muncul 1.5 detik setelah buka app (sekali per sesi)
- Tombol dismiss — banner tidak muncul lagi di sesi yang sama (`sessionStorage`)
- `appinstalled` event — toast konfirmasi setelah install berhasil
- Cache versioning `hilv-v1.4` — old cache otomatis dihapus saat update

---

### [1.4.0] — 2026-05-03

**Added**
- Notes per set — textarea muncul di bawah setiap set row, simpan RPE/feeling/teknik
- Notes ikut tersimpan di localStorage dan di-restore saat buka ulang
- Auto-resize textarea sesuai konten, border lime jika ada isi
- Notes ikut ter-export ke CSV sebagai kolom tambahan
- Reset log per exercise — tombol merah di bawah set tracker
- Confirm dialog sebelum hapus: nama exercise + warning permanen
- Reset hapus semua history exercise dari localStorage + clear UI
- Toast konfirmasi setelah reset berhasil

---

### [1.3.1] — 2026-05-02

**Fixed**
- Chart hanya tampil 1 titik meski sudah ada data dari minggu lalu
- Format tanggal tidak konsisten — `toDateString()` diganti ke ISO `"YYYY-MM-DD"` sebagai `_date`, display label disimpan terpisah di `_dateLabel`
- Same-day detection gagal saat format `_date` berbeda (ISO vs toDateString) — sekarang cek keduanya
- `pcGetPoints` ikut menghitung `_dateLabel` sebagai set data — fix filter ke `!k.startsWith('_')`
- `loadAll` kini auto-migrate data lama ke format baru saat pertama dibuka — tidak perlu reset localStorage

---

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