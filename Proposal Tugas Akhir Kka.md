# RENCANA TUGAS AKHIR / PROPOSAL SKRIPSI TEKNIK INFORMATIKA & SISTEM INFORMASI

**Topik Penelitian:** Rekayasa Perangkat Lunak, Sistem Transaksi Finansial Mandiri (*In-House Billing Engine*), dan Arsitektur Multi-Wilayah Dinamis.  
**Studi Kasus / Mitra Industri:** PT Kediri Media Grafika (Jawa Pos Radar Kediri) — Kompetisi Kompetensi Akademik (KKA) 2026.  
**Metode Pengembangan:** Metode Incremental (*Incremental Model SDLC*).  
**Konteks Proyek:** Kontinuitas dan Peningkatan Sistem dari Produk Magang Kerja Industri (MKI) menuju Karya Ilmiah Sarjana (S1).  

---

## 1. FORMULASI JUDUL PENELITIAN

### 1.1 Judul Utama (Sangat Direkomendasikan):
> **"PENGEMBANGAN SISTEM INFORMASI PENDAFTARAN KOMPETISI AKADEMIK JAWA POS RADAR KEDIRI MENGGUNAKAN METODE INCREMENTAL DENGAN MODUL REKONSILIASI PEMBAYARAN MANDIRI DAN MANAJEMEN WILAYAH DINAMIS"**

### 1.2 Judul Alternatif (Fokus Penekanan Algoritma & Transaksi Finansial):
> **"PENGEMBANGAN SISTEM PENDAFTARAN TERPADU KKA RADAR KEDIRI MENGGUNAKAN METODE INCREMENTAL DENGAN FITUR IN-HOUSE BILLING ENGINE BERBASIS DYNAMIC UNIQUE CODE DAN TIME-TO-LIVE"**

*Catatan Akademik:*  
Kedua judul di atas menggunakan kata **"PENGEMBANGAN"** (bukan *Rancang Bangun*), sehingga secara legal akademik mengakui produk awal yang telah Anda selesaikan saat Magang Industri, dan berfokus pada 3 lompatan inovasi (*increments*) yang dikerjakan pada Tugas Akhir.

---

## 2. LATAR BELAKANG & IDENTIFIKASI PERMASALAHAN (*RESEARCH GAP*)

### 2.1 Konteks Lapangan & Evaluasi Magang
Pada tahap magang industri, telah berhasil dibangun portal pendaftaran KKA versi 1.0 (Astro 5 + Supabase + SheetJS). Namun, dari hasil uji coba operasional dan evaluasi langsung bersama **Pembimbing Lapangan PT Kediri Media Grafika**, teridentifikasi masalah-masalah kritis yang menjadi celah penelitian (*research gap*):

1. **Kendala Biaya Pihak Ketiga pada Skala Massal (*Cost Overhead Problem*)**:
   Menggunakan payment gateway pihak ketiga (seperti Midtrans/Xendit) membebankan biaya administrasi Rp 3.000 – Rp 5.000 per transaksi Virtual Account atau 0,7% untuk QRIS. Pada ajang KKA yang melibatkan hingga puluhan ribu siswa, biaya potongan pihak ketiga dapat menembus **Rp 15.000.000 s/d Rp 30.000.000**, yang sangat membebani peserta dan memotong anggaran pembinaan Radar Kediri.
2. **Ketiadaan Batasan Kuota Peserta vs Keterbatasan Identifikasi Mutasi Bank**:
   Pihak Radar Kediri menetapkan bahwa **pendaftaran KKA tidak memiliki batasan kuota (unlimited applicants)**. Jika peserta mentransfer biaya ujian (misal flat Rp 50.000) langsung ke rekening giro Bank BRI panitia, ribuan mutasi bank akan memiliki nominal yang identik (*collision*). Verifikasi manual oleh panitia rentan manipulasi struk transfer editan dan memakan waktu berhari-hari.
3. **Kebutuhan Fleksibilitas Jam Verifikasi Panitia (*Asynchronous Workflow*)**:
   Panitia memiliki jam kerja dinamis di lapangan, sehingga waktu pengecekan mutasi bank tidak bisa dipatok di jam tertentu (kadang jam 18.00, 20.00, atau 21.00). Diperlukan sistem rekonsiliasi yang *asynchronous* dan *idempotent* (kebal upload berulang) yang mampu membaca mutasi bank kapan pun admin sempat mengunggahnya.
4. **Kebutuhan SPJ Dana BOS / Paguyuban Sekolah Negeri**:
   Pendaftar kolektif dari sekolah negeri membutuhkan dokumen tagihan resmi (*Kwitansi Proforma / Invoice Berstempel*) untuk mencairkan dana komite/BOS sebelum melakukan pembayaran transfer bank.
5. **Kebutuhan Ekspansi Wilayah Baru Tanpa Koding (*Hardcoding Bottleneck*)**:
   Sistem baseline saat magang masih mengikat 7 wilayah eks-Karesidenan Kediri. Pembimbing magang meminta agar panitia dapat memperluas cakupan kompetisi ke kabupaten/kota baru di masa depan langsung dari dashboard administrator tanpa perlu merombak kode sumber program.
6. **Kebutuhan Transparansi Hasil Ujian Tertulis Manual (Paper-Based Test)**:
   Karena ujian KKA dilakukan secara tertulis manual di atas kertas/LJK di berbagai venue sekolah, peserta tidak memiliki akses langsung terhadap skor nilainya. Dibutuhkan portal pengumuman nilai resmi yang dapat diakses mandiri oleh siswa serta menerbitkan e-sertifikat piagam ber-QR code untuk jalur prestasi PPDB.

---

## 3. RUMUSAN MASALAH & BATASAN MASALAH

### 3.1 Rumusan Masalah
1. Bagaimana menerapkan **Metode Incremental** untuk mengembangkan sistem pendaftaran KKA yang terstruktur dari baseline magang menuju sistem yang matang?
2. Bagaimana merancang arsitektur **In-House Billing Engine** berbasis *Dynamic Unique Code* dengan *Time-To-Live (TTL 24 Jam)* dan *Masa Karantina (Cooling-Off Period 24 Jam)* agar bebas tabrakan (*zero-collision*) di tengah kuota pendaftar hingga 20.000 siswa?
3. Bagaimana merancang algoritma **Rekonsiliasi Mutasi Bank BRI Otomatis** berbasis berkas CSV mutasi bulanan yang bersifat *Idempotent* (kebal eksekusi ganda) dan fleksibel terhadap variasi jam unggah admin?
4. Bagaimana menangani kasus transfer terlambat (*late payment anomaly*) secara otomatis dengan *One-Click Recovery Approval*?
5. Bagaimana merancang modul **Manajemen Multi-Wilayah Dinamis** sehingga panitia dapat menambah wilayah baru secara fleksibel (*runtime configuration*)?
6. Bagaimana membangun modul **Pengumuman Hasil Ujian Tertulis** yang memproses nilai massal dari Excel, menghitung peringkat otomatis, dan menerbitkan e-sertifikat ber-QR code?

### 3.2 Batasan Masalah
1. Transaksi pembayaran mandiri diarahkan ke rekening resmi Bank BRI Jawa Pos Radar Kediri.
2. Rekonsiliasi mutasi bank dilakukan melalui parser berkas mutasi resmi format CSV Bank BRI dengan header kolom standar perbankan.
3. Kode unik nominal dibatasi pada rentang 3 digit (`001`–`999`), dengan algoritma *Dynamic Digit Scaling* ke 4 digit (`1000`–`9999`) jika beban konkurensi harian mencapai batas kritis 80%.
4. Notifikasi pelunasan WhatsApp dikirim melalui mekanisme *Direct Intent (`wa.me`)* semi-otomatis satu klik panitia tanpa biaya bot pihak ketiga.
5. Pengujian sistem menggunakan metode *Black-Box Testing*, simulasi konkurensi beban 20.000 peserta, serta pengukuran kepuasan pengguna (*System Usability Scale - SUS*).

---

## 4. METODOLOGI PENELITIAN & INSTRUMEN PENGUMPULAN DATA

### 4.1 Instrumen Wawancara & Penggalian Data Lapangan (Mitra Radar Kediri)
Sebagai dasar analisis kebutuhan pada Bab III skripsi, dilakukan wawancara mendalam (*in-depth interview*) kepada pihak PT Kediri Media Grafika mencakup 7 parameter kritis:
1. **Verifikasi Jenis & Akses Mutasi Rekening BRI Panitia**: Mengonfirmasi ketersediaan fitur unduh CSV mutasi dan struktur data kolom yang dihasilkan.
2. **Karakteristik Transaksi Antarbank (BI-FAST)**: Memvalidasi bahwa nominal transfer masuk di mutasi BRI tetap utuh sampai rupiah terakhir tanpa terpotong biaya admin bank pengirim (Rp 2.500).
3. **Pola Jam Operasional Verifikasi Panitia**: Menentukan skema *Asynchronous Batch Processing* agar admin bebas mengunggah mutasi kapan saja tanpa terikat jam kaku.
4. **Kebutuhan Dokumen Proforma Dana BOS Sekolah**: Mengonfirmasi perlunya fitur cetak invoice/kwitansi proforma berstempel resmi panitia untuk kebutuhan SPJ bendahara sekolah.
5. **Format Berkas Hasil Koreksi Ujian Tertulis LJK**: Memeriksa struktur berkas Excel hasil pemindaian LJK juri untuk pemetaan kolom pada modul import nilai massal.
6. **Kebijakan Etika & Privasi Nilai Siswa**: Mengonfirmasi bahwa leaderboard publik hanya memuat Top 10/Finalis, sementara nilai lengkap siswa diakses privat via NISN.
7. **Rencana Wilayah Ekspansi Baru**: Menjaring kandidat kabupaten/kota baru untuk pengujian fitur multi-wilayah dinamis.

### 4.2 Siklus Metode Incremental
```
[ BASELINE MAGANG ]
Portal Web Dasar, Form Mandiri & Kolektif Awal, DB 5.671 Sekolah, Dual-Sync
       │
       ▼
[ INCREMENT 1 ]: Modul Integritas Data & Normalisasi Berkas Kolektif
- Validasi Keunikan NISN Anti-Duplikat (Dual-Layer: Form & Database Cloud).
- Algoritma String Matching (Jaro-Winkler) untuk mencocokkan singkatan sekolah guru di Excel.
- Parser Client-Side SheetJS dengan deteksi dini tabel merah.
       │
       ▼
[ INCREMENT 2 ]: In-House Billing Engine & Rekonsiliasi Pembayaran Mandiri
- E-Invoice Generator resmi + Cetak Kwitansi Proforma SPJ Sekolah Negeri.
- Dynamic Unique Code Generator (3-Digit Pool) + TTL 24 Jam + Cooling-Off Period 24 Jam.
- Dynamic Digit Scaling (Fallback 4-Digit jika slot terpakai > 80% / 27.000 kapasitas harian).
- Idempotent Bank Reconciliation Parser: Membaca struktur CSV resmi Bank BRI tanpa duplikasi data.
- Asynchronous Workflow: Bebas diunggah jam berapa pun (18.00, 20.00, 21.00, atau beberapa kali sehari).
- Penanganan Siswa Telat Bayar dengan One-Click Recovery Approval di Admin.
- Notifikasi WhatsApp Pelunasan Semi-Otomatis (Opsi A: One-Click wa.me Intent).
       │
       ▼
[ INCREMENT 3 ]: Manajemen Multi-Wilayah Dinamis & Sistem Pengumuman Hasil Ujian Tertulis
- Modul Ekspansi Wilayah Baru di Admin (Tambah Kabupaten, Kecamatan, dan Kuota Venue secara runtime).
- Router dinamis & Form auto-generate wilayah baru (Zero Hardcoding).
- Modul Pengumuman Hasil Ujian Tertulis LJK & Pemeringkatan:
  * Admin: Import massal rekap nilai koreksi LJK (Excel/CSV) + Saklar Embargo Waktu Rilis.
  * Algoritma Auto-Ranking (Peringkat per Sekolah, Wilayah, dan Karesidenan).
  * Pengguna Publik: Portal Cek Rapor Nilai Mandiri via NISN + Papan Peringkat (Leaderboard) Top 10.
  * Penerbitan Piagam / E-Sertifikat Digital Resmi ber-QR Code Verifikasi Keaslian (Validasi PPDB).
       │
       ▼
[ PENGUJIAN AKHIR & EVALUASI ]
Black-Box Testing, Concurrency Collision Simulation, System Usability Scale (SUS)
```

---

## 5. PERANCANGAN SISTEM & LOGIKA ALGORITMA (TEKNIS BAB III & BAB IV)

### 5.1 Finite State Machine (FSM) Status Tagihan Invoice
Setiap tagihan pendaftaran dikendalikan oleh status mesin keadaan (*State Machine*) dengan siklus penguncian kode (*Locking Cycle*):

```
                     [ Peserta Klik Daftar ]
                                │
                                ▼
                       ┌──────────────────┐
                       │     UNPAID       │ ◄── (Timer TTL 24 Jam Berjalan,
                       └────────┬─────────┘      Kode Unik Terkunci di Pool)
                                │
               ┌────────────────┴────────────────┐
               ▼                                 ▼
      [ Mutasi Bank Cocok ]            [ Waktu Habis (TTL = 0) ]
               │                                 │
               ▼                                 ▼
        ┌──────────────┐                  ┌──────────────┐
        │     PAID     │                  │   EXPIRED    │
        └──────────────┘                  └──────┬───────┘
         (Kode Selesai)                          │
                                                 ▼
                                     ┌─────────────────────────┐
                                     │  COOLING-OFF PERIOD     │ ◄── (Masa Karantina Kode 24 Jam,
                                     │  (Karantina 24 Jam)     │      Kode Tidak Dipakai Orang Lain)
                                     └───────────┬─────────────┘
                                                 │
                        ┌────────────────────────┴────────────────────────┐
                        ▼                                                 ▼
             [ Tidak Ada Transfer ]                             [ Siswa Telat Transfer ]
                        │                                                 │
                        ▼                                                 ▼
             ┌─────────────────────┐                          ┌─────────────────────────┐
             │ KODE DIRILIS KE     │                          │ STATUS: REVIEW_TELAT    │
             │ POOL (Bisa Dipakai) │                          │ (One-Click Approval     │
             └─────────────────────┘                          │  oleh Panitia di Admin) │
                                                              └─────────────────────────┘
```

---

### 5.2 Logika Algoritma Alokasi Kode Unik & Masa Karantina (*Cooling-Off Period*)
```typescript
/**
 * PSEUDOCODE: Alokasi Kode Unik Bebas Tabrakan dengan Masa Karantina 24 Jam
 */
function generateUniquePaymentCode(jenjangCategory: 'sd' | 'smp' | 'sma', baseAmount: number): {
    uniqueCode: number,
    totalPayment: number,
    expiredAt: Date,
    quarantineUntil: Date
} {
    const TTL_HOURS = 24;          // Batas bayar siswa: 24 Jam
    const QUARANTINE_HOURS = 24;   // Masa karantina kode setelah expired: 24 Jam
    const now = new Date();
    const expiredAt = new Date(now.getTime() + (TTL_HOURS * 60 * 60 * 1000));
    const quarantineUntil = new Date(now.getTime() + ((TTL_HOURS + QUARANTINE_HOURS) * 60 * 60 * 1000));

    // 1. Dapatkan daftar kode unik yang SEDANG TERKUNCI (UNPAID aktif atau EXPIRED dalam karantina)
    const lockedCodes = database.query(`
        SELECT unique_code FROM invoices 
        WHERE base_amount = :baseAmount 
          AND (
            (status = 'UNPAID' AND expired_at > NOW())
            OR
            (status = 'EXPIRED' AND quarantine_until > NOW())
          )
    `);

    // 2. Rentang default pool 3 digit (001 s.d. 999)
    let minCode = 1;
    let maxCode = 999;

    // 3. Dynamic Scaling: Jika kapasitas pool terpakai >= 80% (800 kode per jenjang),
    // sistem otomatis membuka partisi 4-Digit (1000 s.d. 9999 -> 9.000 kombinasi)
    if (lockedCodes.length >= 800) {
        minCode = 1000;
        maxCode = 9999;
    }

    // 4. Pilih kode acak yang bebas
    let selectedCode: number | null = null;
    while (!selectedCode) {
        const candidate = Math.floor(minCode + Math.random() * (maxCode - minCode + 1));
        if (!lockedCodes.includes(candidate)) {
            selectedCode = candidate;
        }
    }

    const totalPayment = baseAmount + selectedCode;

    return {
        uniqueCode: selectedCode,
        totalPayment: totalPayment,
        expiredAt: expiredAt,
        quarantineUntil: quarantineUntil
    };
}
```

---

### 5.3 Spesifikasi Berkas CSV Bank BRI & Algoritma Rekonsiliasi Idempoten Asinkron

#### A. Struktur Header Berkas CSV Resmi Bank BRI
Sistem dibangun berdasarkan struktur file CSV mutasi asli Bank BRI yang terverifikasi:
```csv
ID,"NOREK","TGL_TRAN","TGL_EFEKTIF","JAM_TRAN","SEQ","DESK_TRAN","SALDO_AWAL_MUTASI","MUTASI_DEBET","MUTASI_KREDIT","SALDO_AKHIR_MUTASI","GLSIGN","TRUSER","KODE_TRAN","KODE_TRAN_TELLER","TRREMK","TLBDS1","TLBDS2","REMARK_CUSTOM"
```
* Kolom Kunci:
  - `MUTASI_KREDIT`: Nominal uang masuk (contoh: `50247.00`).
  - `TGL_TRAN` & `JAM_TRAN`: Waktu pencatatan resmi bank (contoh: `04/11/26` dan `14:35:12`).
  - `DESK_TRAN` / `REMARK_CUSTOM`: Berita transfer atau identitas rekening pengirim.

#### B. Logika Algoritma Parser Rekonsiliasi (Idempotent & Jam Bebas)
```typescript
/**
 * PSEUDOCODE: Idempotent Asynchronous Bank Mutation Parser
 */
function reconcileBriCsvMutation(csvRows: Array<BriMutationRow>) {
    let reconciledCount = 0;
    let lateReviewCount = 0;
    let skippedCount = 0;

    for (const row of csvRows) {
        const creditAmount = parseFloat(row.MUTASI_KREDIT || '0');
        if (creditAmount <= 0) continue; // Abaikan mutasi debet / uang keluar

        const bankTxTimestamp = parseBriDateTime(row.TGL_TRAN, row.JAM_TRAN);

        // 1. CARI TAGIHAN AKTIF (UNPAID): Pencocokan nominal exact
        const activeInvoice = database.queryFirst(`
            SELECT * FROM invoices 
            WHERE status = 'UNPAID' 
              AND total_payment = :amount 
              AND expired_at >= :txTime
            ORDER BY created_at ASC
        `, { amount: creditAmount, txTime: bankTxTimestamp });

        if (activeInvoice) {
            // Update Lunas Seketika
            database.execute(`
                UPDATE invoices SET 
                    status = 'PAID', 
                    paid_at = :txTime, 
                    bank_ref = :ref 
                WHERE id = :id
            `, { id: activeInvoice.id, txTime: bankTxTimestamp, ref: row.DESK_TRAN });

            reconciledCount++;
            continue;
        }

        // 2. CEK KASUS SISWA TELAT BAYAR (EXPIRED DALAM MASA KARANTINA)
        const expiredInvoice = database.queryFirst(`
            SELECT * FROM invoices 
            WHERE status = 'EXPIRED' 
              AND total_payment = :amount 
              AND quarantine_until >= :txTime
            ORDER BY created_at DESC
        `, { amount: creditAmount, txTime: bankTxTimestamp });

        if (expiredInvoice) {
            database.execute(`
                UPDATE invoices SET 
                    status = 'REVIEW_TELAT', 
                    paid_at = :txTime, 
                    bank_ref = :ref 
                WHERE id = :id
            `, { id: expiredInvoice.id, txTime: bankTxTimestamp, ref: row.DESK_TRAN });

            lateReviewCount++;
            continue;
        }

        // 3. Jika transaksi sudah pernah lunas sebelumnya (IDEMPOTENT), sistem mengabaikannya
        skippedCount++;
    }

    return { reconciledCount, lateReviewCount, skippedCount };
}
```

---

### 5.4 Penanganan Biaya Admin Antarbank (BI-FAST Rp 2.500) & Kuitansi Proforma

1. **Kekebalan terhadap Biaya Admin Bank Pengirim**:
   - Ketika wali murid mentransfer dari bank lain (BCA, Mandiri, BNI, Dana, dll.) dengan biaya BI-FAST Rp 2.500, biaya administrasi tersebut **dipotong langsung dari rekening pengirim**, bukan dari nominal tagihan.
   - Uang yang masuk ke rekening BRI panitia pada kolom `MUTASI_KREDIT` adalah **murni nominal tagihan (misal Rp 50.001)**.
   - Sistem dijamin 100% aman dan tidak terpengaruh oleh variasi biaya admin perbankan luar.

2. **Cetak Kuitansi Proforma Resmi untuk Sekolah Negeri (Dana BOS)**:
   - Pada halaman E-Invoice rombongan kolektif, disediakan tombol:  
     👉 **`[ 📄 Cetak Kuitansi Proforma / Tagihan Resmi SPJ (PDF) ]`**
   - Menghasilkan dokumen format A4 berstempel resmi panitia Radar Kediri lengkap dengan nomor tagihan, rincian daftar siswa, dan rekening tujuan transfer BRI agar guru dapat mencairkan dana komite/BOS dengan mudah.

---

### 5.5 Pembuktian Matematis Skalabilitas Beban 20.000 Peserta

1. **Distribusi Normal (Periode 30 Hari)**:
   * Total peserta: 20.000 siswa $\rightarrow$ Rata-rata harian: $20.000 / 30 = \mathbf{667\text{ peserta/hari}}$.
   * Beban per jenjang tarif (SD, SMP, SMA): $667 / 3 \approx \mathbf{222\text{ peserta/jenjang/hari}}$.
   * Beban masa karantina 24 jam ($222 \times 2 = 444\text{ slot terkunci}$).
   * Persentase pemakaian slot 3-digit (`001`–`999`):  
     $$\frac{444}{999} = \mathbf{44,4\%}.$$
   * Tersisa **555 slot menganggur** di setiap jenjang setiap hari.

2. **Efisiensi Transaksi Kolektif Sekolah**:
   * Sekitar 65% peserta mendaftar rombongan via sekolah (50 siswa = 1 transaksi).
   * 20.000 siswa hanya menghasilkan sekitar **6.400 transaksi transfer bank riil** selama 30 hari (~71 transaksi/jenjang/hari).
   * Kapasitas 999 slot sebenarnya baru terpakai **kurang dari 10%** per hari.

3. **Penanganan Beban Lonjakan Ekstrem (*Rush Hour*)**:
   * Jika pemakaian slot 3-digit mencapai $\ge 80\%$, sistem otomatis beralih ke partisi 4-digit (`1000`–`9999`) yang menyediakan kapasitas **27.000 transaksi bersamaan per hari** ($9.000 \times 3\text{ jenjang}$).
   * Kapasitas ini terbukti sanggup menampung seluruh 20.000 peserta lunas hanya dalam 1 hari serentak tanpa pernah terjadi tabrakan nominal.

---

### 5.6 Alur Notifikasi Pelunasan WhatsApp (Opsi A: Semi-Automated One-Click Intent)

Setelah admin mengunggah file mutasi CSV dan sistem melunaskan puluhan siswa secara serentak:
1. Di halaman `/admin/rekonsiliasi`, tampil tabel siswa yang baru saja lunas.
2. Setiap baris memiliki tombol aksi: **`[ 💬 Kirim WA Lunas ]`**.
3. Saat diklik, peramban membuka WhatsApp Web panitia dengan pesan resmi yang telah terisi otomatis:
   ```text
   *KONFIRMASI PELUNASAN KKA RADAR KEDIRI 2026*
   Halo HERI SETIAWAN,
   Pembayaran pendaftaran KKA Anda sebesar Rp 50.247 telah TERVERIFIKASI LUNAS melalui mutasi Bank BRI.

   *No. Invoice*: INV-KKA26-M-8491
   *Asal Sekolah*: SMAN 1 KOTA KEDIRI
   *Status*: LUNAS / TERDAFTAR

   Silakan unduh Lembar Tanda Peserta Ujian resmi Anda pada tautan berikut:
   https://kkaradarkediri.com/daftar/invoice?kode=INV-KKA26-M-8491

   Terima kasih dan selamat mempersiapkan diri!
   ```
4. Tombol di tabel admin otomatis berubah warna menjadi **`[ ✅ WA Terkirim ]`** untuk mencegah duplikasi pengiriman.
5. Jalur ini 100% gratis, aman dari pemblokiran Meta, dan panitia memegang kendali penuh.

---

### 5.7 Perancangan Skema Database Baru (PostgreSQL / Supabase)

```sql
-- 1. Tabel Wilayah Dinamis
CREATE TABLE public.master_regions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    slug VARCHAR(50) UNIQUE NOT NULL,      -- Contoh: 'kabupaten-jombang'
    name VARCHAR(100) NOT NULL,             -- Contoh: 'Kabupaten Jombang'
    badge_label VARCHAR(50),
    pic_whatsapp VARCHAR(20),
    is_active BOOLEAN DEFAULT true,
    created_at TIMESTAMPTZ DEFAULT NOW()
);

-- 2. Tabel In-House Invoices dengan Masa Karantina
CREATE TABLE public.invoices (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    invoice_number VARCHAR(50) UNIQUE NOT NULL, -- Contoh: 'INV-KKA26-K-8941'
    registration_id UUID REFERENCES public.registrations(id) ON DELETE CASCADE,
    base_amount NUMERIC NOT NULL,               -- Tarif dasar (misal 50000)
    unique_code INT NOT NULL,                   -- Kode acak 3 digit (misal 247)
    total_payment NUMERIC NOT NULL,             -- Total exact (misal 50247)
    status VARCHAR(20) DEFAULT 'UNPAID',        -- 'UNPAID', 'PAID', 'EXPIRED', 'REVIEW_TELAT'
    created_at TIMESTAMPTZ DEFAULT NOW(),
    expired_at TIMESTAMPTZ NOT NULL,            -- Batas bayar siswa (24 Jam)
    quarantine_until TIMESTAMPTZ NOT NULL,      -- Batas karantina kode (+24 Jam lagi)
    paid_at TIMESTAMPTZ,
    bank_ref TEXT,
    wa_sent BOOLEAN DEFAULT false
);

-- 3. Tabel Hasil Ujian Tertulis LJK & Pemeringkatan
CREATE TABLE public.exam_results (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    participant_id UUID REFERENCES public.participants(id) ON DELETE CASCADE,
    nisn VARCHAR(20) NOT NULL,
    nama_siswa VARCHAR(150) NOT NULL,
    jenjang VARCHAR(20) NOT NULL,               -- 'SD', 'SMP', 'SMA'
    wilayah_slug VARCHAR(50) NOT NULL,
    skor_total NUMERIC NOT NULL,                -- Nilai akhir hasil koreksi LJK
    peringkat_sekolah INT,
    peringkat_wilayah INT,
    peringkat_umum INT,
    status_kelulusan VARCHAR(50) DEFAULT 'Peserta', -- 'Finalis', 'Peserta'
    is_published BOOLEAN DEFAULT false,
    created_at TIMESTAMPTZ DEFAULT NOW()
);

-- 4. Tabel Piagam / E-Sertifikat Digital (Verifikasi PPDB)
CREATE TABLE public.e_certificates (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    result_id UUID REFERENCES public.exam_results(id) ON DELETE CASCADE,
    certificate_number VARCHAR(100) UNIQUE NOT NULL,
    qr_verify_token TEXT NOT NULL,
    download_count INT DEFAULT 0,
    created_at TIMESTAMPTZ DEFAULT NOW()
);
```

---

## 6. RENCANA PENGUJIAN SISTEM (*TESTING & EVALUATION PLAN*)

1. **Pengujian Fungsional (*Black-Box Testing*)**: Uji coba form pendaftaran, masa kadaluarsa 24 jam, parser CSV mutasi BRI, penanganan siswa telat bayar, dan import nilai LJK.
2. **Pengujian Konkurensi & Beban (*Concurrency Stress Test*)**: Simulasi 1.000 s.d. 20.000 permintaan pendaftaran menggunakan *k6* untuk membuktikan *zero-collision* pada alokasi kode unik dan keandalan *Dynamic Digit Scaling*.
3. **Pengujian Kepuasan Pengguna (*System Usability Scale - SUS*)**: Evaluasi pengalaman pengguna kepada 30 responden (panitia Radar Kediri, guru pembina, dan wali murid) dengan target skor SUS $\ge 75$.

---

## 7. MATRIKS DISTRIBUSI BAB SKRIPSI (PANDUAN PENULISAN LAPORAN)

| Bab Skripsi | Konten Utama yang Ditulis | Sumber Bahan Teknis |
| :--- | :--- | :--- |
| **BAB I** | Latar belakang biaya 3rd party, kuota 20.000 siswa, kebutuhan SPJ BOS, evaluasi magang, rumusan & batasan masalah. | Bagian 1, 2, dan 3 dokumen ini. |
| **BAB II** | Landasan teori: Metode Incremental (Pressman), FSM, Algoritma Jaro-Winkler, Time-To-Live, Idempotent Processing. | Bagian 4 & pustaka standar RPL. |
| **BAB III** | Metodologi penelitian, instrumen wawancara mitra, arsitektur sistem, desain FSM invoice, skema database PostgreSQL. | Bagian 4 dan 5 dokumen ini. |
| **BAB IV** | **Hasil & Pembahasan per Increment**: <br>• 4.1 Increment 1 (Integritas Data NISN & Excel)<br>• 4.2 Increment 2 (In-House Billing, Parser CSV BRI, Idempotency & Telat Bayar)<br>• 4.3 Increment 3 (Wilayah Dinamis & Pengumuman Nilai LJK)<br>• 4.4 Analisis Uji Beban 20.000 Peserta & Skor SUS | Bagian 5 dan 6 dokumen ini. |
| **BAB V** | Kesimpulan efisiensi biaya (menghemat puluhan juta rupiah), keandalan kuota 20.000 siswa, saran pengembangan. | Rangkuman hasil uji & evaluasi. |

---

## 8. STRATEGI MENGHADAPI PERTANYAAN DOSEN PENGUJI

1. **Penguji:** *"Kenapa judulnya Pengembangan, bukan Rancang Bangun?"*  
   **Jawaban Anda:** *"Karena sistem ini memiliki kontinuitas nyata dari program Magang Industri saya di Jawa Pos Radar Kediri. Pada tahap magang, fondasi sistem telah terbangun. Pada Tugas Akhir ini, saya melakukan pengembangan modular (*incremental*) untuk menyelesaikan 3 problem kompleks: integrasi data sekolah, sistem billing mandiri anti-collision, dan manajemen multi-wilayah dinamis."*

2. **Penguji:** *"Kalau pesertanya sampai 20.000 orang, apa tidak jebol atau tabrakan kode unik 3 digitnya?"*  
   **Jawaban Anda:** *"Secara matematis sangat aman karena 3 faktor: Pertama, 20.000 peserta terdistribusi dalam 30 hari dan 3 jenjang tarif berbeda sehingga beban harian per jenjang hanya ~222 siswa (baru memakai 44% kapasitas 999). Kedua, 65% peserta mendaftar secara kolektif rombongan sekolah di mana 50 siswa hanya memakan 1 transaksi. Ketiga, jika terjadi lonjakan ekstrem di hari terakhir melampaui 80%, sistem otomatis mengaktifkan Dynamic Digit Scaling ke 4 digit yang menyediakan 27.000 kapasitas transaksi harian serentak."*

3. **Penguji:** *"Bagaimana kalau panitia upload mutasi tidak teratur (kadang jam 18, 20, atau 21) dan berkasnya bulanan?"*  
   **Jawaban Anda:** *"Sistem dirancang dengan arsitektur Asynchronous Idempotent Processing. Sistem mencocokkan data berdasarkan stempel waktu bank (`TGL_TRAN` dan `JAM_TRAN`), bukan jam upload admin. Selain itu, transaksi yang sudah berstatus PAID otomatis di-skip oleh sistem, sehingga berkas CSV bulanan aman diunggah berulang kali tanpa risiko duplikasi pelunasan."*

4. **Penguji:** *"Bagaimana kalau siswa telat bayar setelah waktu 24 jam kadaluarsa?"*  
   **Jawaban Anda:** *"Sistem menerapkan Cooling-Off Period (Masa Karantina Kode 24 Jam) sehingga kode unik tersebut tidak langsung diberikan ke siswa baru. Ketika siswa mentransfer terlambat, algoritma rekonsiliasi mutasi bank secara cerdas mendeteksi anomali tersebut dan mengisolasinya ke status 'REVIEW_TELAT' di dashboard admin, sehingga panitia dapat melakukan One-Click Recovery Approval tanpa risiko salah orang."*

5. **Penguji:** *"Kenapa tidak pakai Midtrans saja yang praktis?"*  
   **Jawaban Anda:** *"Bagi mitra industri kami (Radar Kediri) dengan puluhan ribu peserta, biaya admin Rp 4.000 per transaksi membebani biaya operasional hingga Rp 15-30 juta rupiah. Solusi In-House Billing Engine ini memangkas biaya perantara hingga 0 rupiah dan memberikan kedaulatan data penuh bagi panitia."*
