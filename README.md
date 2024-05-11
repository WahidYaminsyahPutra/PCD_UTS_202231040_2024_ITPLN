# PCD_UTS_202231040_2024_ITPLN
# Laporan <br>
## Soal No.1
1. Import Library<br>
Pertama membuat labrary untuk mengimport dan menampilkan gambar dengan perintah:
```python
import cv2 #labrary untuk mengimport dan menampilkan gambar
import numpy as np
import matplotlib.pyplot as plt
```
2. untuk membaca gambar<br>
Ke dua untuk membaca gambar menggunakan perintah:
```python
img = cv2.imread("WahidUts.jpg")
```
3. untuk menampilkan ukuran atau dimensi gambar menggunakan perintah:
```python
img.shape
(1200, 1600, 3)
```
4. membuat baris dan kolom menggunakan perintah:
```python
(baris, kolom) = img.shape[:2]
```
5. mengkonversi gambar dari BGR ke RGB:
```python
img = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
```
6. menampilkan hasil gambar yang di konversi dari BGR ke RGB:
```python
plt.imshow(img)
```
7. Mendeteksi Warna Pada Citra:
```python
#gambar citra kontras
plt.subplot(2, 2, 1)
plt.imshow(img)
plt.title('citra kontras')

#deteksi warna biru
plt.subplot(2, 2, 4)
plt.imshow(img[:,:,2], cmap="gray") 
plt.title('biru')

#deteksi warna merah
plt.subplot(2, 2, 2)
plt.imshow(img[:,:,0], cmap="gray") 
plt.title('merah')

#deteksi warna hijau
plt.subplot(2, 2, 3)
plt.imshow(img[:,:,1], cmap="gray") 
plt.title('hijau')

plt.show()
```
analisis deteksi warna citra :
o	Deteksi Warna Biru :
Di output ini, gambar akan ditampilkan dalam skala abu-abu "gray".  bagian-bagian gambar yang memiliki warna biru akan tampak gelap, sementara yang lainnya menjadi lebih terang.
o	Deteksi Warna Merah :
Pada output ini, gambar akan ditampilkan dalam skala abu-abu. Ini akan menyoroti bagian-bagian gambar yang memiliki warna merah menjadi pudar atau seperti tidak tebal.
o	Deteksi Warna Hijau :
Di output ini, gambar akan ditampilkan dalam skala abu-abu. Ini akan menyoroti bagian-bagian gambar yang memiliki warna hijau menjadi pudar atau seperti tidak tebal, sementara warna lain menjadi terang atau tebal.

8. Menampilkan HIstogram:
```python
fig, axs = plt.subplots(3, 2, figsize=(15, 15))

#Biru
biru = img[:, :, 2]
hist_biru = cv2.calcHist([biru], [0], None, [256], [0, 256])
axs[2, 0].imshow(biru, cmap='gray')
axs[2, 0].set_title('biru')
axs[2, 1].plot(hist_biru, color='b')
axs[2, 1].set_title('histogram biru')

#Merah
merah = img[:, :, 0]
hist_merah = cv2.calcHist([merah], [0], None, [256], [0, 256])
axs[0, 0].imshow(merah, cmap='gray')
axs[0, 0].set_title('merah')
axs[0, 1].plot(hist_merah, color='r')
axs[0, 1].set_title('histogram merah')

#Hijau
hijau = img[:, :, 1]
hist_hijau = cv2.calcHist([hijau], [0], None, [256], [0, 256])
axs[1, 0].imshow(hijau, cmap='gray')
axs[1, 0].set_title('hijau')
axs[1, 1].plot(hist_hijau, color='g')
axs[1, 1].set_title('histogram hijau')

plt.tight_layout()
plt.show()
```
analisis histogram:
o	Histogram Merah: Histogram merah menunjukkan distribusi intensitas warna merah di seluruh gambar. Puncak histogram yang tinggi menunjukkan banyaknya piksel dengan intensitas merah yaitu 140000. Untuk melihat seberapa banyak area di gambar yang memiliki tingkat kecerahan merah tertentu.
o	Histogram hijau: Puncak histogram yang tinggi menunjukkan banyaknya piksel dengan intensitas hijau yaitu 120000.
o	Histogram Biru: Puncak histogram yang tinggi menunjukkan bahwa ada banyak piksel dengan intensitas biru yaitu 200000+. 

9.Menampilkan Ambang Batas Citra
```python
# Konversi citra ke ruang warna HSV
hsv = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)
fig, axs=plt.subplots(2,2, figsize =(10,10))
# Tentukan batas atas dan batas bawah yang ekstrem sehingga tidak ada warna yang cocok
lower_threshold = np.array([0, 0, 0])
upper_threshold = np.array([0, 0, 0])

# Buat mask dengan batas atas dan batas bawah yang ekstrem
mask = cv2.inRange(hsv, lower_threshold, upper_threshold)

# Tampilkan mask
axs[0, 0].imshow(mask, cmap='gray')
axs[0, 0].set_title('None')

# Tentukan batas warna biru
lower_blue = np.array([100, 50, 50])
upper_blue = np.array([130, 255, 255])

# Buat mask untuk warna biru
mask = cv2.inRange(hsv, lower_blue, upper_blue)

# Tampilkan mask untuk warna biru
axs[0, 1].imshow(mask, cmap='gray')
axs[0, 1].set_title('Blue')

# Tentukan batas atas dan batas bawah untuk warna merah dalam ruang warna HSV
lower_red1 = np.array([0, 50, 50])
upper_red1 = np.array([10, 255, 255])
lower_red2 = np.array([170, 50, 50])
upper_red2 = np.array([180, 255, 255])

# Buat mask untuk warna biru dan merah
mask_blue = cv2.inRange(hsv, lower_blue, upper_blue)
mask_red1 = cv2.inRange(hsv, lower_red1, upper_red1)
mask_red2 = cv2.inRange(hsv, lower_red2, upper_red2)

# Gabungkan mask biru dan mask merah
mask_combined = cv2.bitwise_or(mask_blue, mask_red1)
mask_combined = cv2.bitwise_or(mask_combined, mask_red2)

# Tampilkan mask
axs[1, 0].imshow(mask_combined, cmap='gray')
axs[1, 0].set_title('Red-Blue')

(thresh, binary4) = cv2.threshold(gray, 145, 255, cv2.THRESH_BINARY)
axs[1,1].imshow(binary4, cmap = 'gray')
axs[1,1].set_title('RED-GREEN-BLUE')

plt.show()
```
- Citra awal (`img`) dikonversi ke ruang warna HSV menggunakan fungsi `cv2.cvtColor()` dengan parameter `cv2.COLOR_BGR2HSV`. Hasilnya disimpan dalam variabel `hsv`.
- Membuat sebuah figure dan empat subplots dengan ukuran 2x2 menggunakan `plt.subplots(2, 2, figsize=(10, 10))`. Subplots akan digunakan untuk menampilkan masker hasil segmentasi.
- Menginisialisasi `lower_threshold` dan `upper_threshold` dengan nilai nol.
- Membuat masker dengan menggunakan `cv2.inRange()` dengan batas atas dan batas bawah yang ekstrem. Masker ini akan menunjukkan piksel mana yang tidak cocok dengan rentang warna apapun. Hasilnya ditampilkan di subplot pertama sebagai "None".
- Menentukan batas atas dan bawah untuk warna biru dalam ruang warna HSV.
- Membuat masker menggunakan `cv2.inRange()` dengan batas atas dan bawah tersebut.
- Menampilkan masker hasil segmentasi warna biru di subplot kedua.
- Menentukan dua rentang batas atas dan bawah untuk warna merah dalam ruang warna HSV karena warna merah dapat memiliki dua variasi di dalam rentang hue.
- Membuat masker untuk warna merah menggunakan `cv2.inRange()` dengan dua rentang tersebut.
- Menggabungkan masker biru dan masker merah untuk memperoleh masker yang mengidentifikasi piksel yang cocok dengan warna merah atau biru.
- Menampilkan masker hasil segmentasi warna merah dan biru di subplot ketiga.
- Melakukan thresholding pada citra awal (asumsinya adalah citra grayscale) menggunakan `cv2.threshold()` dengan ambang batas 145.
- Hasil thresholding ditampilkan di subplot keempat sebagai "RED-GREEN-BLUE".
- Menggunakan `plt.show()` untuk menampilkan semua subplots.

teori Pendukung 
[https://www.geeksforgeeks.org/opencv-python-tutorial/](https://docs.opencv.org/3.4/de/d25/imgproc_color_conversions.html)
https://docs.opencv.org/3.4/d8/dbc/tutorial_histogram_calculation.html
