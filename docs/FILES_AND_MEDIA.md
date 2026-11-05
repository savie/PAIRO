# File, Media, dan In-App Viewer

> **Status: DRAFT — BELUM FINAL — BELUM DISETUJUI.**

## 1. Tujuan

Pengguna ingin gambar, video, audio/suara, dan file yang dikirim melalui PAIRO dibuka atau diputar di dalam PAIRO, bukan otomatis diserahkan ke Gallery, Photos, pemutar media, file manager, atau aplikasi pihak ketiga.

Arah yang diusulkan adalah **private in-app media**: attachment diterima sebagai ciphertext, didekripsi hanya ketika diperlukan, lalu dirender oleh viewer/player internal PAIRO.

## 2. Jenis konten yang dipertimbangkan

| Kategori | Contoh | Pengalaman yang diusulkan |
|---|---|---|
| Gambar | JPEG, PNG, WebP, GIF, HEIF/HEIC | Image viewer internal |
| Video | MP4, WebM, format lain yang didukung decoder | Video player internal |
| Audio | MP3, AAC, M4A, OGG, WAV, FLAC | Audio player internal |
| Dokumen | PDF, TXT, dokumen lain setelah evaluasi | Document viewer atau preview internal |
| Arsip | ZIP dan format sejenis | Preview/ekstraksi terisolasi; belum diputuskan |
| File umum | APK, Office, font, binary lain | Ditampilkan sebagai file; tidak otomatis dieksekusi atau dibuka eksternal |

Daftar format adalah kandidat, bukan janji dukungan. Codec, ukuran maksimum, dan batas pemrosesan perlu ditentukan.

## 3. Alur yang diusulkan

1. Pengirim memilih file melalui Android system picker.
2. PAIRO membaca file menggunakan akses URI yang diberikan Android.
3. PAIRO mengenkripsi file di perangkat sebelum transfer.
4. Relay, jika digunakan, hanya menerima ciphertext dan metadata minimum.
5. Penerima menyimpan ciphertext di internal app storage.
6. Saat dibuka, PAIRO mendekripsi konten seperlunya untuk viewer/player internal.
7. Plaintext sebisa mungkin hanya berada di memory atau cache privat sementara.
8. Cache sementara dibersihkan setelah ditutup sesuai lifecycle dan batasan Android.

## 4. Pembatasan akses aplikasi lain

Usulan kontrol:
- Jangan mengekspor attachment hasil dekripsi ke shared storage secara otomatis.
- Jangan membuat share URI atau FileProvider untuk attachment privat kecuali pengguna secara eksplisit memilih Export/Share.
- Jangan menggunakan implicit ACTION_VIEW untuk membuka attachment privat di aplikasi lain.
- Simpan ciphertext dan database di internal app storage, bukan direktori publik.
- Gunakan internal viewer/player untuk format yang didukung.
- Pertimbangkan Android FLAG_SECURE pada layar sensitif sebagai mitigasi screenshot/screen recording; bukan jaminan absolut.
- Hindari plaintext pada thumbnail cache, notification preview, recent-task snapshot, clipboard, log, dan crash report.
- Audit exported activities, services, receivers, dan providers.
- Validasi ukuran, signature/magic bytes, dan input decoder. Ekstensi atau MIME type saja bukan bukti file aman.

## 5. Batasan yang perlu dipahami

“In-app only” bukan format file ajaib yang membuat file mustahil dibuka di tempat lain. Ini adalah kebijakan aplikasi: secara default PAIRO tidak menyimpan plaintext di folder publik dan tidak menyerahkan file plaintext ke aplikasi lain.

Jika pengguna mengekspor atau membagikan file, salinan hasil dekripsi dapat dibuka aplikasi lain. Penerima juga dapat memotret layar dengan perangkat lain atau merekam konten melalui perangkat yang telah dikompromikan. OS yang dikompromikan dapat mengakses data yang sedang diproses.

Klaim realistis:
- Attachment tidak muncul sebagai plaintext di Gallery/Downloads secara default.
- Konten yang didukung ditampilkan melalui viewer/player internal.
- Export/Share keluar PAIRO, jika tersedia, memerlukan tindakan eksplisit.
- Tidak ada klaim bahwa screenshot, perekaman, atau penyalinan dapat dicegah 100%.

## 6. Keputusan yang masih terbuka

- Apakah Export/Share diizinkan, dinonaktifkan, atau memerlukan PIN/konfirmasi?
- Apakah file dapat diunduh untuk offline viewing?
- Apakah attachment memiliki expiry, disappearing media, atau view-once?
- Apakah dokumen Office dan arsip didukung atau hanya preview?
- Batas ukuran, durasi video, resolusi, dan jumlah attachment.
- Kebijakan thumbnail/cache dan backup attachment.
- Apakah audio dapat diputar di background?

## 7. Acceptance criteria draft

- Attachment tidak muncul sebagai plaintext di Gallery atau Downloads secara default.
- Attachment privat tidak otomatis membuka aplikasi pihak ketiga.
- Format yang didukung dapat dibuka melalui viewer/player internal.
- Relay hanya menerima ciphertext attachment.
- Tidak ada plaintext attachment atau encryption key di log.
- Temporary plaintext/cache mengikuti kebijakan pembersihan.
- Export/Share, jika disediakan, membutuhkan tindakan eksplisit.
- Pengujian mencakup beberapa versi Android, force-stop, crash, reboot, dan kondisi low storage.
