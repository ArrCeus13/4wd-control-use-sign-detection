# Kontrol Mobil Robot 4WD Dengan Deteksi Rambu Lalu Lintas

<p align="center">
   <img src="../doc/TFLite-vs-EdgeTPU.gif">
</p>

## Spesifikasi Alat
Mobil (Hardware)
* Case Akrilik
* 18 Pasang Baut dan Mur
* 4 Spacer 5 cm
* 4 Dinamo 5-12 V
* 4 Ban Karet
* 2 Motor Driver L298N
* Kabel Jumper
* Raspberry Pi 4B 4 GB
* Kamera
* Powerbank

Model Deteksi Rambu (Software)
* Python
* RPIO (Library untuk input output Raspberry Pi)
* Tensorflow (Library untuk Pengembangan model)
* TFLite (Library untuk implementasi model pada Raspberry PI)

## Proses Pembuatan

### 1. Perakitan Mobil
Perakitan Mobil dimulai dengan merakit part dari case akrilik yang disediakan, kemudian menyolder kabel power pada dinamo serta memasang dinamo pada case akrilik. Kemudian, memasang motor driver pada case akrilik serta menghubungkan kabel power pada keempat dinamo menuju soket output dari kedua motor driver

Selanjutnya, dengan menggunakan kabel jumper, menghubungkan soket input dari kedua motor driver menuju GPIO dari Raspberry Pi, serta menyambungkan powerbank dengan Raspberry Pi sebagai Power Supply 5V dari alat.

Berikut adalah diagram kabel dari alat :
<p align="center">
  <img src="/src/image/image3.png">
</p>

### 2. Pengembangan Model
Sebelum ke langkah langakah pengembangan model, ikuti [link ini](https://colab.research.google.com/drive/1yvxqzuNJgQZ4EPWkyVXEdsqmkMQCIvKV?usp=sharing) untuk melihat source pengembangan modelnya yang mengikuti referensi dari github [ini](https://github.com/EdjeElectronics/TensorFlow-Lite-Object-Detection-on-Android-and-Raspberry-Pi).
Berikut adalah langkah langkahnya:
* Pengumpulan dataset
Dataset yang digunakan pada proyek ini adalah dataset rambu lalu lintas belok kanan, belok kiri, jalan terus dan tanda berhenti. Berikut adalah [link datasetnya](https://app.roboflow.com/anggito/straightleftrightstop/3)
* Install kelengkapan deteksi objek menggunakan Tensorflow
* Upload dataset dan persiapan melakukan pelatihan model
* Mengatur konfigurasi pelatihan model
* Latih model deteksi objek
* Konversi model ke Tensorflow Lite
* Menguji model Tensorflow Lite dan menghitung mAP
* Deploy model Tensorflow Lite

## Kode Program Pada Raspberry Pi
Program yang dibuat adalah untuk melakukan deteksi rambu lalu lintas berdasarkan model yang sudah dikembangankan dan juga untuk mengontrol motor driver pergerakan mobil robot 4WD.
[This Link](/src/TFLite_detection_webcam.py)

Catatan : Ubah konfigurasi pin jika pergerakan mobil tidak sesuai


## Penggunaan Kode Program
* Pada [referensi](https://github.com/EdjeElectronics/TensorFlow-Lite-Object-Detection-on-Android-and-Raspberry-Pi) hal pertama yang dilakukan adalah membuat sebuah virtual environment, kemudian menginstall library yang dibutuhkan pada file [get_pi_requirements.sh](/src/get_pi_requirements.sh)
* Setelah selesai membuat virtual environment dan selesai melakukan pelatihan model, jalankan kode program sebelumnya untuk mulai mendeteksi objek dan mengontrol gerak mobil robot chassis. Perlu diingat untuk masuk terlebih dahulu ke dalam environment yang dibuat dengan perintah:

```
source $env-path$/bin/activate
```
* Jalankan kode program yang dibuat dalam virtual environment dengan perintah: 
```
python TFLite_detection_webcam.py --modeldir=TFLite_model
```
Ganti “TFLite_model” dengan direktori model yang sudah kita latih sendiri

## Hasil dan Pembahasan
<p align="center">
  <img src="/src/image/image2.png">
</p>
Grafik diatas menggambarkan performa dari model. Dapat dilihat dari grafik tersebut, nilai mAP menurun drastis dimulai dari nilai IoU threshold 0.90 mencapai 0.95. IoU threshold adalah pengukuran performa dimana model akan diuji prediksi bounding boxnya (prediction) dan bagaimana seharusnya (truth). Terjadinya penurunan drastis karena model harus memprediksi ukuran dan lokasi bounding box dengan presisi yang sangat tinggi pada nilai IoU tinggi, dan berdasarkan grafik tersebut model belum bisa memprediksi dengan presisi yang sangat tinggi pada IoU > 0.90. Performa tersebut masih dapat diterima dikarenakan penggunaan model dalam praktik menggunakan gambar yang mudah dilihat, jadi pada nilai akurasi mAP - IoU Threshold yang bernilai 95.70% - 0.90 merupakan performa yang lumayan bagus. 
<p align="center">
  <img src="/src/image/image5.png">
</p>
Gambar di atas menjelaskan secara umum performa model pada rentang IoU Threshold 0.50 - 0.95. Dapat dilihat setiap kelas memiliki nilai rata-rata di atas 90% dan secara umum memiliki mean Average Precision sekitar 94.65%.

<p align="center">
  <img src="/src/image/image1.png">
</p>
<p align="center">
  <img src="/src/image/image4.png">
</p>
