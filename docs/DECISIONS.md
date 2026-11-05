# Keputusan Awal dan Daftar Hal Terbuka

> **Status: DRAFT — BELUM FINAL — BELUM DISETUJUI.**
>
> Tabel ini membedakan hasil pembicaraan dengan rekomendasi teknis. Tidak ada usulan teknis yang otomatis menjadi keputusan final.

## 1. Hasil pembicaraan yang sudah jelas

| ID | Topik | Hasil pembicaraan |
|---|---|---|
| TALK-001 | Nama aplikasi | PAIRO |
| TALK-002 | Platform awal | Android |
| TALK-003 | Model komunikasi | Satu ruang komunikasi untuk dua identity |
| TALK-004 | APK | Satu APK untuk semua pengguna |
| TALK-005 | E2EE | Isi chat hanya diketahui dua endpoint yang ber-pairing |
| TALK-006 | Database lokal | Wajib |
| TALK-007 | Cloud chat database | Tidak ada |
| TALK-008 | Personal cloud backup | Fitur tersedia; pemakaian opsional |
| TALK-009 | Offline-friendly | Pesan untuk penerima offline dapat menunggu di relay |
| TALK-010 | Relay retention | Rancangan tujuh hari / 168 jam |
| TALK-011 | Standard Mode | Satu identity dan satu pairing aktif |
| TALK-012 | Owner Mode | Owner dapat membuat beberapa identity independen |
| TALK-013 | Owner privilege | Tidak memberi akses membaca chat identity lain |
| TALK-014 | Attachment export | Pengguna boleh memilih sendiri untuk menyimpan/mengekspor attachment biasa ke Gallery/perangkat atau membagikannya; tidak otomatis |
| TALK-015 | View Once | Fitur View Once untuk media disetujui sebagai kebutuhan produk; detail perilaku teknis masih terbuka |

## 2. Usulan teknis yang belum disetujui

| ID | Usulan | Alasan |
|---|---|---|
| PROP-001 | Kotlin + Jetpack Compose | Android native dan UI modern |
| PROP-002 | Room/SQLite + database encryption | Local persistence |
| PROP-003 | Android Keystore | Melindungi key lokal |
| PROP-004 | Signal Protocol implementation | Menghindari desain protokol kriptografi sendiri |
| PROP-005 | WebRTC DataChannel | Direct P2P saat kedua pihak online |
| PROP-006 | Lightweight signaling service | Membantu setup koneksi WebRTC |
| PROP-007 | STUN/TURN | Mengatasi keterbatasan NAT/jaringan |
| PROP-008 | Temporary ciphertext relay | Store-and-forward saat penerima offline |
| PROP-009 | FCM wake-up hint | Membantu receiver mengetahui ada antrean |
| PROP-010 | AES-GCM encrypted backup | Client-side encrypted archive |
| PROP-011 | Google Drive sebagai provider awal | Kandidat integrasi awal |

## 3. Keputusan terbuka

| ID | Pertanyaan | Dampak |
|---|---|---|
| OPEN-001 | Minimum Android API level? | Kompatibilitas perangkat |
| OPEN-002 | Apakah satu identity boleh pindah perangkat? | Recovery dan key lifecycle |
| OPEN-003 | Bagaimana provisioning Owner Mode? | Otorisasi owner |
| OPEN-004 | Library Signal Protocol mana yang kompatibel dan berlisensi sesuai? | E2EE implementation |
| OPEN-005 | Library WebRTC dan distribusinya? | Ukuran APK dan maintenance |
| OPEN-006 | Signaling service dibangun di mana? | Hosting dan operasi |
| OPEN-007 | Relay self-host atau managed service? | Biaya dan kontrol |
| OPEN-008 | Apakah TURN disediakan sendiri atau pihak ketiga? | Konektivitas dan bandwidth |
| OPEN-009 | Provider cloud backup pertama? | OAuth/API/scope |
| OPEN-010 | Apakah backup mencakup identity/session material? | Restore dan keamanan |
| OPEN-011 | Bagaimana pemulihan jika recovery secret hilang? | Kehilangan data |
| OPEN-012 | Apakah TTL pengguna dapat lebih pendek dari tujuh hari? | UX dan retensi |
| OPEN-013 | Bagaimana attachment besar diunggah saat offline? | Relay/storage design |
| OPEN-014 | Lisensi proyek dan kebijakan privasi? | Distribusi dan legal |
| OPEN-015 | Kapan View Once dianggap telah dilihat dan bagaimana menangani crash/force-stop? | Lifecycle dan anti-replay |
| OPEN-016 | Apakah video View Once boleh diputar ulang dalam sesi tampilan yang sama? | UX dan state machine |
| OPEN-017 | Berapa lama penerima boleh menunggu sebelum membuka View Once? | Expiry dan UX |
| OPEN-018 | Bagaimana View Once berperilaku pada multi-device/recovery? | Sinkronisasi state |

## 4. Aturan perubahan

Setiap item hanya dapat dipindahkan menjadi keputusan final setelah dibahas dan disetujui pemilik proyek. Perubahan harus dicatat bersama alasan, konsekuensi, dan dokumen terdampak.
