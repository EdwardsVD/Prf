# Prf — Lyric Player & Video Editor

Dua aplikasi browser standalone (cukup buka file `.html` di browser, tanpa install apa pun):

| File | Isi |
| --- | --- |
| `lyric_player.html` | **Lyric Player** — putar lagu + lirik sinkron (LRC), tap-sync, editor timing, style/efek, sampai export video lirik |
| `video_editor.html` | **Video Editor** — editor video ala CapCut di browser: potong clip, efek, animasi, subtitle, export MP4 |

Mulai **v0.1.0** kedua mode dipisah menjadi file masing-masing (sebelumnya digabung dalam satu file `lyric_sync_player.html`).

## Lyric Player

- Import audio (MP3/dll) atau MP4 + lirik format `.lrc` (atau `.srt`, otomatis dikonversi).
- Sinkronisasi lirik: paste LRC, tap-sync, atau editor timing lengkap (undo/redo, nudge, loop A-B).
- Style teks lengkap: font, ukuran, warna, outline, glow, gradient, partikel, uppercase, zero-emoji.
- Export video lirik (720p/1080p) langsung dari browser.
- Proyek bisa disimpan/dibuka lagi (`.json`) + autosave sesi di localStorage.

### Baru di v0.4.0: Background Foto/Video + Banyak Font

- **Background foto/video**: pilih file foto atau video jadi background; teks tetap di tengah. Video pendek **auto-loop** biar sinkron sama lagu; slider dim biar lirik kebaca. Berlaku juga di mode karaoke.
- **25+ pilihan font** dalam 5 kategori (Settings → Style).
- **Full Text (karaoke)**: waktu `[mm:ss.ms]` pindah ke bawah tiap bait + bisa di-on/off.

### Baru di v0.3.0: Gelombang Audio + Full Text Karaoke

- **Gelombang Audio (Waveform)**: visualizer live yang tinggi, model (Bars / Mirror / Wave), dan warnanya bisa diatur — di Settings → Effects.
- **Full Text .lrc**: semua baris lirik tampil; background abu-abu pindah mulus ke baris aktif mengikuti waktu `[mm:ss.ms]`. Tombol cepat **Full Text** di pojok kanan atas.

### Baru di v0.1.0: Auto-Hide Lirik (jeda)

- Placeholder `"[Musik]"` **dihapus**. Saat tidak ada lirik aktif (intro, jeda, akhir lagu) layar tampil **kosong — hitam polos sesuai background**.
- Kalau sebuah lirik **tidak ada lanjutan selama N detik**, lirik itu otomatis hilang dan layar jadi kosong sampai lirik berikutnya masuk.
- N bisa diatur di **Settings → Effects → Auto-Hide Lirik (Jeda)** — default **5 detik**, bisa diganti (misal **4**). Berlaku di player live **dan** hasil export video.

## Video Editor

- Import MP4 / video / audio / gambar (klik atau drag & drop).
- Timeline multi-clip: geser, trim ujung, split (`S`), duplikat (`D`), hapus (`Del`), undo/redo.
- Efek & animasi clip, preset filter warna, zoom/rotate/flip/opacity, kecepatan (rate).
- Subtitle/teks + preset (TITLE / LOWER THIRD / SUBTITLE / CARD); tempel banyak baris → jadi subtitle berurutan otomatis.
- Loop A-B, context menu (klik kanan / long-press), shortcut keyboard lengkap (tekan `?`).
- Export MP4/WebM (MediaRecorder) sampai **1080p 60fps**; optimasi performa HP (preview adaptif, loop hemat baterai).
- `Ctrl+S` simpan project (JSON) + autosave draft otomatis di browser.

## Changelog v0.4.0 (Lyric Player)

Update kali ini hanya di **Lyric Player** (`lyric_player.html`), tetap **satu file .html saja**; Video Editor tidak berubah.

**Fix yang diminta:**
- **Full Text (karaoke)**: waktu `[mm:ss.ms]` tidak lagi di samping teks — dipindah ke **paling bawah tiap bait**, plus tombol **on/off** ("Tampilkan Waktu" di Settings → Effects). Posisi teks otomatis menyesuaikan (layout berubah jadi kolom tengah).
- **Zero Emoji diperkuat**: blok emoji yang sebelumnya lolos (🫶, flag-tag sequence 🏴󠁧󠁢󠁥󠁮󠁧󠁿, 🃏, ™, ‼️, ⏯, keycap, dll) sekarang ikut dibersihkan.

**Fitur baru:**
- **Pilihan font banyak** — 25+ font dalam 5 kategori (Modern, Tebal/Display, Elegan/Serif, Santai/Handwriting, Mono): Poppins, Montserrat, Bebas Neue, Oswald, Archivo Black, Playfair Display, Lora, Pacifico, Dancing Script, Caveat, Bangers, Righteous, JetBrains Mono, dll (Google Fonts dimuat di head; font juga di-preload sebelum export video biar tidak fallback).
- **Background foto / video** — pilih foto atau video sebagai background (Settings → Style); teks **tetap di tengah**. Video yang lebih pendek dari lagu **di-loop otomatis** (misal lagu 12 menit, background 6 menit → diputar ulang terus biar tetap sinkron dengan subtitle). Ada slider **dim** (redupkan background 0–90%) biar lirik tetap kebaca. Berlaku juga untuk mode Full Text/karaoke. Background ikut play/pause mengikuti lagu.
- Shortcut baru: **T** = toggle Full Text (karaoke).

**Bug fixes:**
- Fix `formatTimeWithMs` (label ms tapi cuma 1 digit + float error `01:05.049`) → sekarang milidetik 3 digit via pembulatan total-ms.
- Fix sesi terakhir: style/effects/offset/animasi yang tersimpan sekarang **dipulihkan** kalau nama file audionya sama (sebelumnya diabaikan).
- Fix potensi kebocoran object URL background (URL dibuat di luar state-updater + revoke saat unmount via ref).

## Changelog v0.3.0 (Lyric Player)

Update kali ini hanya di **Lyric Player** (`lyric_player.html`); Video Editor tidak berubah.

- **Gelombang Audio (Waveform)** — visualizer live di bawah layar, dianalisis real-time dari audio via WebAudio. Bisa diatur:
  - **Model**: `Bars` (kotak-kotak), `Mirror` (simetris tengah), `Wave` (garis halus)
  - **Tinggi**: slider 30–240 px
  - **Warna**: color picker
  - Toggle cepat: tombol **Gelombang** di Settings → Effects
- **Full Text .lrc (mode Karaoke)** — semua baris lirik dari `.lrc` tampil sekaligus:
  - Baris aktif diberi **background abu-abu** yang **berpindah mulus** mengikuti waktu `[menit:detik:milidetik]` — transisi `transform`/warna halus, anti tiba-tiba
  - Tiap baris menampilkan timestamp `[mm:ss.ms]`; klik baris untuk lompat
  - Baris kosong (penanda jeda) tampil sebagai `· · ·`
  - Tombol cepat **Full Text** di pojok kanan atas, atau Settings → Effects
- Versi Lyric Player dinaikkan ke **v0.3.0**.

## Changelog v0.1.0

### Update 2 (bug hunt)

- **Lyric Player**: baris `.lrc` dengan timestamp tapi **teks kosong sekarang terdeteksi** (misal `[01:03.09]` tanpa teks). Baris kosong jadi *penanda jeda*: di waktu itu layar langsung kosong/hitam polos, persis seperti layar kosong di awal video sebelum lirik pertama. Bonus: lirik sebelumnya juga berhenti tepat di timestamp marker, tidak perlu menunggu auto-hide.
- **Lyric Player**: editor timing tidak lagi membuang baris kosong saat "Terapkan"; baris kosong ditampilkan dengan placeholder `(kosong — penanda jeda)`.
- **Lyric Player**: export subtitle SRT/ASS/VTT melewatkan baris kosong (tidak jadi cue kosong yang invalid), tapi tetap memakainya sebagai batas akhir cue sebelumnya.
- **Fix (Lyric Player)**: guard `currentIndex` basi setelah lirik diedit/dihapus (potensi crash seluruh app).
- **Fix (Lyric Player)**: download `.lrc` tidak lagi revoke blob URL secara instan (bisa membatalkan download di beberapa browser).
- **Fix (Video Editor)**: input **rate** clip sekarang divalidasi — field kosong/invalid tidak lagi membuat `NaN` yang merusak seluruh timeline; rate dibatasi 0.05–16x.
- **Fix (Video Editor)**: undo tidak lagi di-push saat drag baru dimulai tanpa pergeseran nyata.
- **Fix (Video Editor)**: export ditolak dengan pesan jelas kalau timeline masih kosong.
- **Fix (Video Editor)**: `EXPORT .SRT` membuang entry durasi 0/terbalik supaya file `.srt` tetap valid.

### Update 1

- **Pisah mode**: Lyric Player dan Video Edit sekarang file terpisah, tidak ada lagi switcher 2-mode.
- **Lyric Player**: hapus placeholder `"[Musik]"`; layar kosong/hitam polos saat tidak ada lirik.
- **Lyric Player**: setting baru *Auto-Hide Lirik* (default 5 detik, bisa diubah) — lirik hilang saat jeda panjang tanpa lanjutan; berlaku juga untuk export video.
- **Fix**: dead code v1.5/v1.6/v1.7 di `<head>` (inline script di dalam `<script src>` tidak pernah dieksekusi browser) dibersihkan.
- **Fix (Video Editor)**: undo tidak lagi menambah entry saat nudge clip/teks tidak benar-benar bergeser (misal nudge kiri saat posisi 0).
- **Fix (Video Editor)**: duplikat clip/teks tidak lagi push undo sebelum item dipastikan ada.
- **Fix**: branding & `versionPatch` lama (rewrite teks DOM tiap 4 detik) dihapus; nama file export/project sekarang `video-editor-*`.

## Lisensi

MIT — lihat [LICENSE](LICENSE).
