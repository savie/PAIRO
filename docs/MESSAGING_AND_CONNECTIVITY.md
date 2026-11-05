# Pesan dan Konektivitas

> **Status: DRAFT — BELUM FINAL — BELUM DISETUJUI.**

## 1. Tujuan

Mendukung pesan online langsung dan pesan tertunda untuk penerima offline tanpa menyimpan riwayat percakapan permanen di cloud.

## 2. Dua jalur transport

### Direct P2P
WebRTC DataChannel digunakan saat kedua endpoint tersedia dan koneksi berhasil dibangun. Signaling tetap diperlukan untuk pertukaran informasi setup. STUN/TURN membantu konektivitas.

### Temporary store-and-forward
Jika penerima offline atau direct path tidak tersedia, sender mengunggah envelope ciphertext ke temporary relay. Relay hanya menyimpan ciphertext dan metadata routing minimum.

## 3. Envelope konseptual

Contoh ini bukan format wire final dan belum boleh dianggap sebagai spesifikasi implementasi:

```json
{
  "messageId": "random-unique-id",
  "routeToken": "opaque-routing-token",
  "createdAt": "server-or-client-timestamp",
  "expiresAt": "server-enforced-expiry",
  "protocolVersion": 1,
  "ciphertext": "binary-ciphertext"
}
```

Format final harus mempertimbangkan autentikasi, replay protection, ukuran maksimum, versi protokol, dan privasi metadata.

## 4. Retensi tujuh hari

Hasil pembicaraan awal menetapkan tujuh hari sebagai nilai rancangan:
- TTL maksimum: 168 jam sejak relay menerima pesan.
- Waktu expiry ditegakkan menggunakan waktu server.
- Relay menghapus salinan setelah ACK penerima yang terautentikasi atau saat expiry.
- Cleanup berkala harus menangani pesan yang melewati expiry.
- Redelivery mungkin terjadi; receiver harus idempotent.
- Penghapusan relay tidak menghapus salinan lokal yang telah diterima.
- Pesan yang expired tidak dapat dipulihkan dari relay.

Apakah pengguna boleh mengatur TTL lebih pendek masih terbuka. Batas maksimum tujuh hari adalah usulan dasar.

## 5. Acknowledgment dan status

ACK harus terikat pada message ID dan identity penerima. ACK tidak boleh dapat dipalsukan hanya dengan mengetahui ID pesan.

Status UI yang diusulkan:
- Saved locally
- Sending
- Waiting for recipient
- Delivered to device
- Expired
- Failed

Upload berhasil ke relay hanya berarti relay menerima ciphertext, bukan berarti pesan sudah delivered.

## 6. Push notification

Firebase Cloud Messaging dapat dievaluasi sebagai wake-up hint. Payload push tidak boleh berisi plaintext, attachment, atau secret key. Push dapat tertunda/tidak terkirim; receiver tetap perlu melakukan sync ke relay.

## 7. Attachment

Attachment dienkripsi pada perangkat sebelum transfer/upload. Kunci attachment dikirim sebagai bagian dari pesan E2EE, bukan metadata relay. Batas ukuran, chunking, resume, dan retensi perlu dirancang sebelum implementasi media.

## 8. Batasan

- Direct P2P tidak selalu berhasil.
- TURN adalah relay trafik real-time, bukan antrean pesan offline.
- Store-and-forward membutuhkan layanan yang online.
- Pengiriman tidak dijamin jika sender tidak berhasil mengunggah pesan atau penerima tidak mengambilnya dalam masa retensi.
- Perilaku saat sender offline sebelum upload berhasil perlu ditangani oleh outbox lokal.
