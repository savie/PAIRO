# Roadmap Pengembangan

> **Status: DRAFT — BELUM FINAL — BELUM DISETUJUI.**

Roadmap ini merupakan urutan kerja yang diusulkan, bukan komitmen jadwal atau persetujuan implementasi.

## 1. Tahapan

| Fase | Fokus | Gerbang penyelesaian draft |
|---|---|---|
| 0 | Foundation: Gradle, package, struktur modul, CI | Debug build berhasil |
| 1 | Local identity dan key lifecycle | Identity isolation tests lulus |
| 2 | Pairing, consent, fingerprint | Identity mismatch ditolak |
| 3 | E2EE text messaging dan local DB | Test vectors dan persistence lulus |
| 4 | WebRTC online transport + signaling | Dua perangkat bertukar pesan |
| 5 | Temporary relay, ACK, TTL 168 jam | Offline, duplicate, retry, expiry tests lulus |
| 6 | Encrypted backup dan provider pertama | Wrong-key ditolak; restore valid berhasil |
| 7 | Encrypted attachments | Tidak ada plaintext attachment di server |
| 8 | Security hardening dan review | Temuan kritis ditangani |
| 9 | Release candidate | QA perangkat nyata dan checklist rilis |

## 2. Usulan MVP

MVP awal:
- Android native.
- Identity lokal.
- Pairing mutual approval.
- Fingerprint verification.
- Pesan teks E2EE.
- Encrypted local database.
- Direct online transport.
- Temporary ciphertext relay untuk offline delivery.
- Retensi relay maksimum 168 jam.

Backup cloud harus dirancang sejak awal, walau integrasi provider dapat dikerjakan setelah jalur pesan inti stabil.

## 3. Risiko proyek

- P2P tidak selalu berhasil pada semua jaringan.
- TURN dan relay dapat menimbulkan biaya bandwidth.
- Offline delivery membutuhkan service yang selalu tersedia.
- Recovery yang salah dapat melemahkan E2EE.
- Library kriptografi dapat berubah atau tidak lagi dipelihara.
- Provider cloud dapat membatasi API, kuota, ukuran, dan scope akses.

## 4. Definition of Done

Sebuah fitur dianggap selesai jika:
- Perilakunya sesuai PRD yang disetujui.
- Unit/instrumentation tests relevan lulus.
- Tidak ada plaintext atau secret di log.
- Retry dan error state ditangani.
- Dampak privasi didokumentasikan.
- Perubahan arsitektur tercermin pada dokumen.
- Tidak ada klaim keamanan yang melampaui bukti pengujian.
