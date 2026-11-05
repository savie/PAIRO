# Ringkasan Proyek PAIRO

> **Status: DRAFT — BELUM FINAL — BELUM DISETUJUI.**

## 1. Identitas produk

| Atribut | Nilai |
|---|---|
| Nama | PAIRO |
| Kategori | Private one-to-one messenger |
| Platform awal | Android |
| Bahasa implementasi | Kotlin |
| UI | Jetpack Compose (usulan) |
| Tahap | Konsep dan dokumentasi awal |
| Tagline kerja | Two Identities. One Private Space. |

Nama PAIRO telah dipilih dalam pembicaraan. Tagline, stack, dan rincian teknis lainnya masih berupa draft.

## 2. Visi

Membangun satu aplikasi Android yang memungkinkan dua identity berkomunikasi dalam satu ruang privat setelah keduanya melakukan pairing dan verifikasi. Isi percakapan dirancang hanya dapat dibaca oleh kedua endpoint yang sah.

PAIRO bukan aplikasi grup atau jejaring sosial. Fokusnya adalah komunikasi dua pihak, kepemilikan data lokal, dan kontrol pengguna atas backup.

## 3. Prinsip produk dari pembicaraan

1. **Two-party conversation:** setiap pairing menghubungkan tepat dua identity.
2. **E2EE:** isi pesan tidak dapat dibaca oleh relay atau developer.
3. **Local-first:** riwayat chat utama disimpan di perangkat masing-masing.
4. **No permanent cloud chat database:** tidak ada database cloud untuk riwayat percakapan permanen.
5. **Personal cloud backup:** aplikasi menyediakan fitur backup ke cloud milik pengguna; pengguna boleh tidak mengaktifkannya.
6. **Offline-friendly:** pesan untuk penerima offline dapat dititipkan sementara dalam bentuk ciphertext.
7. **Seven-day relay retention:** batas retensi sementara yang dibicarakan adalah tujuh hari atau 168 jam.
8. **Identity isolation:** setiap identity/pairing mempunyai material kriptografi dan ruang data terpisah.
9. **One APK:** semua pengguna memasang aplikasi yang sama.

## 4. Model akun yang dibicarakan

### Standard Mode
- Satu identity pada satu instalasi/profil pengguna.
- Maksimal satu pairing aktif.
- Untuk berpindah pasangan, pairing sebelumnya harus diakhiri terlebih dahulu.

### Owner Mode
- Pemilik proyek dapat membuat beberapa identity independen.
- Tiap identity mempunyai pairing, key material, database lokal, dan backup terpisah.
- Owner Mode hanya memberi kemampuan mengelola identity milik sendiri.
- Tidak ada hak istimewa untuk membaca percakapan pengguna lain.

Mekanisme aman untuk menandai atau mengaktifkan Owner Mode masih terbuka. Jangan mengandalkan nama pengguna, email, atau flag lokal yang mudah dimodifikasi sebagai satu-satunya otorisasi.

## 5. Sasaran MVP

- Android ke Android.
- Pembuatan identity lokal.
- Pairing melalui QR/invitation.
- Persetujuan dua arah.
- Verifikasi fingerprint.
- Pesan teks E2EE.
- Database lokal terenkripsi.
- Online messaging.
- Offline store-and-forward relay.
- Expiry relay maksimal 168 jam.
- Status pesan yang akurat.

Backup dan attachment tetap menjadi bagian desain produk, tetapi urutan implementasinya perlu disepakati.

## 6. Bukan sasaran awal

- Grup atau channel.
- Public profile dan user discovery.
- Cloud chat history permanen.
- Web/desktop client.
- Panggilan audio/video.
- Sinkronisasi multi-device.
- Algoritma kriptografi buatan sendiri.

## 7. Batas klaim

E2EE melindungi isi pesan selama endpoint dan implementasi kriptografi bekerja sebagaimana mestinya. E2EE tidak otomatis menyembunyikan metadata seperti IP, waktu, ukuran paket, atau pola komunikasi, dan tidak melindungi perangkat yang telah dikompromikan.
