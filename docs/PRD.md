# Product Requirements Document (PRD)

> **Status: DRAFT — BELUM FINAL — BELUM DISETUJUI.**

## 1. Tujuan

Mendefinisikan kebutuhan produk awal PAIRO: aplikasi Android untuk komunikasi privat antara dua identity yang telah melakukan pairing dan mutual verification.

## 2. Pengguna dan mode

### Standard Mode
Pengguna umum dengan satu identity dan maksimal satu pairing aktif.

### Owner Mode
Pemilik proyek dapat membuat beberapa identity independen pada satu instalasi. Owner tidak memperoleh akses untuk membaca percakapan identity lain atau pengguna lain.

## 3. Kebutuhan fungsional

| ID | Kebutuhan | Prioritas draft | Acceptance criteria draft |
|---|---|---:|---|
| FR-001 | Membuat identity lokal | P0 | Key material dibuat pada perangkat; private key tidak dikirim ke server |
| FR-002 | Pairing melalui QR/invitation | P0 | Undangan acak, sekali pakai, dan memiliki masa berlaku |
| FR-003 | Mutual consent | P0 | Pairing tidak aktif sebelum kedua pihak menyetujui |
| FR-004 | Identity verification | P0 | Kedua pihak dapat membandingkan fingerprint melalui kanal tepercaya |
| FR-005 | Pesan teks E2EE | P0 | Relay hanya menerima ciphertext |
| FR-006 | Local chat history | P0 | Pesan diterima disimpan di database lokal terenkripsi |
| FR-007 | Online delivery | P0 | Pesan dapat dikirim saat jalur langsung/relay tersedia |
| FR-008 | Offline delivery | P0 | Pesan dapat dititipkan sebagai ciphertext untuk penerima offline |
| FR-009 | Relay expiry | P0 | Pesan kedaluwarsa paling lambat 168 jam sejak diterima relay |
| FR-010 | Acknowledgment | P0 | Relay menghapus salinan setelah ACK penerima tervalidasi |
| FR-011 | Personal cloud backup | P1 | Backup dienkripsi di perangkat sebelum diunggah |
| FR-012 | Restore | P1 | Restore membutuhkan recovery material yang benar |
| FR-013 | Owner multi-identity | P1 | Identity terisolasi dan dapat dipilih secara eksplisit |
| FR-014 | Standard single-pair | P0 | Standard identity tidak dapat memiliki dua pairing aktif |
| FR-015 | Attachment | P1 | File dienkripsi sebelum transfer atau upload |
| FR-016 | Disappearing messages | P2 | Kebijakan penghapusan lokal dan relay dijelaskan terpisah |
| FR-017 | Device recovery | P1 | Penggantian perangkat memiliki prosedur identity recovery yang aman |

## 4. Kebutuhan nonfungsional

- Tidak menyimpan plaintext pesan di server.
- Tidak mengirim private key ke backend.
- Semua komunikasi client-server menggunakan TLS.
- Database lokal dan attachment dilindungi dengan enkripsi at-rest.
- Key lokal dilindungi menggunakan Android Keystore bila sesuai.
- Gunakan protokol kriptografi yang sudah ditinjau dan implementasi library yang dipelihara.
- Minimalkan metadata yang dibutuhkan untuk routing.
- Tangani retry, duplicate delivery, out-of-order delivery, crash, dan koneksi terputus.
- Tidak ada analytics yang mengumpulkan isi pesan atau secret.
- Dependency, lisensi, dan kebijakan provider harus diperiksa sebelum dipilih.

## 5. Prinsip UX

- Pairing menjelaskan bahwa kedua pihak harus menyetujui.
- Fingerprint/safety number dapat diakses dari profil percakapan.
- Perubahan identity key ditampilkan sebagai peristiwa keamanan.
- Status pesan membedakan tersimpan lokal, mengirim, menunggu penerima, diterima, gagal, dan kedaluwarsa.
- Pengguna mengetahui bahwa expiry relay tidak menghapus salinan lokal penerima.
- Backup tidak aktif tanpa persetujuan; recovery secret dijelaskan saat setup.

## 6. Di luar MVP

Grup, channel, panggilan, public discovery, web/desktop client, sinkronisasi multi-device, dan fitur sosial.

## 7. Draft metrik keberhasilan

- Pairing hanya aktif setelah mutual consent.
- Identity mismatch ditolak atau meminta verifikasi ulang.
- Pesan dapat dikirim di antara dua perangkat Android.
- Uji offline delivery, retry, duplicate, dan expiry lulus.
- Tidak ada plaintext/private key dalam log atau payload relay.
- Tidak ada kebocoran data antaridentity.
- Backup valid dapat dipulihkan dan backup dengan recovery key salah ditolak.

## 8. Tambahan kebutuhan attachment dan media privat (draft)

| ID | Kebutuhan | Prioritas draft | Acceptance criteria draft |
|---|---|---:|---|
| FR-018 | Private in-app media | P1 | Gambar, video, audio, dan format file yang didukung dibuka melalui viewer/player internal secara default |
| FR-019 | No automatic external opening | P1 | Attachment privat tidak otomatis diekspor ke shared storage atau dibuka aplikasi pihak ketiga |
| FR-020 | User-controlled export/share | P1 | Pengguna dapat secara eksplisit menyimpan attachment biasa ke Gallery/perangkat atau membagikannya; tidak ada ekspor otomatis |
| FR-021 | View Once media | P1 | Pengirim dapat menandai media View Once; penerima hanya dapat membukanya sekali melalui viewer internal sesuai aturan sesi yang ditetapkan |

Tambahan NFR:
- Attachment hasil dekripsi tidak disimpan ke shared/public storage secara default; pengguna boleh memilih ekspor/simpan secara eksplisit.
- Attachment View Once tidak tersedia untuk ekspor melalui fitur PAIRO dan tidak dapat dibuka ulang setelah status viewed tercatat.
- Plaintext attachment dibatasi pada memory atau cache privat sementara sejauh memungkinkan.
- File yang tidak didukung tidak boleh otomatis dibuka aplikasi eksternal.
- Format/codec yang didukung dan batas ukuran file harus ditentukan.

Prinsip UX tambahan: attachment yang didukung dibuka di dalam PAIRO. Export atau pembagian keluar PAIRO, jika tersedia, harus menjadi tindakan eksplisit. Lihat [FILES_AND_MEDIA.md](FILES_AND_MEDIA.md).
