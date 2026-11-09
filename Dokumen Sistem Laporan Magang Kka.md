# DOKUMEN MASTER SISTEM INFORMASI PENDAFTARAN KOMPETISI KOMPETENSI AKADEMIK (KKA) 2026
**Mitra Industri:** PT Kediri Media Grafika (Jawa Pos Radar Kediri)  
**Dokumen Pendukung:** Laporan Magang Kerja Industri (MKI) & Rencana Lanjutan Tugas Akhir (Skripsi)  
**Domain Produksi:** `kkaradarkediri.com`  
**Stack Teknologi:** Astro 5.x · TypeScript · Bootstrap 5.3 · Supabase (PostgreSQL & Storage) · Google Apps Script · SheetJS  

---

## DAFTAR ISI DOKUMEN
- **BAB I: PENDAHULUAN** (Latar Belakang, Rumusan Masalah, Tujuan, Manfaat, Batasan Masalah)
- **BAB II: GAMBARAN UMUM PERUSAHAAN** (Sejarah, Aktivitas Bisnis, Penerapan Teknologi, Data Pembimbing)
- **BAB III: PELAKSANAAN** (Waktu, Tempat, Rincian Jadwal Kegiatan)
- **BAB IV: HASIL DAN PEMBAHASAN** (Arsitektur Sistem, Subsistem Pengguna Publik, Subsistem Administrator, Alur Data & Dual-Sync)
- **BAB V: PENUTUP** (Kesimpulan, Saran & Rencana Lanjutan ke Tugas Akhir)
- **DAFTAR PUSTAKA & LAMPIRAN**

---

# BAB I: PENDAHULUAN

### 1.1 Latar Belakang
Jawa Pos Radar Kediri (PT Kediri Media Grafika) merupakan perusahaan pers dan media informasi multiplatform terkemuka di wilayah eks-Karesidenan Kediri (Kota Kediri, Kabupaten Kediri, Nganjuk, Kota Blitar, Kabupaten Blitar, Tulungagung, dan Trenggalek). Salah satu agenda tahunan terbesar yang diselenggarakan adalah **Kompetisi Kompetensi Akademik (KKA)**, yaitu ajang olimpiade sains dan literasi berbasis ujian tertulis (Paper-Based Test / Lembar Jawaban Komputer LJK) untuk jenjang SD/MI, SMP/MTs, dan SMA/MA/SMK sederajat. Pada tahun sebelumnya, kegiatan ini melibatkan lebih dari 5.600 peserta dari ratusan sekolah.

Sebelum dilakukannya proyek magang ini, sistem publikasi dan pendaftaran KKA masih memiliki beberapa kelemahan mendasar:
1. **Platform Lama Statis dan Terfragmentasi**: Informasi kompetisi hanya menggunakan platform Blogspot yang kaku, sementara alur registrasi dialihkan ke formulir pihak ketiga (*Google Forms*) yang tidak terintegrasi dengan database sekolah resmi.
2. **Kendala Pendaftaran Rombongan Sekolah**: Guru pembina dari ratusan sekolah kesulitan mendaftarkan siswa dalam jumlah banyak karena formulir daring konvensional hanya mendukung input satu per satu.
3. **Kerentanan Data dan Integritas Peserta**: Masih terjadi ketidaksamaan format penulisan nama sekolah, kesalahan penginputan Nomor Induk Siswa Nasional (NISN), serta ketiadaan filter pencegah duplikasi NISN peserta.
4. **Beban Rekonsiliasi Panitia**: Panitia harus memeriksa struk transfer secara manual satu per satu dan menyalin data dari Google Forms ke spreadsheet rekapitulasi, yang rawan kesalahan manusia (*human error*) dan manipulasi bukti transfer.

Untuk menjawab permasalahan tersebut, selama masa Magang Kerja Industri (MKI) dikembangkan portal resmi terpadu **Website KKA Radar Kediri** berbasis teknologi modern *Astro 5*, *Supabase PostgreSQL*, dan pemrosesan lembar kerja *SheetJS*.

### 1.2 Rumusan Masalah
1. Bagaimana merancang dan membangun portal web pendaftaran KKA Radar Kediri yang cepat, responsif, dan mudah diakses oleh ribuan peserta dan guru sekolah?
2. Bagaimana mengimplementasikan sistem pendaftaran kolektif berbasis unggah berkas Excel yang mampu membaca dan memvalidasi ratusan baris data siswa secara instan di peramban pengguna?
3. Bagaimana mengintegrasikan database 5.671 sekolah resmi Kemendikbud se-Karesidenan Kediri dengan filter wilayah dan jenjang terpandu?
4. Bagaimana memastikan integritas data peserta melalui sistem penolakan otomatis NISN ganda (*strict duplicate validation*)?
5. Bagaimana merancang dashboard administrator terpusat untuk verifikasi pembayaran, manajemen data sekolah, dan kontrol status pendaftaran secara *real-time*?

### 1.3 Tujuan
1. Menghasilkan sistem portal pendaftaran terpadu KKA Radar Kediri dengan performa tinggi (*Google Lighthouse score* optimal).
2. Menyediakan modul pendaftaran mandiri (individu) dan kolektif (rombongan sekolah berbasis template Excel resmi per kecamatan).
3. Mengembangkan modul validasi data otomatis untuk menolak nomor NISN yang telah terdaftar di database panitia.
4. Membangun mekanisme sinkronisasi data ganda (*dual-sync*) ke Supabase Cloud PostgreSQL dan Google Spreadsheet operasional panitia.
5. Membangun portal admin yang mencakup verifikasi berkas, kontrol saklar pendaftaran dinamis, CMS konten/galeri, dan pengelolaan data master sekolah.

### 1.4 Manfaat
* **Bagi Perusahaan (Jawa Pos Radar Kediri)**:
  - Meningkatkan citra profesionalisme acara dan efisiensi kerja tim panitia KKA.
  - Mengurangi beban verifikasi manual berkas transfer dan data rombongan sekolah hingga lebih dari 70%.
  - Memberikan transparansi arus kas pendaftaran dan jumlah peserta secara *real-time*.
* **Bagi Peserta dan Sekolah Mitra**:
  - Memberikan kemudahan bagi guru untuk mendaftarkan puluhan siswa sekaligus hanya dengan mengunggah satu file Excel resmi.
  - Memudahkan peserta memantau status verifikasi dan mencetak Lembar Tanda Peserta Ujian resmi format A4 ber-QR Code validasi.
* **Bagi Mahasiswa**:
  - Menerapkan teori rekayasa perangkat lunak modern (arsitektur pulau Astro, serverless backend, dan komputasi *client-side*).
  - Meletakkan dasar arsitektur perangkat lunak yang dapat dilanjutkan ke jenjang penelitian Skripsi / Tugas Akhir.

### 1.5 Batasan Masalah
1. Sistem mencakup pendaftaran di 7 wilayah kerja Jawa Pos Radar Kediri (Kota Kediri, Kabupaten Kediri, Kabupaten Nganjuk, Kota Blitar, Kabupaten Blitar, Kabupaten Tulungagung, dan Kabupaten Trenggalek).
2. Jenjang pendidikan yang diakomodasi adalah SD/MI, SMP/MTs, dan SMA/MA/SMK sederajat.
3. Metode pembayaran diarahkan melalui transfer rekening perbankan resmi panitia (Bank BRI Cabang Kediri).
4. Verifikasi pembayaran pada fase magang dilakukan melalui mekanisme unggah bukti transfer yang terhubung dengan modal verifikasi persetujuan di dashboard admin panitia.
5. Otomasi sinkronisasi spreadsheet memanfaatkan webhook Google Apps Script yang terhubung ke lembar kerja resmi divisi Event Radar Kediri.

---

# BAB II: GAMBARAN UMUM PERUSAHAAN

### 2.1 Sejarah Perusahaan
PT Kediri Media Grafika (Jawa Pos Radar Kediri) berdiri sebagai anak perusahaan dari Jawa Pos Group yang memegang peranan strategis dalam menyajikan jurnalisme lokal berkualitas tinggi untuk kawasan eks-Karesidenan Kediri. Berpusat di Kota Kediri, Jawa Pos Radar Kediri telah bertransformasi dari penerbitan surat kabar harian cetak menjadi entitas media multiplatform terpadu yang menaungi koran harian, portal berita online (*radarkediri.jawapos.com*), media sosial, rumah produksi video, dan divisi manajemen kegiatan (*event organizer*).

### 2.2 Aktivitas Bisnis Perusahaan
1. **Penerbitan Surat Kabar Harian**: Harian Radar Kediri, Radar Tulungagung, dan Radar Blitar.
2. **Media Digital & Jurnalistik Daring**: Penyedia berita aktual dan investigatif regional Kediri Raya.
3. **Penyelenggaraan Acara & Event Pendidikan (MICE)**: Menyelenggarakan kegiatan berskala regional dan nasional, seperti Kompetisi Kompetensi Akademik (KKA), School Contest, Radar Kediri Awards, pameran UMKM, dan lomba literasi pelajar.

### 2.3 Penerapan Teknologi di Perusahaan
Dalam menjalankan operasionalnya, PT Kediri Media Grafika memanfaatkan:
* Sistem Redaksi Terdistribusi (Editorial CMS) untuk produksi berita harian.
* Infrastruktur Cloud Hosting dan Content Delivery Network (CDN) untuk sirkulasi portal berita online.
* Platform integrasi digital untuk pendaftaran dan pencatatan peserta event massal seperti KKA.

### 2.4 Data Pembimbing Lapangan
* **Instansi**: PT Kediri Media Grafika (Jawa Pos Radar Kediri)
* **Alamat Kantor**: Jl. Brawijaya No. 27D, Kelurahan Pocanan, Kota Kediri, Jawa Timur
* **Divisi Tempat Magang**: Divisi Event Organizer & Digital Technology Development
* **Pembimbing Lapangan**: Koordinator Divisi Event / IT Panitia KKA Jawa Pos Radar Kediri

---

# BAB III: PELAKSANAAN

### 3.1 Waktu dan Tempat Kegiatan
* **Waktu Pelaksanaan**: Semester Ganjil / Periode Magang Industri 2026
* **Tempat Pelaksanaan**: Kantor Pusat PT Kediri Media Grafika, Jl. Brawijaya No. 27D, Kota Kediri & Implementasi Lapangan / Hybrid Remote Environment.

### 3.2 Jadwal Kegiatan
1. **Minggu 1 - 2**: Studi Literatur, Analisis Kebutuhan Sistem Pendaftaran KKA, Wawancara Alur Kerja dengan Tim Panitia Radar Kediri, serta Pengumpulan Database Sekolah Kemendikbud.
2. **Minggu 3 - 4**: Perancangan Arsitektur Sistem, Desain UI/UX Korporat Menggunakan Bootstrap 5.3, Pembuatan Komponen Astro, dan Konfigurasi Skema Database Supabase.
3. **Minggu 5 - 6**: Pengembangan Frontend Publik (Beranda, Informasi, Regulasi, Direktori 7 Wilayah, dan Kontak Hotline).
4. **Minggu 7 - 8**: Pengembangan Modul Pendaftaran Mandiri, Logika Gelombang Dinamis, Validasi NISN Ganda, dan Integrasi WhatsApp Formatter.
5. **Minggu 9 - 10**: Pengembangan Modul Pendaftaran Kolektif Sekolah (321 Template Excel Resmi Kecamatan, Parsing SheetJS di Sisi Klien, dan Tabel Pratinjau Cerdas).
6. **Minggu 11 - 12**: Pembangunan Dashboard Admin (Modul Verifikasi Pembayaran, Kontrol Saklar Pendaftaran, Pengaturan Tarif Dinamis, dan Manajemen 5.671 Sekolah).
7. **Minggu 13 - 14**: Integrasi Dual-Sync Supabase dan Google Apps Script, Pengujian Fungsional (Black Box Testing), Audit Kinerja (Lighthouse 90+), Deployment ke Domain Produksi, dan Penyusunan Dokumentasi Akhir.

### 3.3 Instrumen Penggalian Data & Wawancara Lapangan
Dalam menganalisis kebutuhan operasional panitia, disusun 7 instrumen wawancara terstruktur kepada Pembimbing Lapangan di PT Kediri Media Grafika:
1. **Verifikasi Format Mutasi Rekening Bank BRI Panitia**: Memeriksa struktur berkas CSV hasil unduhan Internet Banking BRI (`MUTASI_KREDIT`, `TGL_TRAN`, `JAM_TRAN`, `DESK_TRAN`) untuk perancangan algoritma pencocokan otomatis.
2. **Karakteristik Transaksi Antarbank (BI-FAST)**: Memvalidasi bahwa nominal transfer masuk di mutasi BRI tetap utuh sampai rupiah terakhir tanpa terpotong biaya admin bank pengirim (Rp 2.500).
3. **Pola Jam Operasional Verifikasi Panitia**: Menentukan skema *Asynchronous Batch Processing* agar admin bebas mengunggah mutasi kapan saja tanpa terikat jam kaku (jam 18, 20, 21, atau lebih dari sekali sehari).
4. **Kebutuhan Dokumen Proforma Dana BOS Sekolah**: Mengonfirmasi perlunya fitur cetak invoice/kwitansi proforma berstempel resmi panitia untuk kebutuhan SPJ bendahara sekolah negeri.
5. **Format Berkas Hasil Koreksi Ujian Tertulis LJK**: Memeriksa struktur berkas Excel hasil pemindaian LJK juri untuk pemetaan kolom pada modul import nilai massal.
6. **Kebijakan Etika & Privasi Nilai Siswa**: Mengonfirmasi bahwa leaderboard publik hanya memuat Top 10/Finalis, sementara nilai lengkap siswa diakses privat via NISN.
7. **Rencana Wilayah Ekspansi Baru**: Menjaring kandidat kabupaten/kota baru untuk pengujian fitur multi-wilayah dinamis.

---

# BAB IV: HASIL DAN PEMBAHASAN

### 4.1 Hasil Sistem yang Telah Dikembangkan

Proyek magang ini menghasilkan sistem portal pendaftaran berbasis web terpadu dengan domain target `kkaradarkediri.com` yang terdiri dari **46 Halaman HTML Statis Berkinerja Tinggi** dan **1 Portal Dashboard Administrator Suite**.

Arsitektur sistem dibangun di atas fondasi:
* **Astro 5 (Islands Architecture)**: Mengompilasi seluruh halaman publik menjadi HTML statis murni yang dimuat dalam hitungan milidetik (*zero-JS by default*), hanya memuat script JavaScript interaktif pada komponen formulir pendaftaran.
* **Bootstrap 5.3 + Custom Corporate Tokens**: Desain responsif berpalet warna resmi (*Deep Navy Blue* `#0f2d59`, *Amber/Gold* `#f59e0b`, dan *Emerald Green* `#10b981`), tipografi *Plus Jakarta Sans*, dan elemen grafis profesional (bebas dari emoji artifisial).
* **Supabase Cloud (PostgreSQL)**: Menyimpan entitas data transaksional (`registrations`, `participants`, `schools`, `app_settings`) dan bucket penyimpanan berkas bukti transfer.
* **Google Apps Script Webhook**: Menghubungkan transaksi baru secara instan ke Google Spreadsheet panitia yang otomatis terpecah rapi ke sheet per wilayah.
* **SheetJS (xlsx)**: Memproses pembacaan dan validasi berkas Excel langsung di peramban pendaftar tanpa membebani server backend.

---

### 4.2 Pembahasan Rinci Modul Sistem

Sistem terbagi menjadi dua ranah utama: **Sistem Pengguna Publik (Frontend)** dan **Sistem Administrator Panitia (Backend Suite)**.

#### A. SUBSISTEM PENGGUNA PUBLIK (CUSTOMER-FACING)

```
[ PENGGUNA PUBLIK ]
       │
       ├───► 1. Halaman Beranda & Profil (Hero, Statistik, 7 Wilayah, FAQ, WA FAB)
       │
       ├───► 2. Form Pendaftaran Mandiri (/daftar/mandiri)
       │         - Multi-siswa dinamis
       │         - Filter Sekolah Kemendikbud per Jenjang & Kecamatan
       │         - Validasi Keunikan NISN Anti-Duplikat
       │         - Tagihan Dinamis per Gelombang
       │         - Unggah Struk BRI & Notifikasi WhatsApp Formal
       │
       ├───► 3. Form Pendaftaran Kolektif Sekolah (/daftar/kolektif)
       │         - Unduh Template Excel Resmi per Kecamatan (321 Variasi)
       │         - Parsing SheetJS di Browser
       │         - Pratinjau Tabel & Deteksi Dini Baris NISN Ganda
       │         - Validasi Pre-Submit Cloud
       │         - Kalkulasi Tagihan Rombongan Massal
       │
       └───► 4. Cek Status & Cetak Lembar Bukti (/daftar/status & /daftar/cetak)
                 - Pencarian Kode Registrasi / No. WA
                 - Cetak Lembar Resmi A4 (Kop Radar Kediri, QR Code, Stempel)
```

##### 1. Beranda Interaktif & Navigasi Terpadu (`/`)
* **Hero Banner Adaptif**: Header transparan pada posisi atas halaman yang bertransisi secara mulus menjadi bilah putih bersih solid (`#ffffff`) saat digulir ke bawah.
* **Counter Statistik Prestasi**: Menampilkan 4 metrik historis resmi KKA (5.633+ Peserta, 7 Wilayah Kompetisi, 350+ Sekolah Terdaftar, dan Medali/Tabungan Pembinaan).
* **Direktori 7 Wilayah Karesidenan Kediri**: Grid simetris memuat lambang resmi daerah berkualitas WebP ultra-ringan (< 25 KB) untuk Kota Kediri, Kab. Kediri, Nganjuk, Kota Blitar, Kab. Blitar, Tulungagung, dan Trenggalek.
* **Pemberitahuan Gelombang & Tarif Dinamis**: Tabel biaya pendaftaran terhidrasi langsung dari konfigurasi admin (Gelombang 1 Early Bird, Gelombang 2 Reguler, dan Gelombang 3 Last Call).
* **Floating WhatsApp Hub**: Tombol bantuan cepat mengambang di pojok layar dengan opsi 3 admin wilayah resmi.

##### 2. Modul Pendaftaran Mandiri (`/daftar/mandiri`)
* **Pengisian Multi-Siswa**: Orang tua/wali dapat mendaftarkan lebih dari satu anak dalam satu sesi transaksi dengan mengklik tombol *"Tambah Siswa Peserta"*.
* **Sinkronisasi Database 5.671 Sekolah**: Dropdown nama sekolah secara otomatis menyaring sekolah yang relevan berdasarkan wilayah karesidenan, kecamatan, dan jenjang yang dipilih. Terdapat opsi *"Sekolah Lainnya (+ Ketik Manual)"* sebagai fallback keamanan.
* **Mesin Validasi Keunikan NISN**: Sebelum data dikirim ke Supabase, sistem menjalankan fungsi `checkNisnDuplicates()`:
  - Memeriksa apakah ada nomor NISN yang sama diinput lebih dari sekali dalam formulir.
  - Mengkueri pangkalan data Cloud Supabase untuk memastikan NISN tersebut belum pernah didaftarkan sebelumnya oleh pendaftar lain.
  - Jika NISN duplikat ditemukan, pendaftaran ditolak seketika dan muncul **Modal Peringatan Profesional** yang merinci NISN ganda, nama peserta, sekolah, dan kode registrasi yang sudah tercatat.
  - *Catatan Cerdas*: Jika pendaftaran sebelumnya berstatus `'ditolak'`, NISN tersebut secara otomatis diperbolehkan mendaftar kembali.
* **Rincian Pembayaran Resmi**: Menampilkan instruksi transfer ke satu-satunya rekening resmi: **Bank BRI Cabang Kediri (0033-0100-3455-560 a.n. Heri Muda Setiawan)** dengan tombol salin instan.
* **Konfirmasi WhatsApp Formal**: Menghasilkan draf pesan konfirmasi pendaftaran berstandar korespondensi resmi bebas emoji yang dilengkapi penanda waktu pendaftaran (*Hari, Tanggal Bulan Tahun | HH.mm WIB*).

##### 3. Modul Pendaftaran Kolektif Sekolah Berbasis Excel (`/daftar/kolektif`)
* **Unduh Template Excel Resmi per Kecamatan**: Guru memilih Jenjang dan Kecamatan sekolah, sistem langsung menyediakan file Excel spesifik (contoh: `KKA Kolektif SMA-MA-SMK Kecamatan Kota.xlsx`) yang telah memuat format kolom dan metadata wilayah yang sah.
* **Client-Side Parsing via SheetJS**: Guru mengunggah file Excel melalui area *drag-and-drop*. SheetJS mengekstrak data nama, NISN, jenis kelamin, dan kelas dalam waktu kurang dari 1 detik tanpa mengunggah file mentah ke server.
* **Deteksi Dini Visual Baris Ganda**: Jika di dalam file Excel terdapat siswa dengan nomor NISN yang sama, tabel pratinjau langsung menyorot baris tersebut dengan warna merah (`table-danger`) dan lencana peringatan *"NISN Ganda di Excel"*.
* **Validasi Pre-Submit Cloud**: Memastikan seluruh siswa di dalam file Excel belum terdaftar di database panitia sebelum tombol kirim diaktifkan.
* **Kalkulasi Biaya Rombongan**: Menghitung secara otomatis total pembayaran: $\text{Tarif Gelombang} \times \text{Jumlah Siswa Terbaca}$.

##### 4. Modul Cek Status & Cetak Lembar Bukti Pendaftaran A4 (`/daftar/status` & `/daftar/cetak`)
* **Pencarian Real-Time**: Peserta/guru memasukkan Kode Registrasi (misal: `KKA26-M-KOT-7491`) atau Nomor WhatsApp untuk melihat status verifikasi (*Menunggu Verifikasi*, *Terverifikasi / Lunas*, atau *Ditolak / Butuh Revisi*).
* **Lembar Cetak Tanda Peserta Resmi A4**: Dokumen siap cetak/simpan PDF berstandar formal yang mencakup kop surat resmi Jawa Pos Radar Kediri, stempel dinamis lunas, rincian biaya, tabel daftar siswa peserta, tanda tangan digital Ketua Panitia KKA, serta QR Code verifikasi.

---

#### B. SUBSISTEM ADMINISTRATOR PANITIA (ADMIN SUITE `/admin`)

```
[ PORTAL ADMIN PANITIA ]
       │
       ├───► 1. Login Autentikasi Panitia (/admin/login)
       │
       ├───► 2. Dashboard KPI & Statistik (/admin)
       │         - Metrik total pendaftar, grafik wilayah, estimasi kas masuk
       │
       ├───► 3. Manajemen Pendaftaran (/admin/pendaftaran)
       │         - Filter multi-kriteria & pencarian instan
       │         - Modal Verifikasi: Bukti transfer, daftar siswa
       │         - Aksi Persetujuan: Setujui Lunas / Tolak Revisi + WA Notifier
       │
       ├───► 4. Kontrol Saklar & Pengaturan Sistem (/admin/pengaturan)
       │         - Saklar ON/OFF pendaftaran Mandiri & Kolektif
       │         - Pengaturan Gelombang Aktif (1, 2, 3) & Tarif Dinamis
       │         - Konfigurasi Rekening BRI & Nomor CS Panitia
       │
       └───► 5. Manajemen Master 5.671 Sekolah (/admin/sekolah)
                 - Pengelolaan database sekolah se-Karesidenan Kediri
                 - In-Place Update berbasis ID unik permanen (sch_...)
                 - Tambah sekolah baru & hapus sekolah
```

##### 1. Dashboard Ringkasan KPI (`/admin`)
* Menampilkan kartu metrik utama: Total Pendaftar Mandiri, Total Pendaftar Kolektif, Total Siswa Terdaftar, dan Estimasi Total Dana Masuk.
* Grafik batang persebaran pendaftar di 7 wilayah karesidenan Kediri.

##### 2. Manajemen Verifikasi Pendaftaran (`/admin/pendaftaran`)
* **Tabel Transaksi Dinamis**: Memuat seluruh data pendaftaran dari database Supabase dengan filter berdasarkan status pendaftaran, wilayah, dan metode pendaftaran.
* **Modal Inspeksi Bukti Transfer**: Panitia dapat memperbesar foto bukti struk transfer pembayaran yang diunggah peserta untuk memastikan keaslian transaksi.
* **Tombol Aksi & Generator Pesan WhatsApp**:
  - Tombol **Setujui (Lunas)**: Mengubah status transaksi menjadi terverifikasi lunas, mencatat waktu persetujuan, dan mengizinkan peserta mencetak kartu ujian.
  - Tombol **Tolak (Revisi)**: Menyediakan pilihan template alasan penolakan (misal: *Nominal transfer kurang*, *Bukti buram*, *Salah rekening tujuan*) serta tautan otomatis untuk menghubungi WhatsApp peserta secara personal.

##### 3. Pengaturan Sistem & Saklar Pendaftaran (`/admin/pengaturan`)
* **Master Saklar Pendaftaran (*Kill Switch*)**: Panitia dapat menutup pendaftaran Mandiri atau Kolektif sewaktu-waktu secara terpisah dengan label *"Pendaftaran Ditutup"* atau *"Kuota Penuh"*. Halaman formulir publik seketika beralih menampilkan pesan penutupan resmi.
* **Konfigurasi Gelombang & Biaya CBT**: Panitia dapat memilih gelombang mana yang sedang aktif (Gelombang 1, 2, atau 3), menyetel rentang tanggal periode, dan mengubah nominal biaya ujian per jenjang (SD, SMP, SMA). Seluruh halaman website (Beranda, Tabel Biaya, Halaman Wilayah, dan Form Pendaftaran) langsung tersinkronisasi secara otomatis.
* **Manajemen Rekening & Kontak CS**: Pengaturan nomor rekening Bank BRI, nama pemilik rekening, dan 3 nomor hotline WhatsApp panitia terpusat.

##### 4. Manajemen Pangkalan Data Sekolah (`/admin/sekolah`)
* Mengelola database **5.671 sekolah Kemendikbud** se-Karesidenan Kediri.
* Menggunakan arsitektur pembaruan data di tempat (*in-place update*) berbasis ID permanen (`sch_...`) dari Supabase, sehingga admin dapat mengedit nama sekolah atau menambah sekolah baru tanpa memicu duplikasi data.

---

### 4.3 Alur Data dan Arsitektur Sinkronisasi Ganda (Dual-Sync)

```
[ Pendaftar Mengisi Form di Web ]
                │
                ▼
[ Validasi Klien: Duplikasi NISN & Berkas ]
                │
                ▼
       [ Transaksi Dikirim ]
        ┌───────┴───────┐
        ▼               ▼
[ Supabase PostgreSQL ]   [ Google Apps Script Webhook ]
- Tabel: registrations   - Parsing JSON Transaksi
- Tabel: participants    - Concurrency Lock (Mutex)
- Bucket: proof-transfers - Tulis Baris ke Google Sheets (Tab per Wilayah)
        │
        ▼
[ Admin Melakukan Verifikasi di /admin/pendaftaran ]
        │
        ├─► Jika Setuju: Status 'terverifikasi', Terbit Kartu Ujian Ber-QR
        └─► Jika Tolak : Status 'ditolak', Kirim WhatsApp Revisi Bukti
```

---

# BAB V: PENUTUP

### 5.1 Kesimpulan
1. Telah berhasil dibangun portal web pendaftaran resmi Kompetisi Kompetensi Akademik (KKA) 2026 Jawa Pos Radar Kediri dengan arsitektur modern berbasis Astro 5, Bootstrap 5.3, dan Supabase Cloud.
2. Modul pendaftaran kolektif berbasis SheetJS terbukti mampu membaca dan memvalidasi berkas pendaftaran rombongan sekolah dari 321 variasi kecamatan secara instan di sisi klien, menghilangkan kerumitan input satu per satu.
3. Mesin validasi keunikan NISN (*strict duplicate validation*) berhasil mencegah penggandaan nomor kepesertaan siswa baik di tingkat formulir mandiri maupun berkas Excel massal.
4. Dashboard admin memberikan kendali penuh bagi panitia Radar Kediri untuk memverifikasi bukti pembayaran, mengelola saklar pendaftaran dan tarif gelombang dinamis, serta mengelola pangkalan data 5.671 sekolah.
5. Hasil pengujian menunjukkan seluruh 46 halaman statis terkompilasi sempurna (0 error, 0 warning) dengan waktu bangun 3,5 detik dan siap menampung ribuan pendaftar KKA 2026.

### 5.2 Saran & Rencana Lanjutan ke Skripsi (Tugas Akhir)
Berdasarkan hasil evaluasi sistem dan masukan dari Pembimbing Lapangan di Jawa Pos Radar Kediri, sistem fondasi magang ini memiliki landasan kuat untuk dikembangkan lebih lanjut menjadi penelitian **Skripsi / Tugas Akhir** menggunakan **Metode Incremental** dengan fokus pada 3 pengembangan utama:
1. **Modul In-House Billing Engine & Rekonsiliasi Kode Unik Dinamis (*Increment 2*)**: 
   - Mengembangkan verifikasi pembayaran otomatis berbasis *Dynamic Unique Code 3-Digit* dengan batas waktu (*Time-To-Live / TTL 24 Jam*) dan *Masa Karantina Kode (Cooling-Off Period 24 Jam)*, sehingga transaksi perbankan dapat terverifikasi otomatis tanpa memerlukan payment gateway pihak ketiga berbayar (menghemat puluhan juta rupiah bagi panitia).
   - Menjamin skalabilitas kuota tak terbatas hingga **20.000 peserta** (melalui efisiensi pendaftaran kolektif sekolah dan fitur *Dynamic Digit Scaling 4-Digit* berkapasitas 27.000 transaksi harian), serta dilengkapi algoritma pemulihan otomatis untuk peserta yang telat bayar (*late payment recovery*).
   - Mengintegrasikan parser mutasi resmi Bank BRI (`MUTASI_KREDIT`, `TGL_TRAN`, `JAM_TRAN`, `DESK_TRAN`) yang bersifat *Idempotent* dan *Asynchronous* (panitia bebas mengunggah mutasi jam berapa pun tanpa risiko duplikasi transaksi).
   - Kebal terhadap biaya admin antarbank (BI-FAST Rp 2.500), menyediakan fitur cetak **Kwitansi Proforma SPJ Resmi** untuk sekolah negeri, dan alur notifikasi WhatsApp pelunasan semi-otomatis satu-klik panitia (Opsi A).
2. **Modul Manajemen Multi-Wilayah Dinamis (*Increment 3 - Bagian A*)**: 
   - Mengakomodasi permintaan pembimbing magang agar panitia dapat menambah wilayah dan kabupaten baru di luar Karesidenan Kediri langsung dari dashboard admin secara fleksibel (*zero hardcoding*).
3. **Sistem Pengumuman Hasil Ujian Tertulis LJK & E-Sertifikat Digital (*Increment 3 - Bagian B*)**: 
   - Karena ujian KKA diselenggarakan secara tertulis manual di atas kertas/LJK, website dikembangkan sebagai portal resmi publikasi nilai massal dari Excel, pengecekan rapor nilai mandiri berbasis NISN/No. Peserta, papan peringkat (*leaderboard*) regional, serta penerbitan piagam/e-sertifikat resmi ber-QR Code untuk syarat pendaftaran jalur prestasi PPDB siswa.

---

# DAFTAR PUSTAKA
1. Pressman, R. S., & Maxim, B. R. (2020). *Software Engineering: A Practitioner's Approach (9th ed.)*. McGraw-Hill Education.
2. Sommerville, I. (2016). *Software Engineering (10th ed.)*. Pearson Education.
3. Astro Documentation. (2026). *Islands Architecture and Static Site Generation*. Diakses dari https://docs.astro.build.
4. Supabase Community. (2026). *PostgreSQL Database and Row Level Security Guide*. Diakses dari https://supabase.com/docs.
5. SheetJS LLC. (2026). *Client-Side Spreadsheet Data Processing (XLSX)*. Diakses dari https://docs.sheetjs.com.

---

# LAMPIRAN
* **Lampiran 1**: Struktur Skema Database Supabase (`registrations`, `participants`, `schools`, `app_settings`).
* **Lampiran 2**: Tampilan Antarmuka Halaman Publik (Beranda, Form Mandiri, Form Kolektif, Cek Status).
* **Lampiran 3**: Tampilan Antarmuka Dashboard Administrator (`/admin`, `/admin/pendaftaran`, `/admin/pengaturan`, `/admin/sekolah`).
* **Lampiran 4**: Contoh Format Surat Resmi Lembar Bukti Tanda Peserta Ujian Format A4 Ber-QR Code.
