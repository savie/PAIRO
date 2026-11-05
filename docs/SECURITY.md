# Keamanan dan Privasi

> **Status: DRAFT — BELUM FINAL — BELUM DISETUJUI. Ini bukan hasil audit keamanan.**

## 1. Sasaran keamanan

PAIRO dirancang agar isi pesan hanya dapat dibaca endpoint yang berpartisipasi dalam pairing. Developer, operator layanan, dan penyedia relay tidak boleh memiliki master key atau jalur administratif untuk mendekripsi percakapan.

Sasaran ini baru dapat diklaim setelah desain, implementasi, pengujian, dan review independen memadai.

## 2. Ancaman yang dipertimbangkan

- Penyadapan jaringan.
- Kebocoran temporary relay.
- Kompromi signaling/relay server.
- Akses fisik ke perangkat yang tidak terkunci.
- Malware atau OS perangkat yang dikompromikan.
- Identity substitution saat pairing.
- Replay, duplicate delivery, dan message tampering.
- Kebocoran backup atau recovery secret.
- Metadata correlation.
- Kesalahan implementasi atau dependency rentan.

## 3. Kriptografi

Usulan awal:
- Gunakan protokol messaging yang telah ditinjau seperti Signal Protocol melalui implementasi library yang kompatibel dan terpelihara.
- Jangan membuat cipher, key exchange, atau ratchet sendiri.
- Gunakan authenticated encryption.
- Verifikasi fingerprint melalui kanal independen.
- Terapkan forward secrecy dan post-compromise security sejauh didukung protokol.
- Backup memakai format berversi dan AEAD seperti AES-256-GCM.
- Password backup tidak boleh digunakan langsung sebagai encryption key.

Nama algoritma atau library di dokumen ini adalah usulan, bukan keputusan final. Lisensi, status pemeliharaan, API, dan kompatibilitas Android harus diperiksa.

## 4. Key management

- Private identity/session key tidak dikirim ke server.
- Android Keystore dapat melindungi key enkripsi lokal atau wrapping key.
- Jangan menulis key ke log, crash report, analytics, clipboard, atau file biasa tanpa proteksi.
- Recovery secret tidak dikirim ke developer.
- Kehilangan recovery material dapat menyebabkan backup tidak dapat dipulihkan.
- Prosedur rotasi, revoke, dan pemulihan identity harus dirancang eksplisit.

## 5. Local security

- Database lokal dienkripsi.
- Key database dilindungi melalui mekanisme Android Keystore yang sesuai.
- Attachment dienkripsi saat tersimpan.
- Plaintext pesan dan key tidak masuk log.
- Notification preview dapat disembunyikan.
- Screenshot detection hanya best effort dan tidak boleh dipromosikan sebagai jaminan.

## 6. Server-side controls

Relay:
- Hanya menerima ciphertext.
- Tidak memiliki decryption key.
- Memvalidasi ukuran/format envelope.
- Mengautentikasi pengiriman dan pengambilan.
- Menerapkan rate limit dan kontrol abuse.
- Menegakkan TTL maksimum 168 jam.
- Menghapus salinan setelah ACK valid atau expiry.
- Meminimalkan log dan retensi metadata.

Signaling:
- Menukar data setup minimum.
- Tidak menerima isi chat atau private key.
- Menghapus data sesi setup saat tidak lagi diperlukan.

## 7. Batas klaim privasi

- IP address, waktu koneksi, ukuran paket, dan pola komunikasi dapat terlihat oleh infrastruktur.
- E2EE tidak melindungi endpoint yang telah dikompromikan.
- Penerima dapat menyalin, memotret, atau merekam konten yang tampil.
- Remote deletion tidak dapat menghapus plaintext yang sudah tersimpan di perangkat penerima.
- Screenshot detection Android tidak universal.
- Cloud backup berarti ciphertext disimpan pada provider pihak ketiga; keamanan juga bergantung pada recovery secret dan implementasi backup.
- Relay retention tujuh hari tidak sama dengan penghapusan semua metadata jaringan.

## 8. Gerbang keamanan sebelum rilis

- Threat model ditinjau.
- Dependency dan lisensi diperiksa.
- Test vector protokol lulus.
- Negative test untuk identity mismatch, replay, dan tampering.
- Audit log untuk memastikan tidak ada plaintext/key.
- Pengujian isolasi antaridentity.
- Pengujian backup/restore dan wrong-key.
- Review independen atas implementasi kriptografi dan relay.

## 9. Private in-app media (usulan)

- Attachment dienkripsi sebagai payload E2EE sebelum dikirim ke relay/provider.
- Ciphertext disimpan di internal app storage; plaintext tidak diekspor ke shared/public storage secara default.
- Viewer/player internal menghindari implicit ACTION_VIEW dan tidak membagikan URI/file ke aplikasi lain tanpa tindakan eksplisit.
- Thumbnail, preview, recent-task snapshot, clipboard, dan temporary files masuk dalam threat model.
- FLAG_SECURE dapat dipertimbangkan untuk layar sensitif, tetapi bukan jaminan anti-capture 100%.
- Validasi ukuran, signature/magic bytes, MIME type, dan input decoder; ekstensi file saja tidak membuktikan file aman.
- Audit exported components dan content providers agar data privat tidak terekspos.

Batas klaim: jika pengguna mengekspor/membagikan file, salinan plaintext dapat dibuka aplikasi lain. OS/perangkat yang dikompromikan dan perekaman eksternal berada di luar jaminan aplikasi. Detail rancangan ada di [FILES_AND_MEDIA.md](FILES_AND_MEDIA.md).
