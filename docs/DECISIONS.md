# Keputusan Awal dan Daftar Hal Terbuka

> **Status: DRAFT — BELUM FINAL — BELUM DISETUJUI.**
>
> Dokumen ini adalah register utama status keputusan. Bagian hasil pembicaraan adalah keputusan produk yang sudah dibahas; bagian terbuka hanya memuat hal yang benar-benar belum diputuskan. Dokumen domain menjelaskan detail tanpa membuat daftar keputusan paralel.

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
| TALK-011 | Standard Mode | Satu identity dengan satu pairing aktif |
| TALK-012 | Owner Mode | Owner dapat mengelola beberapa pairing/user sebagai entri kontak lokal PAIRO; setiap hubungan tetap merupakan pairing dua identity yang independen |
| TALK-013 | Owner privilege | Tidak memberi akses membaca chat identity lain |
| TALK-014 | Attachment export | Pengguna boleh memilih sendiri untuk menyimpan/mengekspor attachment biasa ke Gallery/perangkat atau membagikannya; tidak otomatis |
| TALK-015 | View Once | Fitur View Once untuk media disetujui sebagai kebutuhan produk; detail perilaku teknis masih terbuka |
| TALK-016 | Video View Once replay | Video boleh diputar ulang selama sesi viewer yang sama masih terbuka; menutup viewer mengakhiri kesempatan melihat |
| TALK-017 | Rentang dukungan Android | Target produk Android 8 sampai Android 17; pengujian kompatibilitas merupakan pekerjaan QA, bukan pemilihan ulang rentang |
| TALK-018 | Identity portability | Identity harus dapat dipindahkan/dipulihkan ke perangkat pengganti melalui proses validasi |
| TALK-019 | Single-device identity | Satu identity hanya boleh aktif pada satu perangkat dalam satu waktu; aktivasi di perangkat lain memerlukan validasi dan menonaktifkan/mencabut otorisasi perangkat lama sesuai prosedur |
| TALK-020 | Pairing lifecycle | Memutus pairing dan memasangkan kembali harus melalui persetujuan pihak-pihak terkait; pairing baru tidak aktif sepihak |
| TALK-021 | Phone contacts | PAIRO menggunakan daftar kontak/pairing lokal di dalam aplikasi dan tidak meminta akses ke address book/kontak sistem Android |
| TALK-022 | Backup selection | Semua kategori data yang ditetapkan harus dapat dicadangkan. Pengguna dapat memilih kategori backup |
| TALK-023 | Conversation backup granularity | Seluruh percakapan dicadangkan sebagai satu batch; pengguna tidak memilih percakapan satu per satu |
| TALK-024 | Media/file backup granularity | Media dan file dapat dipilih secara individual atau multi-select saat proses backup |
| TALK-025 | Licensing direction | Preferensi awal adalah model hybrid: dapat menggunakan komponen open-source yang lisensinya sesuai dan mengembangkan komponen PAIRO sendiri; lisensi aplikasi/repository belum diputuskan |
| TALK-026 | Identity migration behavior | Ketika identity dipindahkan ke perangkat baru, identity tetap sama. Identitas kriptografis dan fingerprint tetap dipertahankan; bukan membuat identity baru. Migrasi tetap memerlukan validasi dan menonaktifkan perangkat lama. |
| TALK-027 | Identity reactivation validation | Jika perangkat lama masih tersedia, perpindahan kembali ke perangkat tersebut memerlukan persetujuan dari perangkat yang saat ini aktif. Jika perangkat aktif tidak tersedia, pemilik dapat menggunakan recovery key untuk membuktikan kepemilikan. Kedua jalur harus tetap menegakkan satu perangkat aktif per identity. |
| TALK-028 | Recovery key issuance | Recovery key dibuat saat identity pertama kali dibuat, bukan menunggu proses backup. Recovery key terpisah dari password akun cloud dan harus dapat disimpan secara mandiri oleh pengguna. |

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
| PROP-010 | Client-side encrypted backup archive | Backup dienkripsi di perangkat sebelum upload |
| PROP-011 | Google Drive sebagai provider awal | Kandidat integrasi awal |
| PROP-012 | Local contact book PAIRO | Menyimpan label/nama lokal untuk setiap pairing tanpa membaca kontak Android |
| PROP-013 | Device authorization record | Mengikat satu identity ke satu perangkat aktif dan mendukung proses migrasi/revocation |

## 3. Detail teknis yang masih terbuka

Pertanyaan berikut adalah pekerjaan perincian implementasi atas keputusan yang sudah ada, bukan pembukaan ulang keputusan produk.

| ID | Detail teknis yang perlu dirancang/diverifikasi | Dampak |
|---|---|---|
| OPEN-001 | Spesifikasi protokol persetujuan perangkat aktif dan recovery-key fallback: autentikasi, expiry, rate limit, dan replay protection. | Pencegahan pengambilalihan identity |
| OPEN-002 | Mekanisme teknis pencabutan otorisasi perangkat lama yang tetap efektif pada jalur recovery key tanpa cloud database identity permanen. | Single-device enforcement |
| OPEN-003 | Prosedur transfer/pemulihan material identity dan pembentukan ulang session/pre-key tanpa mengubah identity/fingerprint. | Keamanan migrasi |
| OPEN-004 | Mekanisme provisioning dan enforcement Owner Mode yang aman, tanpa mengubah model kontak lokal dan batas akses percakapan yang telah disepakati. | Otorisasi Owner Mode |
| OPEN-005 | Library Signal Protocol mana yang kompatibel dan berlisensi sesuai? | E2EE implementation |
| OPEN-006 | Library WebRTC dan distribusinya? | Ukuran APK dan maintenance |
| OPEN-007 | Signaling service dibangun di mana? | Hosting dan operasi |
| OPEN-008 | Relay self-host atau managed service? | Biaya dan kontrol |
| OPEN-009 | Apakah TURN disediakan sendiri atau pihak ketiga? | Konektivitas dan bandwidth |
| OPEN-010 | Provider cloud backup pertama? | OAuth/API/scope |
| OPEN-011 | Data identity/session apa yang masuk backup dan bagaimana proteksinya? | Restore dan keamanan |
| OPEN-012 | Format recovery key, cara penyimpanan/rotasi, dan prosedur jika recovery key hilang? | Kehilangan data dan pemulihan identity |
| OPEN-013 | Apakah TTL pengguna dapat lebih pendek dari tujuh hari? | Retensi |
| OPEN-014 | Bagaimana attachment besar diunggah saat offline? | Relay/storage design |
| OPEN-015 | Lisensi aplikasi utama, lisensi kontribusi, dan kebijakan distribusi dependency? | Legal dan distribusi |
| OPEN-016 | Kapan View Once dianggap telah dilihat dan bagaimana menangani crash/force-stop? | Lifecycle dan anti-replay |
| OPEN-017 | Bagaimana sesi viewer View Once ditandai selesai secara andal? | UX dan state machine |
| OPEN-018 | Berapa lama penerima boleh menunggu sebelum membuka View Once? | Expiry dan UX |
| OPEN-019 | Bagaimana View Once berperilaku pada recovery/migrasi perangkat? | Sinkronisasi state |
| OPEN-020 | Kategori data apa saja yang tersedia pada backup selector dan bagaimana format manifest-nya? | Backup UX dan restore |
| OPEN-021 | Apakah backup percakapan batch mencakup seluruh identity atau dipisah per identity? | Isolasi data dan restore |

## 4. Aturan perubahan

Setiap item hanya dapat dipindahkan menjadi keputusan final setelah dibahas dan disetujui pemilik proyek. Perubahan harus dicatat bersama alasan, konsekuensi, dan dokumen terdampak.
