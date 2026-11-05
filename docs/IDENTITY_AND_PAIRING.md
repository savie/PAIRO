# Identity dan Pairing

> **Status: DRAFT — BELUM FINAL — BELUM DISETUJUI.**

## 1. Konsep identity

Identity adalah entitas kriptografis lokal PAIRO, bukan sekadar username atau alamat email.

Setiap identity secara konseptual memiliki:
- Identity key pair.
- Material pre-key/session sesuai protokol yang dipilih.
- Fingerprint atau safety number.
- Device authorization record.
- Pairing records.
- Namespace database dan backup tersendiri.

Private key tidak dikirim ke server. Public key dan metadata routing minimum dapat dibagikan sesuai kebutuhan protokol.

## 2. Aturan perangkat

Arahan produk: **satu identity hanya aktif pada satu perangkat dalam satu waktu**.

- Identity dapat dipindahkan atau dipulihkan ke perangkat pengganti.
- Aktivasi identity pada perangkat baru wajib melalui proses validasi.
- Setelah migrasi/aktivasi perangkat baru berhasil, perangkat lama tidak boleh tetap menggunakan identity yang sama sebagai perangkat aktif.
- Perangkat lama harus dinonaktifkan atau otorisasinya dicabut melalui mekanisme yang dapat diverifikasi.
- Jika perangkat lama masih tersedia, proses migrasi dapat memanfaatkannya sebagai faktor persetujuan tambahan; jika hilang/rusak, diperlukan recovery flow alternatif.
- Detail faktor validasi, recovery secret, key rotation, dan penanganan perangkat offline masih terbuka.

Restore file backup saja tidak boleh otomatis membuat dua perangkat aktif dengan identity key yang sama.

## 3. Standard Mode dan Owner Mode

### Standard Mode
- Satu identity aktif.
- Maksimal satu pairing aktif.
- Pairing sebelumnya harus diakhiri sebelum identity membuat pairing baru.

### Owner Mode
- Owner dapat mengelola beberapa user/pairing sebagai daftar kontak lokal di dalam PAIRO.
- Setiap kontak mewakili hubungan pairing antara dua identity; setiap percakapan tetap one-to-one.
- Nama/label kontak dapat disimpan secara lokal oleh PAIRO.
- PAIRO tidak meminta akses ke address book atau kontak sistem Android.
- Setiap identity/pairing memiliki key material, session, database, dan konteks percakapan yang terisolasi.
- Owner tidak memiliki akses membaca percakapan identity lain hanya karena memiliki Owner Mode.

Mekanisme provisioning Owner Mode masih terbuka. Jangan mengandalkan flag lokal yang dapat dimodifikasi sebagai satu-satunya kontrol otorisasi.

## 4. Pairing lifecycle

Status konseptual:
- `created`
- `invited`
- `pending_remote_approval`
- `pending_verification`
- `active`
- `suspended`
- `revoked`
- `expired`

Pairing hanya aktif setelah kedua pihak menyetujui dan menyelesaikan verifikasi yang diwajibkan.

Memutus pairing dan membuat pairing kembali harus melalui persetujuan pihak-pihak terkait. Tidak boleh ada re-pairing diam-diam atau aktivasi sepihak. Detail apakah persetujuan dilakukan pada kedua perangkat yang sedang aktif atau melalui recovery flow masih perlu dirancang.

## 5. Undangan QR/invitation code

Undangan sebaiknya:
- Menggunakan entropy yang memadai.
- Berumur pendek.
- Sekali pakai.
- Tidak memuat private key.
- Tidak menjadi bukti identitas sendirian.
- Dapat dibatalkan oleh pembuat undangan.

QR adalah cara bootstrap, bukan pengganti mutual verification.

## 6. Mutual verification

Setelah pertukaran public identity material, kedua pihak membandingkan fingerprint/safety number melalui kanal tepercaya. UI harus menampilkan perubahan identity key dan meminta verifikasi ulang sebelum sesi sensitif diteruskan.

## 7. Mengakhiri dan menyambungkan kembali pairing

- Salah satu pihak dapat memulai permintaan untuk mengakhiri pairing.
- Perubahan status dan pemutusan pairing harus mengikuti persetujuan yang ditetapkan untuk kedua pihak.
- Aplikasi menghentikan pengiriman menggunakan sesi lama setelah revoke diproses.
- Key/session terkait dinonaktifkan atau dihapus sesuai kebijakan retensi.
- Kedua pihak diberi status yang jelas.
- Pairing baru memerlukan undangan, persetujuan, dan verifikasi baru.
- Revoke tidak dapat menghapus plaintext atau salinan yang sudah tersimpan di perangkat lawan bicara.

## 8. Pemulihan dan migrasi identity

Identity harus dapat dipulihkan/dipindahkan ke perangkat pengganti. Recovery identity berbeda dari sekadar restore database atau attachment.

Desain wajib mencakup:
- Validasi pemilik identity.
- Pengikatan identity ke perangkat baru.
- Penonaktifan/revocation perangkat lama.
- Key rotation atau re-establishment session bila diperlukan.
- Pemulihan data dari backup terenkripsi.
- Penanganan perangkat lama yang hilang, rusak, offline, atau masih aktif.
- Perlindungan dari restore backup yang menghasilkan dua perangkat aktif.

Prosedur, recovery secret, dan faktor validasi belum diputuskan. Jangan mengklaim identity recovery aman sebelum alur tersebut dirancang dan diuji.
