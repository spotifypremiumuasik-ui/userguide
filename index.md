# Panduan Operasional Sistem Tally Ikan

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Tally Proses** adalah modul untuk mencatat seluruh proses pengolahan ikan dari bahan baku sampai siap dikemas.

---

## Daftar Isi

- [I. Alur Kerja Sistem](#i-alur-kerja-sistem)
- [II. Login dan Buat Sesi](#ii-login-dan-buat-sesi)
- [III. Registrasi Bahan Baku](#iii-registrasi-bahan-baku)
- [IV. Input Timbangan](#iv-input-timbangan)
- [V. Input Proses](#v-input-proses)
- [VI. Packing](#vi-packing)
  - [VI.I. Sebelum Glazing](#vi-i-sebelum-glazing)
  - [VI.II. Sesudah Glazing](#vi-ii-sesudah-glazing)
  - [VI.III. MCO Lama](#vi-iii-mco-lama)
- [VII. Tips & Trick](#vii-tips-&-trick)
- [VIII. Troubleshooting](#viii-troubleshooting)
- [IX. Cheklist Harian](#ix-checklist-harian)

---

## I. Alur Kerja Sistem

<a id="i-alur-kerja-sistem"></a>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Berikut adalah diagram alur yang menggambarkan tahapan lengkap dari awal hingga akhir:

![Diagram Alur Kerja Sistem](images/TallyProses/diagramAlurKerjaProses.png)
_Gambar I.I: Diagram Alur Kerja Sistem._

---

## II. Login dan Buat Sesi

<a id="ii-login-dan-buat-sesi"></a>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Anda harus masuk ke akun anda dan membuat sesi, lakukan langkah-langkah berikut:

1.  **Akses URL:** Akses aplikasi melalui **Browser atau Google Chrome** dengan membuka [https://synergis.berkat-agung.com/tallyikan/product-index.php](https://synergis.berkat-agung.com/tallyikan/product-index.php).
2.  **Login:** Login ke aplikasi menggunakan **Akun Tally** anda, dengan memasukan **Username** dan **Password** yang sesuai.

    ![Halaman Login](images/TallyProses/halamanLogin.png)
    _Gambar II.I: Halaman Login._

3.  **Tally Proses:** Pilih modul **Tally Proses**.

    ![Tally Proses](images/TallyProses/tallyProses.png)
    _Gambar II.II: Tally Proses._

4.  **Buat Sesi Baru:** Klik tombol **Buat Sesi Baru**.

    ![Buat Sesi Baru](images/TallyProses/buatSesiBaru.png)
    _Gambar II.III: Buat Sesi Baru._

5.  **Masukan Jenis Ikan:** Isi jenis ikan yang akan diolah dan tanggal, lalu klik simpan

    ![Form Sesi Baru](images/TallyProses/formSesiBaru.png)
    _Gambar II.IV: Form Pembuatan Sesi Baru._

> ⚠️ **Penting** <br> Satu Sesi Hanya Untuk Satu Jenis Ikan!

---

## III. Registrasi Bahan Baku

<a id="iii-registrasi-bahan-baku"></a>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Setelah anda berhasil masuk dan membuat sesi, Tahapan selanjutnya adalah memasukan bahan baku, langkah-langkahnya sebagai berikut:

1.  **Pilih Sesi:** Setelah berhasil membuat sesi, anda akan diarahkan ke halaman daftar sesi, cari data sesi yang tadi anda isi, lalu klik menu **REG**

    ![Pilih Sesi](images/TallyProses/pilihSesi.png)
    \*Gambar III.I: Pilih Sesi

2.  **Masukan Data:** Isi data seluruh data yang dibutuhkan, meliputi:
    - **Sesi Pengiriman:** Pilih sesi jika BPB dibuat melalui website
    - **Supplier (Non BPB):** Diisi manual jika BPB tidak melalui website
    - **Kode Supplier:** Diisi manual jika BPB tidak melalui website

    <br>

    > ⚠️ **Penting** <br> Perhatikan ketentuan dibawah ini!

    <br>

    | BPB Dibuat Melalui Website                    | BPB Tidak Melalui Website     |
    | --------------------------------------------- | ----------------------------- |
    | Wajib Memilih Sesi Pengirman                  | Sesi Pengiriman Dikosongkan   |
    | Data supplier akan mengikuti data dari sistem | Wajib mengisi supplier manual |
    | Tidak perlu isi supplier manual               | Mengisi kode supplier manual  |

    <br>
    - **Jenis Ikan:** Nama ikan yang akan diproses
    - **Tanggal:** Tanggal bahan keluar dari CS
    - **Proses Pengerjaan:** Pilih proses pengerjaan yang sesuai

    <br>

    > ⚠️ **Penting** <br> Perhatikan ketentuan dibawah ini!

    <br>

    | Metode      | Deskripsi                   | Kapan Digunakan                                |
    | ----------- | --------------------------- | ---------------------------------------------- |
    | Pisah       | 1 BPB = 1 Hasil Proses      | Untuk BPB yang diproses terpisah               |
    | Campur Size | 1 BPB dengan ukuran berbeda | Untuk mencampur size dalam 1 BPB               |
    | Campur BPB  | Gabungan beberapa BPB       | Untuk menggabungkan beberapa BPB jadi 1 proses |

    <br>
    - **Kode Ikan:** Digunakan khusus untuk reproses jika ada
    - **Catatan:** Menentukan grouping hasil proses

    <br>

    > ⚠️ **Penting** <br> Perhatikan ketentuan dibawah ini!

    <br>

    > 📖 **Aturan:**

        ✅ Notes yang sama akan digabung jadi 1 grup
        ✅ Notes yang berbeda akan dipisah jadi grup yang berbeda
        ❌ Spasi juga akan berpengaruh: "ABC" tidak sama dengan "A BC"
        ❌ Huruf besar dan kecil juga akan berpengaruh: "abc" tidak sama dengan "ABC"

    > 💡**Tips** <br> Gunakan notes yang konsisten untuk menhindari kesalahan grouping

    <br>

    | Contoh                                                                      | Hasil                     |
    | --------------------------------------------------------------------------- | ------------------------- |
    | Batch 1: "SupplierA" <br> Batch 2: "Supplier A" <br> Batch 3: "Supplier a"  | Jadi 3 Group yang berbeda |
    | Batch 1: "Supplier A" <br> Batch 2: "Supplier A" <br> Batch 3: "Supplier A" | Jadi 1 Group yang sama    |

<br>

3. **Mulai Timbang:** Setelah semua selesai klik Mulai Timbang, untuk menyimpan data dan lanjut ke proses selanjutnya

   ![Mulai Timbang](images/TallyProses/mulaiTimbang.png)
   \*Gambar III.II: Mulai Timbang

---

## IV. Input Timbangan

<a id="iv-input-timbangan"></a>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Setelah selesai melakukan registrasi bahan baku, langkah selanjutnya adalah untuk melakukan input proses, dengan langkah-langkah sebagai berikut:

1. **Pilih Jenis Ikan:** Setiap jenis ikan akan ditampilkan dalam bentuk card di sistem, pilih ikan untuk mulai melakukan penimbangan

   ![Pilih Jenis Ikan](images/TallyProses/pilihJenisIkan.png)
   \*Gambar IV.I: Pilih Jenis Ikan

   <br>

   > ⚠️ **Penting** <br> Penimbangan akan dilakukan dua kali!

   <br>

2. **Timgangan Pertama:** Untuk mencatat berat ikan yang keluar dari CS atau hasil penerimaan, sistem menyimpannya sebagai berat awal

   ![Timbangan Pertama](images/TallyProses/timbanganPertama.png)
   \*Gambar IV.II: Timbangan Pertama

   <br>

   Setelah semua selesai, klik Timbang Ulang untuk melanjutkan proses ke timbangan kedua

   ![Timbang Ulang](images/TallyProses/timbanganPertama.png)
   \*Gambar IV.III: Timbang Ulang

3. **Timbangan Kedua:** Untuk validasi dan akan digunakan sebagai dasar perhitungan selanjutnya

   ![Timbangan Kedua](images/TallyProses/timbanganKedua.png)
   \*Gambar IV.IV: Timbangan Kedua

   Setelah semua selesai, klik Proses untuk lanjut ke tahapan berikutnya

   ![Proses](images/TallyProses/proses.png)
   \*Gambar IV.V: Proses

---

## V. Input Proses

<a id="v-input-proses"></a>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Setelah selesai melakukan timbangan, anda bisa melanjutkan ke tahapan Input Proses, langkah-langkahnya sebagai berikut:

1.  **Siapkan Data:** Siapkan data dengan mengisi beberapa inputan sebagai berikut:
    - **Asal Ikan:** Sesuaikan dengan grouping saat registrasi
    - **Jenis Proses:** Pilih jenis proses, seperti Fillet, Whole, dll
      <br>

    > ⚠️ **Penting** <br> Jika item belum ada, bisa dibuat manual dengan syarat!

        ℹ️ Pastikan item benar-benar belum ada dalam daftar
        ℹ️ Hindari duplikasi sistem
        ℹ️ Cek dulu sebelum membuat yang baru

    <br>
    - **Hasil Proses:** Masukan hasil proses, contoh 2/4, 4/6, dll  
      <br>

    > ⚠️ **Penting** <br> Jika item belum ada, bisa dibuat manual dengan syarat!

        ℹ️ Pastikan item benar-benar belum ada dalam daftar
        ℹ️ Hindari duplikasi sistem
        ℹ️ Cek dulu sebelum membuat yang baru

    ![Siapkan Data](images/TallyProses/siapkanData.png)
    \*Gambar V.I: Siapkan Data

2.  **Input Timbangan:** Hasil dari setiap kombinasi akan ditampilkan dalam bentuk card, input hasil timbang per kombinasi per proses.

    ![Input Timbangan](images/TallyProses/inputTimbangan.png)
    \*Gambar V.II: Input Timbangan

---

## VI. Packing

<a id="vi-packing"></a>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Setelah selesai melakukan input proses, anda bisa melanjutkan ke tahapan packing. Tahapan packing terdiri dari 3 jenis, sebagai berikut:

### VI.I Sebelum Glazing

<a id="vi-i-sebelum-glazing"></a>

Setelah melakukan input proses, anda akan diarahkan ke halaman packing, tahap ini dilakukan sebagai dasar sebelum proses glazing, caranya:

1. **Pilih Tahapan:** Pilih tahapan Sebelum Glazing

   ![Pilih Tahapan](images/TallyProses/pilihTahapanSebelumGlazing.png)
   \*Gambar VI.I.I: Pilih Tahapan

2. **Input Hasil Packing:** Input hasil packing barang jadi dengan memilih inputan yang sesuai

   ![Input Hasil Packing](images/TallyProses/inputHasilPacking.png)
   \*Gambar VI.I.II: input Hasil Packing

3. **Masukan Berat Timbangan:** Masukan hasil berat timbangan yang sesuai

   ![Hasil Inputan](images/TallyProses/hasilInputanPacking.png)
   \*Gambar VI.I.II: Hasil Inputan

### VI.II Sesudah Glazing

<a id="vi-ii-sesudah-glazing"></a>

> ⚠️ **Penting** <br> Harus siapkan Master terlebih dahulu!

Tahapan selanjutnya adalah memasukan data Sesudah Glazing, sebagai berikut:

1. **Pilih Tahapan:** Pilih tahapan sesudah glazing

   ![Pilih Tahapan Sesudah Glazing](images/TallyProses/pilihTahapanSesudahGlaz.png)
   \*Gambar V.II.I: Pilih Tahapan Sesudah Glazing

2. **Buat Master:** Buat master dengan, klik tombol tambah kombinasi dan mengisi data sebagai berikut:

   ![Tambah Kombinasi](images/TallyProses/tambahKombinasi.png)
   \*Gambar VI.II.II: Tambah Kombinasi
   - **Hasil Proses:** Dari mana hasil proses berasal
   - **Jenis Ikan:** Jenis ikan yang di proses
   - **Berat per PV/PB:** Berat standar 1 Pack Vacuum (Kg)
   - **PV/PB per MC:** Berapa PV dalam 1 Master Cartoon

   <br>

   > 📖 **Cara Kerja Sistem:**

   | Kondisi              | Hasil                   |
   | -------------------- | ----------------------- |
   | Berat = Berat 1 PV   | Dihitung sebagai 1 PV   |
   | Total PV = PV per MC | Otomatis jadi 1 MC      |
   | Berat < Berat 1 PV   | Otomatis jadi MCO Akhir |

   > 📚 **Contoh Perhitungan:** <br> **Master:** <br> - Berat 1 PV = 10 Kg <br> - Jumlah PV per MC = 5 PV <br><br> **Input:** <br> - Timbangan 1 = 10 Kg -> Jadi 1 PV <br> - Timbangan 2 = 10 Kg -> Jadi 1 PV **(Total 2 PV)** <br> - Timbangan 3 = 10 Kg -> Jadi 1 PV **(Total 3 PV)** <br> - Timbangan 4 = 10 Kg -> Jadi 1 PV **(Total 4 PV)**<br> - Timbangan 5 = 10 Kg -> Jadi 1 PV **(Total 5 PV)** -> **Otomatis jadi 1 MC** <br> - Timbangan 6 = 7 Kg -> Berat < 10Kg -> **Otomatis jadi MCO Akhir**

   ![Masukan Untuk Membuat Master](images/TallyProses/masukanTambahKombinas.png)
   \*Gambar VI.II.III: Masukan Untuk Membuat Master

   Setelah memasukan semua data, klik Simpan Kombinasi untuk menyimpan master

3. Buat semua master yang dibutuhkan, kalau sudah master akan tampil seperti ini:

   ![Master Yang Sudah Dibuat](images/TallyProses/hasilKombinasi.png)
   \*Gambar VI.II.IV: Master yang sudah dibuat

4. Masukan data yang dibutuhkan, lalu input data sesudah glazing, dengan cara mengisi form berikut:
   - **Tanggal Packing:** Tanggal proses packing dilakukan
   - **Jenis Packing:** Pilih jenis packing, MC atau MCO
   - **Pilih Kombinasi Ikan & Proses:** Pilih master

   Selanjutnya, masukan kiloan sesuai berat 1 PV

   ![Masukan Hasil Glazing](images/TallyProses/inputKiloanGlazing.png)
   \*Gambar VI.II.V: Masukan Hasil Glazing

   Hasil inputan akan tampil berupa tabel seperti gambar dibawah:

   ![Hasil Kiloan](images/TallyProses/hasilKiloanSesudahGlazing.png)
   \*Gambar VI.II.VI: Hasil Kiloan

### VI.III MCO Lama

<a id="vi-iii-mco-lama"></a>

Jika terdapat MCO Lama, masukan langsung ke kolom MCO Lama, tidak perlu melalui perhitungan PV/MC, langkah-langkahnya sebagai berikut:

1. **Pilih Tahapan:** Pilih tahapan MCO Lama

   ![Pilih Tahapan MCO Lama](images/TallyProses/pilihMCOLama.png)
   \*Gambar V.III.I: Pilih Tahapan MCO Lama

2. **Tambah Data:** Masukan data yang dibutuhkan, lalu klik tambah data untuk memasukan hasil

   ![Tambah Data MCO Lama](images/TallyProses/tambahDataMCOLama.png)
   \*Gambar V.III.II: Tambah Data MCO Lama
