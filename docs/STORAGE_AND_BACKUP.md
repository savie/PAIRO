# Penyimpanan Lokal dan Personal Cloud Backup

> **Status: DRAFT — BELUM FINAL — BELUM DISETUJUI.**

## 1. Prinsip penyimpanan

Riwayat chat utama berada pada database lokal masing-masing perangkat. Tidak ada cloud database untuk menyimpan chat history permanen.

Perlu dibedakan:
- **Local database:** wajib sebagai penyimpanan utama.
- **Cloud chat database:** tidak digunakan.
- **Personal cloud backup:** fitur wajib tersedia di aplikasi, tetapi aktivasi dan penggunaannya opsional bagi pengguna.
- **Temporary relay:** antrean ciphertext sementara untuk offline delivery, bukan backup dan bukan riwayat chat permanen.

## 2. Data lokal

Secara konseptual data lokal mencakup:
- Identity metadata dan material pemulihan yang dipilih untuk dicadangkan.
- Pairing/contact metadata.
- Message records dan delivery state.
- Attachment references dan encrypted blobs.
- Pengaturan aplikasi.
- Status backup.

## 3. Isolasi identity

Setiap identity memiliki namespace data sendiri. Pada Owner Mode, database, attachment, session, dan backup tidak boleh tercampur.

Contoh struktur logis:

```text
PAIRO/
  identities/
    <identity-A>/
      database/
      attachments/
      state/
    <identity-B>/
      database/
      attachments/
      state/
  app/
    preferences/
```

Struktur ini bersifat konseptual. Private key tidak boleh disimpan sebagai file biasa hanya karena ada direktori bernama keys.

## 4. Database lokal

SQLite/Room dapat dievaluasi sebagai lapisan data dengan solusi enkripsi database yang dipelihara. Kunci tidak boleh ditanam di source code atau disimpan plaintext. Implementasi perlu mendukung transaksi, migrasi skema, snapshot konsisten, dan pemulihan setelah crash.

## 5. Personal cloud backup

Arahan produk:
- Semua kategori data yang ditetapkan harus tersedia sebagai pilihan backup.
- Pengguna dapat memilih kategori data yang ingin dicadangkan.
- Backup tetap opsional dan tidak berjalan tanpa persetujuan pengguna.
- Backup harus mendukung pemilihan media/file satuan maupun multi-select.

Provider yang dipertimbangkan:
- Google Drive.
- Dropbox.
- OneDrive.
- MEGA, setelah API dan ketentuannya diverifikasi.

Provider pertama untuk MVP belum dipilih. Aplikasi harus meminta persetujuan pengguna dan hanya meminta scope akses yang diperlukan.

## 6. Granularitas pilihan backup

### Percakapan

- Seluruh data percakapan diperlakukan sebagai satu batch backup.
- Pengguna tidak memilih atau mengecualikan percakapan satu per satu.
- Pilihan backup percakapan mencakup batch percakapan sesuai identity yang sedang dipilih.
- Apakah batch dipisah per identity atau mencakup semua identity dalam satu arsip masih perlu diputuskan.

### Media dan file

- Pengguna dapat memilih satu media/file tertentu.
- Pengguna dapat memilih beberapa media/file sekaligus (multi-select).
- Pemilihan dilakukan saat proses backup.
- Media/file yang dipilih tetap dienkripsi sebelum diunggah.
- Perlu ditentukan bagaimana attachment yang terkait dengan pesan ditampilkan ketika pengguna memilih media secara terpisah.

### Kategori lain

Identity, pairing/contact metadata, pengaturan, dan state aplikasi perlu tersedia sebagai kategori yang dapat dipilih. Namun, material identity/session yang sensitif membutuhkan kebijakan khusus agar backup tidak menciptakan dua perangkat aktif atau melemahkan keamanan identity.

## 7. Alur backup

1. Pengguna memilih kategori yang ingin dicadangkan.
2. Jika kategori percakapan dipilih, PAIRO menyiapkan seluruh percakapan sebagai satu batch, bukan per-conversation selection.
3. Jika kategori media/file dipilih, pengguna dapat memilih satu atau beberapa item.
4. Client membuat snapshot konsisten dan manifest backup.
5. Client mengenkripsi arsip/data di perangkat sebelum upload.
6. Client mengunggah ciphertext ke cloud pilihan pengguna.
7. Metadata minimum disimpan untuk identifikasi versi backup.
8. Restore mengunduh ciphertext, memverifikasi integritas, mendekripsi lokal, lalu memulihkan data sesuai pilihan dan validasi identity.

Provider tidak menerima plaintext backup atau encryption key.

## 8. Recovery dan single-device identity

- Identity harus dapat dipulihkan/dipindahkan ke perangkat pengganti melalui proses validasi.
- Satu identity hanya boleh aktif pada satu perangkat dalam satu waktu.
- Restore backup pada perangkat baru tidak boleh membuat perangkat lama tetap aktif menggunakan identity yang sama.
- Recovery key/secret terpisah dari password akun cloud.
- Recovery key dibuat bersamaan dengan pembuatan identity, bukan saat backup pertama kali dijalankan.
- Pengguna harus diberi kesempatan menyimpan recovery key secara mandiri di luar perangkat.
- Saat perangkat aktif masih dapat diakses, migrasi meminta persetujuan dari perangkat aktif.
- Jika perangkat aktif hilang/rusak/tidak tersedia, recovery key menjadi jalur validasi alternatif.
- Kehilangan recovery key ketika perangkat aktif juga tidak tersedia dapat menyebabkan identity tidak dapat dipulihkan; prosedur pemulihan alternatif belum ditetapkan.
- Backup identity/session keys meningkatkan sensitivitas arsip dan memerlukan desain khusus.
- Format backup harus versioned dan memiliki strategi migrasi.

## 9. Penghapusan

Aplikasi dapat meminta penghapusan backup melalui API provider. Penghapusan file tidak menjamin seluruh salinan internal provider langsung terhapus dari sistem backup mereka. Status penghapusan harus mengikuti kemampuan dan respons provider.

## 10. Hal yang masih terbuka

- Daftar kategori backup final.
- Apakah percakapan dibatch per identity atau lintas identity.
- Apakah media/file yang dipilih satuan ikut membawa metadata pesan terkait.
- Apakah backup identity/session material dipisahkan dari backup konten.
- Protokol persetujuan perangkat aktif dan recovery-key fallback.
- Format, penyimpanan, rotasi, dan perlindungan recovery key.
- Mekanisme revocation yang menjamin perangkat lama kehilangan otorisasi setelah recovery-key migration.
- Prosedur jika recovery key hilang dan perangkat aktif tidak tersedia.
- Provider cloud pertama, batas ukuran, dan strategi upload/resume.
