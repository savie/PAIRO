# Identity dan Pairing

> **Status: DRAFT — BELUM FINAL — BELUM DISETUJUI.**

## 1. Konsep identity

Identity adalah entitas kriptografis lokal PAIRO, bukan sekadar username atau alamat email.

Setiap identity secara konseptual memiliki:
- Identity key pair.
- Material pre-key/session sesuai protokol yang dipilih.
- Fingerprint atau safety number.
- Pairing record.
- Namespace database dan backup tersendiri.

Private key tidak dikirim ke server. Public key dan metadata routing minimum dapat dibagikan sesuai kebutuhan protokol.

## 2. Standard Mode dan Owner Mode

### Standard Mode
- Satu identity aktif.
- Maksimal satu pairing aktif.
- Pairing sebelumnya harus diakhiri sebelum identity membuat pairing baru.

### Owner Mode
- Pemilik proyek dapat membuat banyak identity independen.
- Setiap identity memiliki key material, session, pasangan, database lokal, dan backup terpisah.
- Berpindah identity harus mengunci konteks identity sebelumnya.
- Owner tidak memiliki akses membaca percakapan pengguna lain.

Mekanisme provisioning Owner Mode masih terbuka. Jangan mengandalkan flag lokal yang dapat dimodifikasi sebagai satu-satunya kontrol otorisasi.

## 3. Pairing lifecycle

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

## 4. Undangan QR/invitation code

Undangan sebaiknya:
- Menggunakan entropy yang memadai.
- Berumur pendek.
- Sekali pakai.
- Tidak memuat private key.
- Tidak menjadi bukti identitas sendirian.
- Dapat dibatalkan oleh pembuat undangan.

QR adalah cara bootstrap, bukan pengganti mutual verification.

## 5. Mutual verification

Setelah pertukaran public identity material, kedua pihak membandingkan fingerprint/safety number melalui kanal tepercaya. UI harus menampilkan perubahan identity key dan meminta verifikasi ulang sebelum sesi sensitif diteruskan.

## 6. Mengakhiri pairing

- Salah satu pihak dapat memulai revoke.
- Aplikasi menghentikan pengiriman menggunakan sesi lama setelah revoke diproses.
- Key/session terkait dihapus sesuai kebijakan.
- Kedua pihak diberi status yang jelas.
- Pairing baru memerlukan undangan dan verifikasi baru.
- Revoke tidak dapat menghapus plaintext atau salinan yang sudah tersimpan di perangkat lawan bicara.

## 7. Pemulihan identity

Restore database tidak otomatis sama dengan pemulihan identity. Device replacement harus memiliki prosedur tersendiri, termasuk cara memverifikasi perangkat baru dan menangani key rotation. Desain recovery belum final.
