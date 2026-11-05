# Dokumentasi PAIRO

> **STATUS: DRAFT — BELUM FINAL — BELUM DISETUJUI PEMILIK PROYEK.**
>
> Dokumen ini merekam hasil pembicaraan awal dan usulan teknis. Keberadaan suatu keputusan di dokumen tidak berarti keputusan tersebut sudah final.

## Peta dokumentasi

| Dokumen | Cakupan |
|---|---|
| [PROJECT_OVERVIEW.md](PROJECT_OVERVIEW.md) | Visi, prinsip, ruang lingkup, istilah produk |
| [PRD.md](PRD.md) | Kebutuhan produk dan acceptance criteria |
| [ARCHITECTURE.md](ARCHITECTURE.md) | Arsitektur Android, backend minimum, alur data |
| [IDENTITY_AND_PAIRING.md](IDENTITY_AND_PAIRING.md) | Identity, Standard/Owner Mode, pairing |
| [MESSAGING_AND_CONNECTIVITY.md](MESSAGING_AND_CONNECTIVITY.md) | Pesan, WebRTC, relay, offline delivery |
| [FILES_AND_MEDIA.md](FILES_AND_MEDIA.md) | Attachment E2EE, in-app viewer/player, ekspor atas pilihan pengguna, dan View Once |
| [SECURITY.md](SECURITY.md) | Sasaran keamanan, key management, batas klaim |
| [STORAGE_AND_BACKUP.md](STORAGE_AND_BACKUP.md) | Database lokal dan personal cloud backup |
| [THREAT_MODEL.md](THREAT_MODEL.md) | Aset, aktor ancaman, trust boundary, mitigasi |
| [DECISIONS.md](DECISIONS.md) | Keputusan awal dan hal yang belum diputuskan |
| [ROADMAP.md](ROADMAP.md) | Fase pengembangan dan gerbang penyelesaian |
| [GLOSSARY.md](GLOSSARY.md) | Glosarium teknis |

## Sumber kebenaran keputusan

[DECISIONS.md](DECISIONS.md) adalah satu-satunya sumber status keputusan dan daftar hal terbuka.

- Jangan mengulang pertanyaan yang sudah tercatat sebagai keputusan.
- Jangan membuat daftar keputusan/hal terbuka baru di dokumen domain.
- Dokumen domain hanya menjelaskan detail sesuai cakupannya.
- Target Android 8–17 sudah ditetapkan; pengujian kompatibilitas adalah pekerjaan QA, bukan keputusan produk terbuka.

## Aturan dokumentasi

1. Catat keputusan baru terlebih dahulu di [DECISIONS.md](DECISIONS.md).
2. Perbarui hanya dokumen domain yang terdampak; jangan menyalin ulang seluruh register keputusan.
3. Jangan mengubah usulan menjadi keputusan tanpa persetujuan pemilik proyek.
4. Jangan membuat klaim keamanan yang belum dibuktikan melalui implementasi, pengujian, dan review independen.
5. Harga, kuota, lisensi, serta dukungan provider harus diverifikasi kembali sebelum implementasi.

Tambahan dokumentasi: [FILES_AND_MEDIA.md](FILES_AND_MEDIA.md) menjelaskan attachment E2EE, private in-app viewer/player, pembatasan akses aplikasi lain, dan batas klaim keamanan.
