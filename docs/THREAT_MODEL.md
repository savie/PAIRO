# Threat Model PAIRO

> **Status: DRAFT — BELUM FINAL — BELUM DISETUJUI.**

## 1. Tujuan

Mendokumentasikan aset, trust boundary, aktor ancaman, dan risiko awal agar desain PAIRO tidak menyamakan E2EE dengan anonimitas atau keamanan endpoint.

## 2. Aset yang dilindungi

| Aset | Contoh |
|---|---|
| Isi pesan | Teks, metadata sensitif di dalam pesan |
| Attachment | Foto, video, voice note, dokumen |
| Identity private key | Kunci identitas dan material sesi |
| Backup recovery secret | Kunci/passphrase pemulihan backup |
| Database lokal | Riwayat pesan dan status |
| Pairing state | Hubungan dua identity |
| Metadata | IP, waktu, ukuran, routing token |

## 3. Trust boundary

1. Perangkat pengirim.
2. Perangkat penerima.
3. Jaringan publik.
4. Signaling service.
5. Temporary ciphertext relay.
6. Push provider.
7. Personal cloud provider.

Endpoint adalah pihak yang dipercaya untuk melihat plaintext. Server relay, signaling, push, dan cloud backup tidak boleh menerima plaintext atau private key.

## 4. Aktor ancaman

- Penyerang jaringan.
- Pihak yang memperoleh akses ke relay.
- Operator infrastruktur yang ingin mengakses data.
- Penyerang yang mencoba menyamar saat pairing.
- Orang yang memperoleh akses fisik ke perangkat.
- Malware pada endpoint.
- Penerima yang menyimpan atau membagikan ulang konten.
- Penyerang yang mencoba mengirim spam atau memenuhi kapasitas relay.

## 5. Risiko dan mitigasi awal

| Risiko | Mitigasi draft | Residual risk |
|---|---|---|
| Relay bocor | Ciphertext only, key tidak ada di server | Metadata tetap mungkin terekspos |
| MITM saat pairing | Mutual verification fingerprint | Pengguna dapat melewatkan verifikasi |
| Replay message | Message ID, nonce/counter protokol, deduplication | Bug implementasi |
| Duplicate delivery | Receiver idempotency | UI/status dapat terlambat |
| Perangkat dicuri | Device lock, encrypted DB, Keystore | Perangkat terbuka/kompromi |
| Backup dicuri | Client-side encryption dan recovery secret | Password lemah atau secret bocor |
| Metadata correlation | Minimasi log dan routing opaque | IP/timing masih dapat terlihat |
| Relay abuse | Authentication, quota, rate limit, size cap | DoS tetap mungkin |
| Key loss | Recovery design dan backup | Recovery dapat gagal jika secret hilang |

## 6. Di luar cakupan perlindungan

PAIRO tidak dapat menjamin:
- Anonimitas jaringan penuh.
- Perlindungan terhadap perangkat yang sudah dikuasai malware.
- Pencegahan total screenshot, kamera eksternal, atau penyalinan manual.
- Penghapusan salinan yang sudah diterima pihak lain.
- Ketersediaan layanan relay tanpa gangguan.

## 7. Validasi sebelum rilis

Threat model ini harus ditinjau ulang setelah protokol, library, server, format envelope, dan model recovery dipilih. Lakukan security review independen sebelum membuat klaim publik tentang tingkat keamanan.
