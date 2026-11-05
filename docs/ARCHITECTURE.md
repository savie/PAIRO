# Software Architecture Document

> **Status: DRAFT — BELUM FINAL — BELUM DISETUJUI.**

## 1. Ringkasan

PAIRO dirancang sebagai aplikasi Android native dengan local-first storage. Transport dapat memakai direct peer-to-peer ketika kedua perangkat tersedia, dan temporary store-and-forward relay ketika penerima offline.

Tidak ada cloud database untuk riwayat chat permanen. Namun, offline delivery memerlukan layanan relay yang menyimpan ciphertext sementara. Artinya, “tanpa cloud chat database” tidak sama dengan “tanpa infrastruktur server sama sekali”.

## 2. Diagram konseptual

```text
ANDROID A                                      ANDROID B
+-------------------------+                    +-------------------------+
| Compose UI / Domain     |                    | Compose UI / Domain     |
| Identity + Pairing      |                    | Identity + Pairing      |
| E2EE Session            |                    | E2EE Session            |
| Encrypted Local DB      |                    | Encrypted Local DB      |
| Transport Manager       |                    | Transport Manager       |
+------------+------------+                    +------------+------------+
             |                                              |
             +----------- WebRTC DataChannel ---------------+
             |                                              |
             +---- TLS ---- Temporary Ciphertext Relay -----+
                                    |
                             ciphertext only
                             TTL <= 168 hours
```

## 3. Komponen Android (usulan)

- **Presentation:** Kotlin + Jetpack Compose.
- **Domain:** use cases, entities, repository interfaces.
- **Data:** SQLite/Room dengan solusi enkripsi database yang kompatibel dan terpelihara.
- **Crypto adapter:** integrasi ke protokol E2EE yang dipilih.
- **Key protection:** Android Keystore untuk melindungi key enkripsi lokal/wrapping key.
- **Transport:** WebRTC DataChannel dan HTTPS relay.
- **Backup:** snapshot konsisten, encrypted archive, dan provider adapter.

Pemilihan library final belum ditetapkan.

## 4. Komponen infrastruktur

1. **Signaling service:** menukar informasi setup WebRTC. Bukan tempat menyimpan isi chat.
2. **STUN:** membantu endpoint menemukan jalur koneksi.
3. **TURN:** merelay trafik real-time saat koneksi langsung gagal. TURN tidak otomatis menyimpan pesan untuk penerima offline.
4. **Temporary message relay:** store-and-forward ciphertext dengan TTL maksimal 168 jam.
5. **Push notification gateway (opsional):** mengirim wake-up hint tanpa plaintext atau key.

Komponen tersebut dapat memiliki biaya operasional. Hosting dan implementasi belum diputuskan.

## 5. WebRTC bukan aplikasi web

WebRTC dapat digunakan dari aplikasi Android native melalui library Android. PAIRO tidak perlu dibuat sebagai website. WebRTC DataChannel adalah kanal komunikasi data, bukan database dan bukan message queue offline.

## 6. Alur pesan online

1. Sender membuat pesan.
2. E2EE session mengenkripsi payload.
3. Client menyiapkan koneksi peer melalui signaling.
4. Jika koneksi direct tersedia, ciphertext dikirim melalui DataChannel.
5. Receiver memverifikasi, mendekripsi, lalu menyimpan pesan ke database lokal.
6. Receiver mengirim ACK terautentikasi.

## 7. Alur pesan offline

1. Sender mengenkripsi pesan.
2. Jika peer tidak tersedia, sender mengirim envelope ciphertext ke temporary relay.
3. Relay menyimpan envelope dengan waktu expiry server.
4. Receiver online dan mengambil antrean.
5. Receiver memverifikasi dan mendekripsi pesan.
6. Receiver menyimpan pesan secara lokal.
7. Receiver mengirim ACK terautentikasi.
8. Relay menghapus salinan setelah ACK valid; jika tidak, pesan kedaluwarsa paling lambat 168 jam.

## 8. Konsistensi dan retry

- Setiap pesan memiliki ID acak unik.
- Receiver harus idempotent terhadap redelivery.
- ACK terikat secara kriptografis ke message ID dan identity penerima.
- Retry memakai backoff dan batas yang ditentukan.
- Upload sukses bukan status delivered.
- Penghapusan setelah ACK harus mempertimbangkan crash dan retry.
- Expiry dihitung berdasarkan waktu server relay, bukan jam perangkat pengirim.

## 9. Batasan

- P2P tidak selalu berhasil karena NAT, firewall, jaringan seluler, atau kebijakan perangkat.
- TURN dan relay membutuhkan bandwidth serta biaya.
- Offline delivery membutuhkan relay yang tetap tersedia.
- FCM bukan durable message queue.
- Pesan tidak dapat dijamin terkirim jika sender tidak pernah berhasil mengunggahnya atau penerima tidak mengambilnya sebelum expiry.

## 10. Keputusan teknis yang masih terbuka

Library Signal Protocol, implementasi WebRTC, enkripsi database, backend signaling, relay hosting, TURN provider, minimum Android API, dan mekanisme recovery harus dipilih setelah evaluasi kompatibilitas, lisensi, pemeliharaan, keamanan, serta biaya.
