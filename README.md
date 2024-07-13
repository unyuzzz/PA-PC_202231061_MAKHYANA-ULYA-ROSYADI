# Penjelasan mengenai python filtering citra
##### Pengerjaan filtering digunakan foto dengan diambil menggunakan hp tipe Iphone x dengan jarak minimal 2 meter. 
##### berikut pengubahan men-filtering citranya:

sebelum melanjutkan, perlu adanya memanggil beberapa library yang akan digunakan kedepannya
```bash
import cv2   # untuk berbagi operasi pengolahan termasuk merupakan bagian opencv
import numpy as np  # untuk pengerjaan operasi array dan juga perhitungan numerilk
from matplotlib import pyplot as plt  #untuk kita dapat membuat visualisasi data
```

bagian fungsi untuk pengubahan mean filter pada gambar grayscale
kemudian adanya pembuatan kernel dengan ukuran filter_size x filter_size dengan semua elemen bernilai 1
dan membaginya ke setiap elemen dengan jumlah elemen dalam kernel, sehingga setiap elemen memiliki nilai yang sama, 
dan bagian ini untuk dapat memfilter mean kepada citra.
``` bash
def mean_filtered(image, filter_size): #mendefinisikan fungsi dengan berdasarkan (gambar yg akan diubah, size mem-filternya)
    kernel = np.ones((filter_size, filter_size), np.float32) / (filter_size * filter_size) 
    height, width = image.shape
    mean_filtered_image = np.zeros((height, width), np.float32)
    padded_image = cv2.copyMakeBorder(image, filter_size//2, filter_size//2, filter_size//2, filter_size//2, cv2.BORDER_REFLECT)
    
    for i in range(height):
        for j in range(width):
            mean_filtered_image[i, j] = np.sum(padded_image[i:i+filter_size, j:j+filter_size] * kernel)
    
    return np.clip(mean_filtered_image, 0, 255).astype(np.uint8)
```

memanggil dan membaca citra yang akan diolah atau difilter dengan di-inisialisasi image_color
```bash
image_color = cv2.imread("ulya.jpg")
```

bagian pendefinisian dengan inisialisasi
```bash
image_gray = cv2.cvtColor(image_color, cv2.COLOR_BGR2GRAY) # definisi untuk konversi citra dalam bentuk grayscale
median_filtered = cv2.medianBlur(image_color, 3) # untuk definisi citra terhadap median filter dengan ukuran jendela 3x3 pada citra berwarna
mean_filtered = mean_filtered(image_gray, 3) # untuk definisi mean filter
```

### bagian yang akan menampilkan hasil citra setelah adanya filterisasi 
membuat sebuah figure baru dengan ukuran 18x18 inch, yang dimana figure untuk menempatkan citra dan
```bash
plt.figure(figsize=(18, 18))
```

untuk menempatkan gambar asli yang sudah dibaca oleh imread, yang kemudian dengan format RGB dikonversi ke BGR(format default Opencv)
ke RGB dengan ditempatkan pada subplot pertama dengan dinamakan "Citra Asli".
```bash
plt.subplot(141), plt.imshow(cv2.cvtColor(image_color, cv2.COLOR_BGR2RGB)), plt.title('Citra Asli')
```

untuk selanjutnya akan ditempatkan hasil dari filter median dengan median_filtered yang sebelumnya sudah didefinisikan,
dengan ditempatkan pada subplot kedua dengan dinamakan "after filter median".
```bash
plt.subplot(142), plt.imshow(cv2.cvtColor(median_filtered, cv2.COLOR_BGR2RGB)), plt.title('After filter median')
```

menempatkan gambar asli dengan tampilan gray untuk compare dengan mean filter yang akan ditempatkan pada subpplot ketiga
dengan dinamakan "original image"
```bash
plt.subplot(143), plt.imshow(image_gray, cmap='gray'), plt.title('Original Image')
```

subplot terakhir atau keempat yang akan menampilkan hasil dari filer mean, yang akan diberi judul subplotnya dengan "mean filtered image"
```bash
plt.subplot(144), plt.imshow(mean_filtered, cmap='gray'), plt.title('Mean Filtered Image')
plt.show() # kemudian tampilkan semua subplot yang sudah diatur
```

## full tampilan code nya
```bash
import cv2
import numpy as np
import matplotlib.pyplot as plt

def mean_filtered(image, filter_size):
    kernel = np.ones((filter_size, filter_size), np.float32) / (filter_size * filter_size)
    height, width = image.shape
    mean_filtered_image = np.zeros((height, width), np.float32)
    padded_image = cv2.copyMakeBorder(image, filter_size//2, filter_size//2, filter_size//2, filter_size//2, cv2.BORDER_REFLECT)
    
    for i in range(height):
        for j in range(width):
            mean_filtered_image[i, j] = np.sum(padded_image[i:i+filter_size, j:j+filter_size] * kernel)
    
    return np.clip(mean_filtered_image, 0, 255).astype(np.uint8)

image_color = cv2.imread("ulya.jpg")

image_gray = cv2.cvtColor(image_color, cv2.COLOR_BGR2GRAY)
median_filtered = cv2.medianBlur(image_color, 3)
mean_filtered = mean_filtered(image_gray, 3)

plt.figure(figsize=(18, 18))
plt.subplot(141), plt.imshow(cv2.cvtColor(image_color, cv2.COLOR_BGR2RGB)), plt.title('Citra Asli')
plt.subplot(142), plt.imshow(cv2.cvtColor(median_filtered, cv2.COLOR_BGR2RGB)), plt.title('After filter median')
plt.subplot(143), plt.imshow(image_gray, cmap='gray'), plt.title('Original Image')
plt.subplot(144), plt.imshow(mean_filtered, cmap='gray'), plt.title('Mean Filtered Image')
plt.show()
```
