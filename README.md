# Analisis Kondisi Emosi Seseorang Berdasarkan Suara Bicara Menggunakan Transformasi Fourier

Projek akhir mata kuliah **Pengolahan Sinyal Digital**. Sistem ini mengolah sinyal suara (`.wav`) melalui normalisasi, framing, windowing, dan transformasi Fourier (STFT/FFT), lalu mengekstrak fitur akustik (MFCC, pitch, energi RMS) untuk mengklasifikasikan **8 kondisi emosi** dengan model **Multi-Layer Perceptron (MLP)**.

**Kelompok 3 – Kelas 2024E**
Program Studi Sains Data, Fakultas Matematika dan Ilmu Pengetahuan Alam, Universitas Negeri Surabaya (2025)

| Nama | NIM |
|------|-----|
| Bima Setia Sugiharto | 24031554040 |
| Moh. Rasya Al Khalifi | 24031554132 |

---

## Latar Belakang

Suara manusia tidak hanya membawa isi pesan, tetapi juga emosi pembicaranya. Emosi tercermin pada perubahan karakteristik akustik seperti frekuensi dasar (pitch), amplitudo, energi spektrum, dan pola perubahan nada. Analisis emosi dari suara sudah banyak dimanfaatkan pada *virtual assistant* dan asisten keamanan.

Projek ini menerapkan transformasi Fourier untuk mengubah sinyal suara dari domain waktu ke domain frekuensi, mengekstrak fitur yang berkaitan dengan emosi, memvisualisasikannya, dan membangun model prediksi emosi.

## Tujuan

1. Menerapkan STFT pada sinyal suara untuk mendapatkan spektrogram.
2. Mengekstrak fitur kunci terkait emosi dari spektrum (pitch, energi, MFCC).
3. Memvisualisasikan karakteristik frekuensi tiap emosi.
4. Merancang model yang dapat memprediksi emosi dari fitur yang telah diproses.

## Dataset

- **Format:** file audio `.wav` di folder `Audio/`
- **Jumlah:** 192 file, terdiri dari 8 kelas emosi dengan masing-masing 24 file
- **Sumber:** Ryerson Audio-Visual Database of Emotional Speech and Song (RAVDESS)
- **Label:** diambil dari kode emosi pada nama file (bagian ke-3), misalnya `03-01-05-02-01-01-01.wav` → kode `05`

| Kode | Emosi |
|:----:|-------|
| 01 | Normal |
| 02 | Santai |
| 03 | Senang |
| 04 | Sedih |
| 05 | Marah |
| 06 | Takut |
| 07 | Jijik |
| 08 | Kaget |

## Alur Pengerjaan

1. **Input audio** – membaca file `.wav` dan menampilkan waveform.
2. **Pra-pemrosesan sinyal**
   - *Normalisasi amplitudo* agar volume konsisten antar file (rentang −1 sampai 1).
   - *Framing* dengan `frame_size = 2048` dan `hop_length = 512`.
   - *Windowing* dengan Hamming window untuk mengurangi kebocoran spektral.
3. **Transformasi Fourier** – STFT untuk spektrogram (Hz dan dB) dan FFT untuk spektrum frekuensi.
4. **Ekstraksi fitur** (42 fitur per file)
   - 40 koefisien **MFCC** (rata-rata per koefisien)
   - **Energi RMS** (rata-rata)
   - **Pitch / F0** dengan algoritma `pyin` (rata-rata)
5. **Visualisasi** – waveform, spektrogram, spektrum frekuensi, serta energi dan pitch per waktu.
6. **Pemodelan** – klasifikasi emosi dengan MLP.

## Model

| Parameter | Nilai |
|-----------|-------|
| Algoritma | `MLPClassifier` (scikit-learn) |
| Hidden layer | (128, 64) |
| Aktivasi | ReLU |
| Solver | Adam |
| Max iterasi | 500 |
| Split data | 80% latih : 20% uji (stratified, `random_state=42`) |
| Praproses fitur | `StandardScaler` |

## Hasil

**Akurasi MLP: 79,49%** (data uji: 39 sampel)

| Emosi | Precision | Recall | F1-score |
|-------|:---------:|:------:|:--------:|
| Jijik | 0,67 | 0,80 | 0,73 |
| Kaget | 1,00 | 0,50 | 0,67 |
| Marah | 0,83 | 1,00 | 0,91 |
| Normal | 0,83 | 1,00 | 0,91 |
| Santai | 0,60 | 0,60 | 0,60 |
| Sedih | 0,80 | 0,80 | 0,80 |
| Senang | 1,00 | 0,80 | 0,89 |
| Takut | 0,80 | 0,80 | 0,80 |
| **Macro avg** | **0,82** | **0,79** | **0,79** |

Kombinasi fitur spektral (MFCC), intonasi (pitch), dan intensitas (RMS) cukup efektif untuk membedakan emosi dalam suara manusia.

## Cara Menjalankan

1. Clone repository:
```bash
   git clone https://github.com/<username>/speech-emotion-recognition-fft-mlp.git
   cd speech-emotion-recognition-fft-mlp
```
2. Install dependensi:
```bash
   pip install -r requirements.txt
```
3. Pastikan folder `Audio/` berisi file `.wav` dataset, dan file contoh (`03-01-01-01-01-01-01.wav`, dst.) berada satu folder dengan notebook.
4. Jalankan notebook:
```bash
   jupyter notebook
```
   lalu buka `speech-emotion-recognition-fft-mlp_source code.ipynb` dan jalankan seluruh sel secara berurutan.

## Struktur Repository

```
├── speech-emotion-recognition-fft-mlp_source code.ipynb   # source code utama
├── Audio/                          # dataset audio (.wav)
├── requirements.txt
└── README.md
```
