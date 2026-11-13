# Preferensi Gaya Belajar Mahasiswa UAJY — Data Preparation & EDA

Proyek UTS mata kuliah **Pembelajaran Mesin**, Universitas Atma Jaya Yogyakarta, Semester Gasal 2026/2027.

Proyek ini menyiapkan dan mengeksplorasi data survei preferensi gaya belajar mahasiswa berdasarkan empat framework: **VARK**, **Honey-Mumford**, **Felder-Silverman**, dan **Kolb**. Isinya mencakup data understanding, cleaning dan validasi, data preparation (termasuk scoring dan label sesuai aturan dosen), serta exploratory data analysis (EDA).

## Anggota Kelompok

| Nama | NPM |
|---|---|
| Andika Surya Wonowijaya | 240712825 |
| Samuel Viva Gracia | 240712829 |
| Rhiquelshen Charolim | 240712846 |

Pembagian tugas: lihat `KelpLOL_anggota.txt`.

## Struktur Folder

```
Project_uts/
├── dataset/
│   └── KelpLOL_survey_prepared.csv      # dataset hasil preparation (192 baris × 215 kolom)
├── dictionary/
│   └── KelpLOL_data_dictionary.xlsx     # data dictionary, aturan scoring, cleaning log
├── notebook/
│   └── KelpLOL_UTS_DataPreparation_EDA.ipynb
├── figures/                             # 8 grafik EDA (PNG), dibuat otomatis oleh notebook
├── recruitment/
│   ├── KelpLOL_rekrutmen.pdf            # dokumentasi rekrutmen responden
│   └── KelpLOL_rekrutmen.docx
├── report/                              # laporan UTS
├── KelpLOL_anggota.txt
└── README.md
```

## Data

- **Sumber:** survei resmi Microsoft Forms dari dosen. Dataset dibagikan dosen setelah survei ditutup (Kamis, 05 November 2026 pukul 12.00 WIB).
- **Data mentah (`Survei_Gaya_Belajar_IPS_IPK_NO_PII.xlsx`) tidak disertakan di repository ini** demi menjaga privasi responden. File ini dikecualikan lewat `.gitignore`.
- Dataset tidak memuat identifier langsung (nama, NPM, email, telepon). Kolom `ID` hanya nomor urut dan dipakai sebagai kunci teknis, bukan variabel analisis.

## Cara Menjalankan

1. Clone repository dan masuk ke foldernya.
2. Letakkan file data mentah dari dosen di `dataset/Survei_Gaya_Belajar_IPS_IPK_NO_PII.xlsx`.
3. Install dependency:
   ```bash
   pip install -r requirements.txt
   ```
4. Buka notebook dari folder `notebook/`, lalu jalankan semua cell dari atas (**Restart & Run All**):
   ```bash
   cd notebook
   jupyter notebook KelpLOL_UTS_DataPreparation_EDA.ipynb
   ```

Notebook membaca data mentah, lalu menghasilkan ulang `dataset/KelpLOL_survey_prepared.csv`, `dictionary/KelpLOL_data_dictionary.xlsx`, dan seluruh grafik di `figures/`. File data mentah hanya dibaca dan tidak pernah ditimpa.

Notebook diuji dengan Python 3 dan pandas 2.2.3.

## Alur Notebook

| Bagian | Isi |
|---|---|
| 1. Data Understanding | Ukuran data, tipe kolom, statistik awal, pengelompokan variabel, cek identifier, nilai unik, range, konsistensi format |
| 2. Data Cleaning & Validasi | Missing value, respons kosong, duplikasi, validasi range dan opsi jawaban, normalisasi teks, konversi tipe, penanganan nilai ekstrem (ditandai, tidak dihapus) |
| 3. Data Preparation | Penamaan kolom, pemisahan data sumber dan prepared, encoding VARK, skor dan label, variabel konteks, data dictionary, ekspor |
| 4. EDA | Distribusi konteks, skor, label dan class imbalance, missingness, gaya menjawab, korelasi/asosiasi, kesesuaian antar-framework |

Ringkasan cleaning: 194 respons sumber → **192 respons dianalisis** (2 respons kosong dihapus). Seluruh perubahan tercatat di sheet `cleaning_log` pada data dictionary.

## Aturan Skor dan Label

Skor dan label dihitung sesuai *Panduan Tugas Klasifikasi Preferensi Belajar* dari dosen:

| Framework | Skor | Label |
|---|---|---|
| VARK | Jumlah indikator per modalitas V, A, R, K (0–16) | Modalitas dengan skor maksimum unik; seri → `Seri` |
| Honey-Mumford | Jumlah 8 item per gaya (8–40) | Gaya dengan skor maksimum unik; seri → `Seri` |
| Felder-Silverman | Jumlah 4 item per kutub (4–20) | Active–Reflective: `Active` / `Reflective` / `Setara` |
| Kolb | Jumlah 6 item per mode (6–30); d1 = AC − CE, d2 = AE − RO | Tanda d1 dan d2 → `CE_RO`, `AC_RO`, `AC_AE`, `CE_AE`; nol → `Batas` |

`Seri`, `Setara`, dan `Batas` adalah kategori teknis untuk respons yang tidak dapat dipaksakan ke satu kategori, bukan gaya belajar tambahan.

## Temuan Utama EDA

- Responden cenderung menjawab "setuju" pada hampir semua item Likert, sehingga skor HM, FS, dan Kolb menumpuk di atas dan saling berkorelasi tinggi.
- Banyak skor seri, sehingga kategori teknis cukup besar: HM `Seri` 33%, FS `Setara` 32%, Kolb `Batas` 36,5%.
- Setelah gaya menjawab dikendalikan, kesesuaian antar-framework lemah. Hanya dimensi aktif/reflektif dan visual yang sedikit sejalan. VARK hampir independen dari framework lain.
- Tidak ada hubungan kuat antara skor atau label gaya belajar dan IPK.

## Privasi dan Etika

- Data hanya dipakai untuk keperluan mata kuliah.
- Data mentah tidak dipublikasikan. Dataset prepared hanya dibagikan sesuai izin dosen.
- Grafik dan tabel hanya menampilkan data agregat. Kelompok kecil digabung agar tidak mengarah ke individu tertentu.
