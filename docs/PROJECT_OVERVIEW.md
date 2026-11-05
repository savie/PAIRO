# Ringkasan Proyek PAIRO

> **Status: DRAFT — BELUM FINAL — BELUM DISETUJUI.**

## 1. Identitas produk

| Atribut | Nilai |
|---|---|
| Nama | PAIRO |
| Kategori | Private one-to-one messenger |
| Platform awal | Android |
| Target rentang OS awal | Android 8 sampai Android 17 |
| Bahasa implementasi | Kotlin (usulan) |
| UI | Jetpack Compose (usulan) |
| Tahap | Konsep dan dokumentasi awal |
| Tagline kerja | Two Identities. One Private Space. |

Nama PAIRO telah dipilih dalam pembicaraan. Rentang Android 8–17 merupakan target awal yang disampaikan dan masih perlu verifikasi API level, dependency, serta perangkat nyata. Tagline dan rincian teknis lainnya masih berupa draft.

## 2. Visi

Membangun satu aplikasi Android yang memungkinkan dua identity berkomunikasi dalam satu ruang privat setelah keduanya melakukan pairing dan verifikasi. Isi percakapan dirancang hanya dapat dibaca oleh kedua endpoint yang sah.

PAIRO berfokus pada komunikasi dua pihak, kepemilikan data lokal, kontrol pengguna atas backup, dan pengelolaan kontak/pairing di dalam aplikasi.

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
10. **Single-device identity:** satu identity hanya aktif pada satu perangkat dalam satu waktu; migrasi ke perangkat baru memerlukan validasi.
11. **Mutual pairing consent:** memutus pairing dan memasangkan kembali memerlukan persetujuan pihak-pihak terkait.
12. **PAIRO local contacts:** daftar kontak dikelola di dalam PAIRO; aplikasi tidak meminta akses ke kontak sistem Android.
13. **Selective backup:** kategori data dapat dipilih; percakapan menjadi satu batch, sedangkan media/file dapat dipilih satuan atau multi-select.

## 4. Model akun dan kontak yang dibicarakan

### Standard Mode
- Satu identity aktif.
- Maksimal satu pairing aktif.
- Untuk berpindah pasangan, pairing sebelumnya harus diakhiri melalui proses persetujuan yang ditentukan.

### Owner Mode
- Owner dapat mengelola beberapa user/pairing sebagai kontak lokal PAIRO.
- Setiap kontak mewakili hubungan one-to-one yang independen.
- Tiap identity mempunyai pairing, key material, database lokal, dan backup terpisah.
- Owner Mode hanya memberi kemampuan mengelola identity/pairing milik sendiri.
- Tidak ada hak istimewa untuk membaca percakapan pengguna lain.
- PAIRO tidak mengakses address book Android.

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

Backup dan attachment tetap menjadi bagian desain produk. Identity migration/recovery, single-device enforcement, dan backup granularity harus dirancang sebelum fitur terkait dinyatakan siap.

## 6. Bukan sasaran awal

- Grup atau channel.
- Public profile dan user discovery.
- Cloud chat history permanen.
- Web/desktop client.
- Panggilan audio/video.
- Sinkronisasi multi-device untuk satu identity.
- Algoritma kriptografi buatan sendiri.

## 7. Batas klaim

E2EE melindungi isi pesan selama endpoint dan implementasi kriptografi bekerja sebagaimana mestinya. E2EE tidak otomatis menyembunyikan metadata seperti IP, waktu, ukuran paket, atau pola komunikasi, dan tidak melindungi perangkat yang telah dikompromikan.
