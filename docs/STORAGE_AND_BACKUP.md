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
- Identity metadata.
- Pairing metadata.
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

Provider yang dipertimbangkan:
- Google Drive.
- Dropbox.
- OneDrive.
- MEGA, setelah API dan ketentuannya diverifikasi.

Provider pertama untuk MVP belum dipilih. Aplikasi harus meminta persetujuan pengguna dan hanya meminta scope akses yang diperlukan.

## 6. Alur backup

1. Buat snapshot konsisten database dan attachment.
2. Buat arsip dengan manifest dan format version.
3. Enkripsi arsip pada perangkat sebelum upload.
4. Upload ciphertext ke cloud pilihan pengguna.
5. Simpan metadata minimum untuk identifikasi versi backup.
6. Restore mengunduh ciphertext, memverifikasi integritas, mendekripsi lokal, lalu memulihkan data.

Provider tidak menerima plaintext backup atau encryption key.

## 7. Recovery

- Recovery key/secret terpisah dari password akun cloud.
- Pengguna harus dapat menyimpan recovery material secara aman.
- Kehilangan recovery secret dapat membuat backup tidak dapat dipulihkan.
- Restore database tidak otomatis memulihkan identity/session jika material identity tidak ikut dicadangkan dengan aman.
- Format backup harus versioned dan memiliki strategi migrasi.
- Backup identity/session keys meningkatkan sensitivitas arsip dan memerlukan desain khusus.

## 8. Penghapusan

Aplikasi dapat meminta penghapusan backup melalui API provider. Penghapusan file tidak menjamin seluruh salinan internal provider langsung terhapus dari sistem backup mereka. Status penghapusan harus mengikuti kemampuan dan respons provider.
