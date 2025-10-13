# Transformasi Intensitas dan Filtering Spasial

## Berdasarkan Bab 3: Intensity Transformations and Spatial Filtering

## 1. Pendahuluan

### Transformasi Intensitas
Transformasi intensitas adalah operasi point processing di mana setiap piksel dalam citra input dipetakan ke piksel yang sesuai dalam citra output. Nilai output hanya bergantung pada nilai input piksel tersebut, tidak pada tetangganya.

### Filtering Spasial
Filtering spasial adalah operasi neighborhood processing di mana nilai output piksel bergantung pada nilai piksel input dan tetangganya dalam suatu region.

## 2. Transformasi Intensitas Dasar

### Fungsi Transformasi
Transformasi intensitas dinyatakan sebagai:
```
s = T(r)
```
di mana:
- **r**: Intensitas input piksel
- **s**: Intensitas output piksel
- **T**: Fungsi transformasi

### Linear Transformation
Transformasi linear memiliki bentuk:
```
s = a × r + b
```
- **a**: Kontrol kontras (slope)
- **b**: Kontrol kecerahan (intercept)

![Linear Transformation Functions](../images/bab3_linear-transformations.png)

#### Variasi Linear Transformation:
1. **Identity Transformation**
   ```
   T(r) = r
   ```
   - Tidak ada perubahan pada citra

2. **Negative Transformation**
   ```
   T(r) = L - 1 - r
   ```
   - Memperoleh negatif citra
   - Meningkatkan detail pada area gelap

3. **Contrast Stretching**
   ```
   s = (r - r_min) × (L-1)/(r_max - r_min)
   ```
   - Meningkatkan rentang dinamis citra

![Negative Transformation Example](../images/bab3_negative-transformation-example.png)

### Logarithmic Transformation
```
s = c × log(1 + r)
```
#### Karakteristik:
- Meningkatkan kontras pada area gelap
- Mengurangi kontras pada area terang
- Berguna untuk citra dengan dynamic range tinggi

#### Aplikasi:
- Citra dengan spektrum frekuensi (Fourier)
- Citra medis dengan rentang intensitas lebar
- Citra astronomi

### Power-Law (Gamma) Transformation
```
s = c × r^γ
```
di mana c dan γ adalah konstanta positif.

![Gamma Correction Curves](../images/bab3_gamma-correction-curves.png)

#### Berbagai Nilai Gamma:
- **γ < 1**: Memperluas nilai intensitas rendah (brightening)
- **γ = 1**: Linear transformation
- **γ > 1**: Memperluas nilai intensitas tinggi (darkening)

![Gamma Correction Examples](../images/bab3_gamma-correction-examples.png)

#### Aplikasi:
- Koreksi gamma pada display devices
- Contrast enhancement
- Image preprocessing untuk computer vision

### Piecewise-Linear Transformation
Transformasi yang terdiri dari beberapa segmen linear dengan slope berbeda.

![Piecewise-Linear Transformations](../images/bab3_piecewise-linear-transformations.png)

#### Jenis Piecewise-Linear:
1. **Contrast Stretching**
   - Meningkatkan kontras keseluruhan
   - Linear antara (r1, s1) dan (r2, s2)

2. **Thresholding**
   ```
   s = 0 jika r < T
   s = L-1 jika r ≥ T
   ```
   - Binerisasi citra
   - Segmentasi sederhana

3. **Gray-Level Slicing**
   - Menonjolkan rentang intensitas tertentu
   - Dua mode: dengan atau tanpa background

![Thresholding Example](../images/bab3_thresholding-example.png)

## 3. Histogram Processing

### Histogram Definition
Histogram citra adalah fungsi h(rk) yang memberikan jumlah piksel dengan intensitas rk:
```
h(rk) = jumlah piksel dengan intensitas rk
```

### Normalized Histogram
```
p(rk) = h(rk) / MN
```
di mana MN adalah total piksel dalam citra.

### Histogram Equalization
Tujuan: Mendistribusikan ulang intensitas untuk mendapatkan histogram flat.

![Histogram Equalization Process](../images/bab3_histogram-equalization-process.png)

#### Algoritma:
1. Hitung histogram p(rk)
2. Hitung CDF (Cumulative Distribution Function):
   ```
   sk = Σi=0^k p(ri)
   ```
3. Map nilai intensitas:
   ```
   s = sk × (L-1)
   ```

#### Properties:
- Meningkatkan kontras global
- Automatis contrast enhancement
- Hasil tidak dapat diprediksi

![Histogram Equalization Example](../images/bab3_histogram-equalization-example.png)

### Histogram Matching (Specification)
Menyesuaikan histogram citra agar sesuai dengan histogram target yang diinginkan.

#### Algoritma:
1. Hitung histogram input dan target
2. Lakukan equalization pada kedua citra
3. Map intensities berdasarkan CDF

#### Aplikasi:
- Standardisasi citra dari berbagai sumber
- Matching untuk citra medis
- Color correction

### Local Histogram Processing
Processing dilakukan pada local neighborhoods (tiles):
- Ukuran tile tipikal: 64×64 atau 128×128
- Overlapping tiles untuk smooth transition
- Adaptive contrast enhancement

## 4. Spatial Filtering

### Fundamental Concepts
Filtering spasial menggunakan neighborhood operations:
- **Filter mask/kernel**: Matriks koefisien
- **Convolution**: Operasi matematis utama
- **Anchor point**: Center of mask

### Convolution Operation
```
g(x,y) = ΣΣ w(s,t) × f(x+s, y+t)
```
di mana:
- **g(x,y)**: Output pixel
- **w(s,t)**: Filter coefficients
- **f(x+s, y+t)**: Input neighborhood

![Convolution Operation](../images/bab3_convolution-operation.png)

### Smoothing Linear Filters
Tujuan: Mengurangi noise dan detail halus.

#### Box Filter (Mean Filter)
Mask 3×3:
```
1/9 [1 1 1]
    [1 1 1]
    [1 1 1]
```
#### Karakteristik:
- Sederhana dan cepat
- Menghasilkan blurring
- Tidak optimal untuk noise reduction

![Mean Filter Example](../images/bab3-mean-filter-example.png)

#### Weighted Average Filter
Mask 3×3:
```
1/16 [1 2 1]
     [2 4 2]
     [1 2 1]
```
#### Karakteristik:
- Pembobotan Gaussian
- Hasil lebih smooth
- Preservasi edges lebih baik

#### Gaussian Filter
Fungsi Gaussian 2D:
```
G(x,y) = (1/2πσ²) × e^(-(x²+y²)/2σ²)
```
#### Properties:
- Rotational symmetry
- Separable filter
- Optimal untuk noise reduction

### Sharpening Linear Filters
Tujuan: Meningkatkan detail dan edges.

#### Laplacian Filter
Turunan orde dua:
```
∇²f = ∂²f/∂x² + ∂²f/∂y²
```
#### Masks:
1. **4-neighbor Laplacian**:
   ```
   [0  1  0]
   [1 -4  1]
   [0  1  0]
   ```

2. **8-neighbor Laplacian**:
   ```
   [1  1  1]
   [1 -8  1]
   [1  1  1]
   ```

![Laplacian Masks](../images/bab3_laplacian-masks.png)

#### Laplacian Enhancement:
```
g(x,y) = f(x,y) - c × ∇²f(x,y)
```
di mana c adalah konstanta positif.

![Laplacian Sharpening Example](../images/bab3_laplacian-sharpening-example.png)

#### Unsharp Masking
Metode sharpening klasik:
1. **Blur citra original**: g_blur
2. **Hitung mask**: mask = f - g_blur
3. **Add mask ke original**: g = f + k × mask

![Unsharp Masking Process](../images/bab3_unsharp-masking-process.png)

#### High-Boost Filtering
Variasi unsharp masking:
```
g(x,y) = A × f(x,y) - (A-1) × f_blur(x,y)
```
di mana A > 1 adalah amplification factor.

## 5. Order-Statistics Filters

### Median Filter
Non-linear filter yang menggunakan median dari neighborhood:
```
g(x,y) = median{f(x+s, y+t) | (s,t) ∈ W}
```
#### Karakteristik:
- Efektif untuk salt-and-pepper noise
- Mempreserve edges
- Non-linear operation

![Median Filter Example](../images/bab3-median-filter-example.png)

### Variations:
1. **Max Filter**: Maximum dari neighborhood
2. **Min Filter**: Minimum dari neighborhood
3. **Midpoint Filter**: (max + min)/2
4. **Alpha-Trimmed Mean Filter**: Menghilangkan extreme values

![Order-Statistics Filters Comparison](../images/bab3-order-statistics-filters.png)

## 6. Gradient-Based Filters

### First-Order Derivatives
Deteksi edges menggunakan turunan pertama.

#### Sobel Operator
Mask untuk Gx (horizontal):
```
[-1  0  1]
[-2  0  2]
[-1  0  1]
```
Mask untuk Gy (vertical):
```
[-1 -2 -1]
[ 0  0  0]
[ 1  2  1]
```

![Sobel Operator Masks](../images/bab3_sobel-operator-masks.png)

Gradient magnitude:
```
|∇f| = √(Gx² + Gy²)
```

Gradient direction:
```
θ = arctan(Gy/Gx)
```

![Sobel Edge Detection Example](../images/bab3_sobel-edge-detection-example.png)

#### Prewitt Operator
Similar dengan Sobel, namun dengan koefisien seragam.

![Prewitt Operator Masks](../images/bab3_prewitt-operator-masks.png)

#### Roberts Cross-Gradient
Operator 2×2 sederhana:
```
Gx = [1  0]    Gy = [0  1]
     [0 -1]        [-1  0]
```

### Second-Order Derivatives
#### Laplacian of Gaussian (LoG)
Kombinasi Gaussian smoothing dan Laplacian:
```
LoG(x,y) = ∇²[G(x,y) × f(x,y)]
```
#### Properties:
- Deteksi edges dengan presisi tinggi
- Sensitif terhadap noise
- Zero-crossing detection

![LoG Filter and Zero-Crossing Detection](../images/bab3-log-filter-zero-crossing.png)

## 7. Adaptive Filters

### Adaptive Median Filter
Median filter dengan ukuran mask adaptif:
1. Ukuran mask minimum: 3×3
2. Ekspansi mask jika noise terdeteksi
3. Statistik local untuk decision making

![Adaptive Median Filter Algorithm](../images/bab3_adaptive-median-filter-algorithm.png)

### Adaptive Local Noise Reduction
Filter yang menyesuaikan berdasarkan karakteristik local noise:
```
g(x,y) = f(x,y) - (σ²_η/σ²_L) × [f(x,y) - m_L]
```
di mana:
- **σ²_η**: Variance noise estimasi
- **σ²_L**: Local variance
- **m_L**: Local mean

![Adaptive Noise Reduction Example](../images/bab3_adaptive-noise-reduction-example.png)

## 8. Transformations for Color Images

### Color Space Considerations
Processing citra warna memerlukan pendekatan khusus:

1. **RGB Space**
   - Process each channel independently
   - Risikonya: color artifacts

2. **HSI/HSV Space**
   - Process intensity channel only
   - Preserve hue and saturation
   - Better for enhancement operations

3. **YCbCr Space**
   - Separate luminance (Y) and chrominance (CbCr)
   - Common in image/video compression

### Color Enhancement Techniques
1. **Histogram Equalization per Channel**
2. **Adaptive Histogram Equalization**
3. **Color Balance Adjustment**
4. **Saturation Enhancement**

## 9. Implementation Considerations

### Computational Complexity
- **Point operations**: O(MN)
- **Linear filtering**: O(MN × k²) untuk k×k mask
- **Median filtering**: O(MN × k² × log k²)
- **Optimization techniques**: Separable filters, integral images

### Boundary Handling
Metode untuk menangani piksel di border:
1. **Zero padding**: Isi dengan nol
2. **Replication**: Duplikasi edge pixels
3. **Wrap-around**: Periodic extension
4. **Mirror extension**: Refleksi simetris

### Hardware Acceleration
1. **GPU Processing**: Parallel filtering operations
2. **SIMD Instructions**: Vector operations
3. **FPGA Implementation**: Real-time applications

## 10. Applications

### Medical Imaging
- Contrast enhancement untuk X-ray, MRI, CT
- Noise reduction untuk ultrasound
- Feature enhancement untuk diagnosis

### Remote Sensing
- Atmospheric correction
- Feature enhancement
- Multi-spectral image processing

### Computer Vision
- Preprocessing untuk object detection
- Edge detection untuk segmentation
- Feature extraction

### Photography
- Contrast and brightness adjustment
- Sharpening and blurring effects
- Artistic filtering

## 11. Advanced Topics

### Multi-scale Filtering
Filtering pada berbagai skala:
- Image pyramids
- Scale-space theory
- Multi-resolution analysis

### Anisotropic Filtering
Direction-dependent filtering:
- Structure-preserving smoothing
- Edge-preserving denoising
- Coherence-enhancing filtering

### Deep Learning Approaches
Learned filters:
- Convolutional Neural Networks (CNNs)
- Autoencoder-based denoising
- Generative Adversarial Networks (GANs) for enhancement

## 12. Performance Evaluation

### Quality Metrics
1. **Mean Squared Error (MSE)**
2. **Peak Signal-to-Noise Ratio (PSNR)**
3. **Structural Similarity Index (SSIM)**
4. **Perceptual quality metrics**

### Visual Assessment
- Subjective evaluation oleh human observers
- Task-based performance measurement
- Computational efficiency analysis

## Kesimpulan

Transformasi intensitas dan filtering spasial merupakan fondasi dalam pengolahan citra digital. Pemahaman yang mendalam tentang berbagai teknik, karakteristik, dan aplikasinya sangat penting untuk:

1. **Image Enhancement**: Meningkatkan kualitas visual citra
2. **Preprocessing**: Menyiapkan citra untuk analisis lanjutan
3. **Noise Reduction**: Menghilangkan noise sambil mempertahankan detail
4. **Feature Enhancement**: Menonjolkan informasi penting dalam citra

Pilihan teknik yang tepat bergantung pada karakteristik citra input, tujuan processing, dan kendala komputasi yang ada.

*Catatan: Materi ini disusun berdasarkan bab 3 yang membahas intensitas transformations dan spatial filtering sebagai teknik fundamental dalam preprocessing citra digital.*

---

## 13. Latihan Soal dan Jawaban

### Soal 3.1: Transformasi Intensitas Dasar

**Pertanyaan:** Diberikan citra grayscale 8-bit dengan rentang intensitas [0, 255]. Jelaskan secara detail cara kerja, kelebihan, dan kekurangan dari tiga jenis transformasi intensitas berikut:
1. Transformasi Linear (Contrast Stretching)
2. Transformasi Logaritmik
3. Transformasi Power-Law (Gamma Correction)

**Jawaban:**

#### 1. Transformasi Linear (Contrast Stretching)

**Cara Kerja:**
- **Langkah 1:** Identifikasi nilai intensitas minimum (r_min) dan maksimum (r_min) dalam citra
- **Langkah 2:** Tentukan rentang output yang diinginkan [0, 255]
- **Langkah 3:** Aplikasikan fungsi transformasi:
  ```
  s = (r - r_min) × (255)/(r_max - r_min)
  ```
  di mana r = intensitas input, s = intensitas output

**Kelebihan:**
- Sederhana dan komputasi cepat
- Meningkatkan kontras global secara efektif
- Memperluas rentang dinamis citra
- Mudah diimplementasikan

**Kekurangan:**
- Sensitif terhadap outlier (noise ekstrem)
- Dapat menyebabkan over-enhancement
- Tidak adaptif untuk berbagai region citra
- Berpotensi loss detail di area saturated

#### 2. Transformasi Logaritmik

**Cara Kerja:**
- **Langkah 1:** Pilih konstanta scaling c untuk mengatur hasil
- **Langkah 2:** Aplikasikan fungsi logaritmik:
  ```
  s = c × log(1 + r)
  ```
- **Langkah 3:** Normalisasi hasil ke rentang [0, 255]

**Kelebihan:**
- Meningkatkan kontras pada area gelap secara signifikan
- Mengompresi rentang dinamis yang besar
- Efektif untuk citra dengan dynamic range tinggi
- Preservasi detail pada low-intensity regions

**Kekurangan:**
- Mengurangi kontras pada area terang
- Dapat menghasilkan citra yang terlalu terang
- Memerlukan penyesuaian konstanta c secara manual
- Tidak linear, sulit untuk interpretasi langsung

#### 3. Transformasi Power-Law (Gamma Correction)

**Cara Kerja:**
- **Langkah 1:** Pilih nilai γ (gamma) dan konstanta c
- **Langkah 2:** Aplikasikan fungsi power-law:
  ```
  s = c × r^γ
  ```
- **Langkah 3:** Normalisasi hasil ke rentang [0, 255]

**Variasi γ:**
- **γ < 1 (0.2-0.8):** Memperluas intensitas rendah (brightening)
- **γ = 1:** Transformasi linear (tidak ada perubahan)
- **γ > 1 (1.2-2.0):** Memperluas intensitas tinggi (darkening)

**Kelebihan:**
- Fleksibel dengan berbagai nilai γ
- Sesuai dengan karakteristik human vision
- Efektif untuk display calibration
- Preservasi detail sesuai kebutuhan

**Kekurangan:**
- Memerlukan pengetahuan tentang nilai γ optimal
- Komputasi lebih kompleks dari linear
- Dapat menyebabkan banding artifacts
- Non-linear relationship sulit untuk prediksi

---

### Soal 3.2: Histogram Processing

**Pertanyaan:** Jelaskan proses histogram equalization secara matematis dan implementasinya. Bandingkan dengan histogram matching, kapan harus menggunakan masing-masing teknik?

**Jawaban:**

#### Histogram Equalization

**Proses Matematis:**

**Langkah 1: Hitung Histogram**
```
h(r_k) = jumlah piksel dengan intensitas r_k
```
untuk k = 0, 1, 2, ..., L-1 (L = 256 untuk 8-bit)

**Langkah 2: Normalisasi Histogram**
```
p(r_k) = h(r_k) / MN
```
di mana MN adalah total piksel dalam citra

**Langkah 3: Hitung CDF (Cumulative Distribution Function)**
```
s_k = Σ_{i=0}^k p(r_i)
```
CDF memberikan probabilitas kumulatif hingga intensitas k

**Langkah 4: Transformasi Intensitas**
```
s_k = round(s_k × (L-1))
```
Map nilai CDF ke rentang intensitas [0, L-1]

**Implementasi Pseudocode:**
```python
def histogram_equalization(image):
    # Step 1: Calculate histogram
    hist = np.zeros(256)
    for pixel in image.flatten():
        hist[pixel] += 1

    # Step 2: Normalize histogram
    pmf = hist / (image.shape[0] * image.shape[1])

    # Step 3: Calculate CDF
    cdf = np.cumsum(pmf)

    # Step 4: Create lookup table
    lookup_table = np.round(cdf * 255).astype('uint8')

    # Step 5: Apply transformation
    equalized = lookup_table[image]

    return equalized
```

**Karakteristik:**
- Otomatis menghasilkan histogram yang flat (uniform)
- Maksimalkan entropy citra
- Tidak memerlukan parameter manual
- Hasil tidak dapat diprediksi sebelumnya

#### Histogram Matching (Specification)

**Proses Matematis:**

**Langkah 1:** Hitung histogram input h_input(r) dan CDF_input
**Langkah 2:** Tentukan histogram target h_target(s) dan CDF_target
**Langkah 3:** Equalize kedua histogram:
```
s_k = CDF_input(r_k)
z_k = CDF_target(s_k)
```
**Langkah 4:** Buat mapping dari input ke target:
```
r_new = CDF_target^(-1)(CDF_input(r))
```

**Kapan Menggunakan:**

**Histogram Equalization cocok untuk:**
- Citra dengan kontras rendah yang tidak jelas
- Preprocessing otomatis tanpa supervisi
- Aplikasi medis untuk menonjolkan detail
- Situasi di mana peningkatan kontras global diperlukan

**Histogram Matching cocok untuk:**
- Standardisasi citra dari berbagai sumber
- Matching karakteristik citra ke citra referensi
- Color correction dan white balance
- Aplikasi di mana output yang spesifik diinginkan
- Multi-sensor image fusion

**Contoh Aplikasi:**
- **Medical Imaging:** Equalization untuk X-ray enhancement
- **Photography:** Matching untuk consistent color grading
- **Remote Sensing:** Standardisasi citra dari waktu berbeda
- **Quality Control:** Matching dengan citra golden standard

---

### Soal 3.3: Spatial Filtering untuk Noise Reduction

**Pertanyaan:** Bandingkan performa dari tiga filter berikut untuk mengurangi Gaussian noise dan Salt-and-Pepper noise. Analisis kelebihan dan kekurangan masing-masing:
1. Mean Filter (3×3)
2. Median Filter (3×3)
3. Gaussian Filter (σ=1.0)

Berikan rekomendasi filter terbaik untuk setiap jenis noise.

**Jawaban:**

#### Analisis Komprehensif Filter

**1. Mean Filter (3×3)**

**Cara Kerja:**
```
g(x,y) = (1/9) × Σ_{i=-1}^{1} Σ_{j=-1}^{1} f(x+i, y+j)
```

**Performa Gaussian Noise:**
- **Kelebihan:** Mengurangi noise secara efektif karena averaging
- **Kekurangan:** Menyebabkan blurring yang signifikan
- **PSNR:** Sedang (~25-30 dB untuk σ=25)
- **Edge Preservation:** Buruk, edges menjadi blur

**Performa Salt-and-Pepper Noise:**
- **Kelebihan:** Mengurangi densitas noise secara uniform
- **Kekurangan:** Tidak efektif menghilangkan extreme values
- **Artifacts:** Noise terdistribusi kembali ke neighbor
- **Visual Quality:** Buruk, masih terlihat noise spots

**2. Median Filter (3×3)**

**Cara Kerja:**
```
g(x,y) = median{f(x+i, y+j) | i,j ∈ {-1,0,1}}
```

**Performa Gaussian Noise:**
- **Kelebihan:** Preservasi edges lebih baik dari mean filter
- **Kekurangan:** Kurang efektif untuk noise Gaussian murni
- **PSNR:** Rendah-menengah (~20-25 dB untuk σ=25)
- **Edge Preservation:** Baik, edges tajam terpelihara

**Performa Salt-and-Pepper Noise:**
- **Kelebihan:** Sangat efektif menghilangkan extreme values
- **Kekurangan:** Dapat menghilangkan detail halus
- **Artifacts:** Minimal untuk low-to-medium noise density
- **Visual Quality:** Sangat baik, hampir tidak ada noise

**3. Gaussian Filter (σ=1.0)**

**Cara Kerja:**
```
G(x,y) = (1/2πσ²) × exp(-(x²+y²)/2σ²)
g(x,y) = Σ Σ G(i,j) × f(x+i, y+j)
```
Untuk σ=1.0, kernel 3×3:
```
1/16 [1  2  1]
     [2  4  2]
     [1  2  1]
```

**Performa Gaussian Noise:**
- **Kelebihan:** Optimal untuk Gaussian noise (theoretically optimal)
- **Kekurangan:** Masih menyebabkan blurring moderate
- **PSNR:** Baik (~28-32 dB untuk σ=25)
- **Edge Preservation:** Sedang, edges masih terlihat

**Performa Salt-and-Pepper Noise:**
- **Kelebihan:** Sedikit lebih baik dari mean filter
- **Kekurangan:** Tetap tidak efektif untuk impulse noise
- **Artifacts:** Noise terdistribusi dengan weighting
- **Visual Quality:** Buruk, noise masih jelas terlihat

#### Perbandingan Kuantitatif

| Metrik | Mean Filter | Median Filter | Gaussian Filter |
|--------|-------------|---------------|-----------------|
| **Gaussian Noise PSNR** | 26-30 dB | 22-26 dB | 28-32 dB |
| **Salt-Pepper Noise PSNR** | 18-22 dB | 30-35 dB | 20-24 dB |
| **Kecepatan Komputasi** | Cepat | Sedang | Cepat |
| **Edge Preservation** | Buruk | Baik | Sedang |
| **Memory Requirement** | Rendah | Sedang | Rendah |

#### Rekomendasi Filter Terbaik

**Untuk Gaussian Noise:**
1. **Pilihan Terbaik:** Gaussian Filter (σ=1.0)
   - Alasan: Optimal secara matematis untuk Gaussian noise
   - Kasus penggunaan: Photography, medical imaging, natural scenes

2. **Pilihan Alternatif:** Mean Filter
   - Alasan: Lebih sederhana dan cepat
   - Kasus penggunaan: Real-time applications, low-power devices

**Untuk Salt-and-Pepper Noise:**
1. **Pilihan Terbaik:** Median Filter (3×3)
   - Alasan: Sangat efektif menghilangkan impulse noise
   - Kasus penggunaan: Image restoration, document scanning, satellite imagery

2. **Pilihan Alternatif:** Adaptive Median Filter
   - Alusan: Menyesuaikan ukuran kernel secara dinamis
   - Kasus penggunaan: Variable noise density, critical applications

**Rekomendasi Hybrid Approach:**
Untuk citra dengan mixed noise types:
```python
def hybrid_filter(image):
    # Step 1: Remove salt-and-pepper noise
    denoised = median_filter(image, 3×3)

    # Step 2: Reduce remaining Gaussian noise
    final = gaussian_filter(denoised, σ=0.8)

    return final
```

**Kriteria Pemilihan Filter:**
- **Tipe Noise:** Sesuaikan filter dengan karakteristik noise
- **Preservasi Detail:** Prioritaskan median filter untuk edges
- **Kualitas Visual:** Gaussian untuk natural appearance
- **Constraint Komputasi:** Mean filter untuk real-time applications

**Kesimpulan:**
Tidak ada satu filter yang universal terbaik. Pilihan filter harus didasarkan pada:
1. Karakteristik noise dominan
2. Tingkat preservasi detail yang dibutuhkan
3. Constraint komputasi dan real-time requirements
4. Kualitas visual yang diinginkan untuk aplikasi spesifik