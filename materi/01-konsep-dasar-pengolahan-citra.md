# Pengolahan Citra Digital - Konsep Dasar

## Berdasarkan Buku Gonzales & Woods Digital Image Processing 4th Edition

## 1. Pendahuluan Pengolahan Citra Digital

### Definisi Pengolahan Citra Digital
Pengolahan citra digital adalah disiplin ilmu yang mempelajari implementasi algoritma komputer untuk melakukan pemrosesan pada citra digital. Tujuannya adalah untuk meningkatkan kualitas citra, mengekstrak informasi, atau melakukan analisis visual.

### Sejarah Perkembangan
- **1960-an**: Aplikasi awal dalam industri ruang angkasa (NASA)
- **1970-an**: Pengembangan algoritma dasar pengolahan citra
- **1980-an**: Penerapan dalam medis dan industri
- **1990-an hingga sekarang**: Integrasi dengan kecerdasan buatan dan machine learning

## 2. Elemen Dasar Penglihatan Manusia dan Citra Digital

### Struktur Mata Manusia
1. **Kornea**: Bagian transparan di depan mata yang memfokuskan cahaya
2. **Iris**: Mengontrol jumlah cahaya yang masuk
3. **Lens**: Memfokuskan cahaya ke retina
4. **Retina**: Berisi fotoreceptor (rods dan cones)
5. **Optic Nerve**: Mentransmisikan sinyal visual ke otak

### Rods vs Cones
- **Rods**: ~120 juta, sensitif terhadap cahaya redup, tidak mendeteksi warna
- **Cones**: ~6-7 juta, berfungsi dalam cahaya terang, mendeteksi warna
- **Fovea**: Area dengan konsentrasi cones tertinggi

### Persepsi Warna
- Teori Tristimulus: Kombinasi merah, hijau, biru (RGB)
- Ruang warna: RGB, CMY(K), HSI, YCbCr

## 3. Sampling dan Kuantisasi

### Representasi Citra Digital
Citra digital direpresentasikan sebagai matriks dua dimensi:
- **f(x,y)**: Nilai intensitas piksel pada koordinat (x,y)
- **M × N**: Ukuran citra (M baris, N kolom)
- **L**: Jumlah level intensitas (2^k untuk k-bit citra)

### Sampling
Proses konversi sinyal kontinyu menjadi diskrit:
- **Spatial sampling**: Pemilihan titik-titik dalam ruang
- **Temporal sampling**: Pemilihan titik-titik dalam waktu
- **Aliasing**: Distorsi akibat sampling tidak cukup
- **Nyquist criterion**: Frekuensi sampling ≥ 2× frekuensi sinyal tertinggi

### Kuantisasi
Proses memetakan nilai kontinyu ke nilai diskrit:
- **Uniform quantization**: Interval sama untuk semua level
- **Non-uniform quantization**: Interval berbeda untuk presisi lebih baik
- **Error kuantisasi**: Selisih antara nilai asli dan terkuantisasi

## 4. Hubungan Antara Piksel

### Koordinat Piksel
- **(x,y)**: Koordinat spasial
- **f(x,y)**: Nilai intensitas
- **Origin**: Biasanya di kiri atas

### Tetangga Piksel (Neighbors)
1. **4-neighbors (N4)**: Atas, bawah, kiri, kanan
   ```
   (x-1,y)  (x,y-1)  (x,y)  (x,y+1)  (x+1,y)
   ```

2. **8-neighbors (N8)**: 4-neighbors + diagonal
   ```
   (x-1,y-1) (x-1,y) (x-1,y+1)
   (x,y-1)   (x,y)   (x,y+1)
   (x+1,y-1) (x+1,y) (x+1,y+1)
   ```

3. **Diagonal neighbors (ND)**: Keempat diagonal

### Konektivitas
- **4-connectivity**: Terhubung melalui N4
- **8-connectivity**: Terhubung melalui N8
- **m-connectivity**: Kombinasi untuk menghindari ambiguitas

## 5. Operasi Aritmatika dan Logika pada Citra

### Operasi Aritmatika
1. **Penjumlahan Citra**
   - **Formula**: g(x,y) = f1(x,y) + f2(x,y)
   - **Aplikasi**: Noise reduction, image averaging
   - **Clipping**: g(x,y) = min{f1(x,y) + f2(x,y), 255}

2. **Pengurangan Citra**
   - **Formula**: g(x,y) = f1(x,y) - f2(x,y)
   - **Aplikasi**: Background subtraction, change detection
   - **Absolute difference**: g(x,y) = |f1(x,y) - f2(x,y)|

3. **Perkalian Citra**
   - **Formula**: g(x,y) = f1(x,y) × f2(x,y)
   - **Aplikasi**: Masking, shading correction

4. **Pembagian Citra**
   - **Formula**: g(x,y) = f1(x,y) / f2(x,y)
   - **Aplikasi**: Normalization, ratio imaging

### Operasi Logika
1. **AND**: g(x,y) = f1(x,y) ∧ f2(x,y)
2. **OR**: g(x,y) = f1(x,y) ∨ f2(x,y)
3. **NOT**: g(x,y) = ¬f(x,y)
4. **XOR**: g(x,y) = f1(x,y) ⊕ f2(x,y)

## 6. Transformasi Geometri Dasar

### Transformasi Koordinat
- **(x,y)**: Koordinat asli
- **(x',y')**: Koordinat hasil transformasi

### Jenis Transformasi
1. **Translasi (Pergeseran)**
   ```
   x' = x + tx
   y' = y + ty
   ```
   di mana (tx,ty) adalah vektor translasi

2. **Rotasi**
   ```
   x' = x cos(θ) - y sin(θ)
   y' = x sin(θ) + y cos(θ)
   ```
   di mana θ adalah sudut rotasi

3. **Scaling (Penskalaan)**
   ```
   x' = sx × x
   y' = sy × y
   ```
   di mana (sx,sy) adalah faktor skala

4. **Shearing**
   ```
   x' = x + shx × y
   y' = shy × x + y
   ```

### Interpolasi Piksel
1. **Nearest Neighbor**: Cepat, namun hasilnya kasar
2. **Bilinear**: Kualitas lebih baik dengan komputasi sedang
3. **Bicubic**: Kualitas terbaik dengan komputasi tinggi

## 7. Representasi Warna

### Model Warna RGB
- **R (Red)**: Komponen merah [0,255]
- **G (Green)**: Komponen hijau [0,255]
- **B (Blue)**: Komponen biru [0,255]
- **24-bit color**: 16.7 juta warna

### Model Warna CMY/CMYK
- **C (Cyan)**: 100 - R
- **M (Magenta)**: 100 - G
- **Y (Yellow)**: 100 - B
- **K (Black/Key)**: Komponen tambahan untuk depth

### Model HSI (Hue, Saturation, Intensity)
- **Hue**: Warna murni (0-360°)
- **Saturation**: Kekuatan warna (0-1)
- **Intensity**: Kecerahan (0-1)

### Konversi RGB ke HSI
```
θ = cos⁻¹[(0.5[(R-G)+(R-B)]) / √((R-G)²+(R-B)(G-B))]

H = θ jika B ≤ G
H = 360° - θ jika B > G

S = 1 - [3/(R+G+B)] × min(R,G,B)

I = (R+G+B)/3
```

## 8. Histogram Citra

### Definisi Histogram
Histogram adalah fungsi yang menyatakan frekuensi kemunculan setiap level intensitas dalam citra:
```
h(k) = jumlah piksel dengan intensitas k
```

### Normalisasi Histogram
```
p(k) = h(k) / (M×N)
```
di mana M×N adalah total jumlah piksel

### Properties Histogram
1. **Informasi intensitas**: Distribusi nilai piksel
2. **Shape**: Menunjukkan karakteristik citra
3. **Contrast**: Sebaran nilai intensitas
4. **Brightness**: Rata-rata intensitas

### Aplikasi Histogram
1. **Thresholding**: Menentukan nilai batas
2. **Contrast enhancement**: Histogram equalization
3. **Image analysis**: Segmentasi otomatis

## 9. Transformasi Point Processing

### Linear Transformation
```
g(x,y) = a × f(x,y) + b
```
- **a**: Contrast control
- **b**: Brightness control

### Log Transformation
```
s = c × log(1 + r)
```
- Meningkatkan kontras pada daerah gelap
- Mengurangi kontras pada daerah terang

### Power-Law (Gamma) Transformation
```
s = c × r^γ
```
- **γ < 1**: Menambah brightness daerah gelap
- **γ > 1**: Menambah brightness daerah terang

### Piecewise Linear Transformation
1. **Contrast stretching**: Meningkatkan rentang dinamis
2. **Thresholding**: Binerisasi citra
3. **Gray-level slicing**: Meningkatkan rentang tertentu

## 10. Spatial Filtering

### Konsep Filtering
Filtering adalah proses memodifikasi citra dengan operasi neighborhood:
- **Mask/kernel**: Matriks koefisien filter
- **Convolution**: Operasi matematis filtering
- **Correlation**: Operasi similar tanpa rotasi

### Jenis Filter Linear
1. **Smoothing Filters**
   - **Mean filter**: Rata-rata nilai tetangga
   - **Gaussian filter**: Pembobotan Gaussian
   ```
   w(x,y) = (1/2πσ²) × e^(-(x²+y²)/2σ²)
   ```

2. **Sharpening Filters**
   - **Laplacian filter**: Turunan orde dua
   ```
   ∇²f = ∂²f/∂x² + ∂²f/∂y²
   ```
   - **Unsharp masking**: Pengasaran dengan substraksi

### Filter Non-linear
1. **Median filter**: Mengurangi noise sambil mempertahankan tepi
2. **Max/min filter**: Operasi morfologi dasar
3. **Bilateral filter**: Preservasi tepi dengan smoothing

## 11. Transformasi Frekuensi

### Fourier Transform
Konversi dari domain spasial ke domain frekuensi:
```
F(u,v) = ∑∑ f(x,y) × e^(-j2π(ux/M + vy/N))
```

### Properties Fourier Transform
1. **Linearity**: Transformasi linear
2. **Shift**: Pergeseran di domain spasial = perubahan fase
3. **Scaling**: Penskalaan spasial = penskalaan frekuensi terbalik
4. **Rotation**: Rotasi di domain spasial = rotasi di domain frekuensi

### Filter di Domain Frekuensi
1. **Low-pass filter**: Melewati frekuensi rendah (smoothing)
2. **High-pass filter**: Melewati frekuensi tinggi (sharpening)
3. **Band-pass filter**: Frekuensi rentang tertentu
4. **Notch filter**: Menghilangkan frekuensi spesifik

## 12. Kompresi Citra

### Jenis Kompresi
1. **Lossless Compression**
   - **Run-length encoding (RLE)**
   - **Huffman coding**
   - **Lempel-Ziv-Welch (LZW)**
   - **Lossless JPEG**

2. **Lossy Compression**
   - **JPEG**: Discrete Cosine Transform (DCT)
   - **JPEG 2000**: Wavelet transform
   - **Fractal compression**

### Metrik Kualitas
1. **MSE (Mean Squared Error)**
   ```
   MSE = (1/MN) × ∑∑ [f(x,y) - g(x,y)]²
   ```

2. **PSNR (Peak Signal-to-Noise Ratio)**
   ```
   PSNR = 10 × log10(MAX²/MSE)
   ```

3. **SSIM (Structural Similarity)**
   - Membandingkan struktur visual
   - Lebih sesuai dengan persepsi manusia

## 13. Segmentasi Citra

### Thresholding
Pemisahan objek dan background berdasarkan intensitas:
1. **Global thresholding**: Satu nilai untuk seluruh citra
2. **Local thresholding**: Nilai berbeda per region
3. **Adaptive thresholding**: Nilai dinamis

### Edge Detection
Deteksi batas objek dengan operasi turunan:
1. **Gradient operators**
   - **Sobel**: Kombinasi smoothing dan differentiation
   - **Prewitt**: Similar dengan Sobel
   - **Roberts**: Simple dan cepat

2. **Second-order derivative**
   - **Laplacian of Gaussian (LoG)**: Gaussian + Laplacian
   - **Canny edge detector**: Optimal edge detection

### Region-based Segmentation
1. **Region growing**: Penggabungan piksel serupa
2. **Region splitting and merging**: Partisi dan gabung
3. **Watershed**: Berdasarkan topografi citra

## 14. Representasi dan Deskripsi

### Chain Code
Representasi boundary dengan arah:
- **4-directional**: {0,1,2,3}
- **8-directional**: {0,1,2,3,4,5,6,7}

### Moment Invariants
Fitur shape yang invarian terhadap transformasi:
- **Translation invariants**
- **Rotation invariants**
- **Scale invariants**

### Texture Analysis
1. **Statistical methods**
   - **Co-occurrence matrix**
   - **Run-length statistics**

2. **Spectral methods**
   - **Fourier transform**
   - **Wavelet transform**

## 15. Pattern Recognition

### Feature Extraction
Seleksi fitur yang relevan untuk klasifikasi:
- **Color features**: Histogram, color moments
- **Shape features**: Moment, boundary descriptor
- **Texture features**: GLCM, wavelet coefficients

### Classification Methods
1. **Supervised learning**
   - **Bayesian classifier**
   - **Neural networks**
   - **Support vector machines (SVM)**

2. **Unsupervised learning**
   - **K-means clustering**
   - **Hierarchical clustering**

### Performance Evaluation
- **Confusion matrix**: True positive, false positive, dll
- **Precision**: TP/(TP+FP)
- **Recall**: TP/(TP+FN)
- **F1-score**: Harmonik mean precision dan recall

## Kesimpulan

Pengolahan citra digital adalah bidang interdisipliner yang menggabungkan matematika, sains komputer, dan fisika. Pemahaman konsep dasar seperti representasi citra, transformasi, filtering, dan segmentasi sangat penting untuk mengembangkan aplikasi pengolahan citra yang efektif. Buku Gonzales & Woods memberikan fondasi matematis yang kuat untuk semua konsep ini dengan penjelasan yang komprehensif dan implementasi praktis.

*Catatan: Materi ini disusun berdasarkan bab-bab awal buku Gonzales & Woods Digital Image Processing 4th Edition sebagai pengenalan konsep dasar pengolahan citra digital.*