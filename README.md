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

## Changelog v0.1.0

- **Pisah mode**: Lyric Player dan Video Edit sekarang file terpisah, tidak ada lagi switcher 2-mode.
- **Lyric Player**: hapus placeholder `"[Musik]"`; layar kosong/hitam polos saat tidak ada lirik.
- **Lyric Player**: setting baru *Auto-Hide Lirik* (default 5 detik, bisa diubah) — lirik hilang saat jeda panjang tanpa lanjutan; berlaku juga untuk export video.
- **Fix**: dead code v1.5/v1.6/v1.7 di `<head>` (inline script di dalam `<script src>` tidak pernah dieksekusi browser) dibersihkan.
- **Fix (Video Editor)**: undo tidak lagi menambah entry saat nudge clip/teks tidak benar-benar bergeser (misal nudge kiri saat posisi 0).
- **Fix (Video Editor)**: duplikat clip/teks tidak lagi push undo sebelum item dipastikan ada.
- **Fix**: branding & `versionPatch` lama (rewrite teks DOM tiap 4 detik) dihapus; nama file export/project sekarang `video-editor-*`.

## Lisensi

MIT — lihat [LICENSE](LICENSE).
