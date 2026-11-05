# PAIRO

**Two Identities. One Private Space.**

PAIRO adalah konsep aplikasi messenger Android yang dirancang untuk komunikasi privat antara dua identity yang telah saling menyetujui pairing.

> **Status seluruh dokumen: DRAFT — BELUM FINAL — BELUM DISETUJUI.**
>
> Nama **PAIRO** telah dipilih. Semua keputusan produk dan teknis lainnya masih berupa hasil pembicaraan awal, usulan, atau asumsi yang perlu ditinjau dan disetujui pemilik proyek.

## Arah produk

- Android native dengan Kotlin.
- Satu APK untuk semua pengguna.
- Satu ruang percakapan hanya memiliki dua identity.
- E2EE untuk pesan dan attachment.
- Riwayat chat disimpan secara lokal pada perangkat.
- Tidak ada cloud database untuk riwayat chat permanen.
- Fitur backup ke cloud pribadi tersedia, tetapi pengguna bebas mengaktifkannya atau tidak.
- Offline delivery menggunakan temporary encrypted relay dengan batas retensi rancangan tujuh hari.
- Standard Mode dibatasi satu pairing aktif; Owner Mode dapat mengelola beberapa identity independen.
- Developer tidak memiliki master key untuk membaca percakapan pengguna.

## Dokumentasi

Mulai dari [docs/README.md](docs/README.md). Semua dokumen ditulis dalam Bahasa Indonesia; istilah teknis dan canonical names dipertahankan dalam bahasa Inggris.

## Status implementasi

Repository ini masih berada pada tahap dokumentasi dan perencanaan. Belum ada aplikasi Android, backend, protokol produksi, maupun audit keamanan yang dapat diklaim selesai.

## Lisensi

Belum ditentukan.
