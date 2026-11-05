# File, Media, dan In-App Viewer

> **Status: DRAFT — BELUM FINAL — BELUM DISETUJUI.**

## 1. Tujuan

Gambar, video, audio/suara, dan file yang dikirim melalui PAIRO secara default dibuka atau diputar di dalam PAIRO. Attachment diterima sebagai ciphertext, lalu didekripsi seperlunya oleh viewer/player internal.

## 2. Jenis konten yang dipertimbangkan

| Kategori | Contoh | Pengalaman |
|---|---|---|
| Gambar | JPEG, PNG, WebP, GIF, HEIF/HEIC | Image Viewer |
| Video | MP4, WebM, format yang didukung decoder | Video Player |
| Audio | MP3, AAC, M4A, OGG, WAV, FLAC | Audio Player |
| Dokumen | PDF, TXT, format lain setelah evaluasi | Document Viewer |
| Arsip | ZIP dan format sejenis | Preview/ekstraksi terisolasi; belum diputuskan |
| File umum | APK, Office, font, binary | Informasi file; tidak otomatis dieksekusi |

Format dan batas ukuran masih perlu ditentukan.

## 3. Alur attachment

1. Pengirim memilih file melalui Android system picker.
2. PAIRO membaca file menggunakan akses URI yang diberikan Android.
3. PAIRO mengenkripsi file di perangkat sebelum transfer.
4. Relay, jika digunakan, hanya menerima ciphertext dan metadata minimum.
5. Penerima menyimpan ciphertext di internal app storage.
6. Saat dibuka, PAIRO mendekripsi konten seperlunya untuk viewer/player internal.
7. Plaintext sebisa mungkin hanya berada di memory atau cache privat sementara.
8. Cache sementara dibersihkan setelah ditutup sesuai lifecycle dan batasan Android.

## 4. Kebijakan akses dan ekspor

Prinsip yang disetujui pengguna: **in-app by default, export by user choice**.

- Secara default attachment dibuka di viewer/player internal dan tidak otomatis diekspor.
- Pengguna boleh memilih Save to Gallery, Save to Device, Export, atau Share untuk attachment biasa.
- Ekspor merupakan tindakan eksplisit pengguna, misalnya melalui menu attachment.
- Ketika ekspor dilakukan, PAIRO mendekripsi file lalu menyerahkan salinan plaintext ke tujuan yang dipilih.
- UI menjelaskan bahwa setelah diekspor, file berada di luar kontrol penyimpanan privat PAIRO dan dapat diakses aplikasi lain sesuai lokasi dan izin Android.
- Pengguna tidak perlu mengekspor file hanya untuk melihatnya; viewer internal tetap menjadi jalur utama.

Kontrol teknis yang diusulkan:
- Jangan mengekspor attachment hasil dekripsi ke shared storage secara otomatis.
- Buat share URI/FileProvider hanya ketika pengguna secara eksplisit memilih Share.
- Jangan otomatis membuka attachment privat melalui aplikasi lain.
- Simpan ciphertext dan database di internal app storage.
- Hindari plaintext pada thumbnail cache, notification preview, recent-task snapshot, clipboard, log, dan crash report.
- Validasi ukuran, signature/magic bytes, dan input decoder. Ekstensi atau MIME type saja bukan bukti file aman.

## 5. View Once

Pengguna menyetujui View Once sebagai kebutuhan produk. Media yang dikirim dengan opsi ini ditujukan untuk dibuka satu kali oleh penerima melalui viewer internal PAIRO.

Perilaku produk yang diusulkan:
- Pengirim memilih opsi View Once sebelum mengirim attachment.
- Penerima melihat indikator bahwa attachment adalah View Once.
- Setelah konten dibuka dan status viewed tercatat, PAIRO tidak menyediakan pembukaan ulang dari chat.
- View Once tidak dapat diekspor atau disimpan ke Gallery melalui fitur ekspor PAIRO.
- PAIRO dapat menerapkan Android FLAG_SECURE ketika konten ditampilkan sebagai mitigasi screenshot/screen recording yang didukung Android.
- Status viewed harus disinkronkan agar redelivery tidak mengembalikan kemampuan membuka ulang.

Detail teknis yang masih terbuka:
- Kapan view dihitung: saat dibuka atau setelah konten selesai dimuat.
- Apakah video boleh diputar ulang dalam sesi yang sama.
- Batas waktu penerima untuk membuka media.
- Perilaku saat aplikasi crash, force-stop, atau perangkat offline.
- Penanganan multi-device dan sinkronisasi status viewed.

View Once tidak dapat mencegah penerima memotret layar dengan perangkat lain atau merekam konten dari endpoint yang dikompromikan.

## 6. Batasan keamanan

“In-app only” bukan format file ajaib yang membuat file mustahil dibuka di tempat lain. Ini adalah kebijakan aplikasi: secara default PAIRO tidak menyimpan plaintext di folder publik dan tidak menyerahkan file plaintext ke aplikasi lain.

Jika pengguna mengekspor atau membagikan attachment biasa, salinan hasil dekripsi dapat dibuka aplikasi lain. Penerima juga dapat merekam layar dengan perangkat lain. OS yang dikompromikan dapat mengakses data yang sedang diproses.

Tidak ada klaim bahwa screenshot, perekaman, atau penyalinan dapat dicegah 100%.

## 7. Acceptance criteria draft

- Attachment tidak muncul sebagai plaintext di Gallery/Downloads secara default kecuali pengguna memilih Save/Export.
- Pengguna dapat menyimpan attachment biasa ke Gallery/perangkat atau membagikannya melalui tindakan eksplisit.
- Attachment biasa tidak otomatis membuka aplikasi pihak ketiga.
- Attachment View Once tidak dapat diekspor melalui fitur PAIRO dan tidak dapat dibuka ulang setelah status viewed tercatat.
- Format yang didukung dapat dibuka melalui viewer/player internal.
- Relay hanya menerima ciphertext attachment.
- Tidak ada plaintext attachment atau encryption key di log.
- Temporary plaintext/cache mengikuti kebijakan pembersihan.
- Pengujian mencakup beberapa versi Android, force-stop, crash, reboot, dan kondisi low storage.
