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
| [SECURITY.md](SECURITY.md) | Sasaran keamanan, key management, batas klaim |
| [STORAGE_AND_BACKUP.md](STORAGE_AND_BACKUP.md) | Database lokal dan personal cloud backup |
| [THREAT_MODEL.md](THREAT_MODEL.md) | Aset, aktor ancaman, trust boundary, mitigasi |
| [DECISIONS.md](DECISIONS.md) | Keputusan awal dan hal yang belum diputuskan |
| [ROADMAP.md](ROADMAP.md) | Fase pengembangan dan gerbang penyelesaian |
| [GLOSSARY.md](GLOSSARY.md) | Glosarium teknis |

## Cara membaca status

- **Hasil pembicaraan:** arah yang secara eksplisit disebut dalam percakapan.
- **Usulan:** rekomendasi teknis yang belum disetujui.
- **Terbuka:** memerlukan keputusan lanjutan.
- **Draft:** seluruh spesifikasi masih dapat berubah.

## Aturan dokumentasi

1. Jangan mengubah usulan menjadi keputusan final tanpa persetujuan pemilik proyek.
2. Setiap perubahan arsitektur harus memperbarui dokumen terkait.
3. Jangan membuat klaim keamanan yang belum dibuktikan melalui implementasi, pengujian, dan review independen.
4. Harga, kuota, lisensi, serta dukungan provider harus diverifikasi kembali sebelum implementasi.
