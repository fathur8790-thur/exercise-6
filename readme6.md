# Exercise 6 - Menunjukkan Cara Menghilangkan Kontensi Sumber Daya dengan Menangguhkan Scheduler

## Deskripsi Percobaan
Pada **Exercise 6**, kita akan mempelajari bagaimana cara menghilangkan kontensi (konflik) akses sumber daya di sistem multitasking dengan menggunakan teknik *penangguhan scheduler*. Dalam sistem multitasking berbasis FreeRTOS, beberapa tugas mungkin perlu mengakses sumber daya bersama seperti variabel atau perangkat keras (misalnya LED). Tanpa pengaturan yang tepat, beberapa tugas dapat mengakses sumber daya tersebut secara bersamaan, yang dapat menyebabkan kontensi dan mengakibatkan kerusakan data atau perilaku yang tidak diinginkan.

Untuk mencegah masalah ini, kita akan menggunakan fungsi dari FreeRTOS, yaitu `taskENTER_CRITICAL()` dan `taskEXIT_CRITICAL()`, untuk menangguhkan scheduler sementara tugas sedang mengakses sumber daya bersama. Dengan cara ini, hanya satu tugas yang dapat mengakses sumber daya pada satu waktu, mencegah kontensi.

## Tujuan Percobaan
Percobaan ini bertujuan untuk:
1. Menunjukkan bagaimana kontensi dapat terjadi saat beberapa tugas mengakses sumber daya bersama secara bersamaan.
2. Menggunakan teknik penangguhan scheduler untuk mencegah kontensi dan memastikan hanya satu tugas yang mengakses sumber daya pada satu waktu.

## Langkah-langkah Percobaan

### 1. Persiapan Awal
- Pastikan Anda sudah memiliki proyek dengan dua tugas yang mengakses LED (misalnya, LED Hijau dan LED Merah) sebagai sumber daya bersama.
- Dalam percobaan ini, Anda akan menggunakan dua tugas yang melakukan operasi berikut:
  - **Tugas LED Hijau**: Menyalakan LED Hijau selama 4 detik, kemudian mati.
  - **Tugas LED Merah**: Menyalakan LED Merah selama 0.5 detik, kemudian mati.
  
- Tugas ini akan berbagi sumber daya bersama, misalnya sebuah variabel atau LED, dan Anda akan mengamati bagaimana menangguhkan scheduler dapat mencegah kontensi saat mengakses sumber daya tersebut.

### 2. Menambahkan Penangguhan Scheduler dengan `taskENTER_CRITICAL()` dan `taskEXIT_CRITICAL()`
**Tujuan**: Mencegah kontensi dengan menangguhkan scheduler selama tugas mengakses sumber daya bersama.

**Langkah-langkah**:
- Modifikasi kode untuk menangguhkan scheduler dengan menggunakan fungsi `taskENTER_CRITICAL()` dan `taskEXIT_CRITICAL()` dari FreeRTOS.
- Fungsi `taskENTER_CRITICAL()` akan menonaktifkan *interrupt*, yang mencegah tugas lain untuk mengakses sumber daya sampai tugas yang sedang berjalan selesai.
- Fungsi `taskEXIT_CRITICAL()` akan mengaktifkan kembali *interrupt* dan memungkinkan tugas lain untuk dilanjutkan.

### 3. Menjalankan Percobaan
Tujuan: Menjalankan sistem dengan dua tugas yang aktif untuk mengamati pengaruh penangguhan scheduler.

Langkah-langkah:

- Aktifkan kedua tugas dalam kode Anda.
- Jalankan program dan amati LED pada board.
- Tanpa penangguhan scheduler (sebelum modifikasi), LED Biru akan menyala jika kontensi terjadi.
- Dengan penangguhan scheduler (setelah modifikasi), LED Biru tidak akan menyala, menunjukkan bahwa kontensi dapat dihindari.

### 4. Menganalisis Hasil Percobaan
- Tanpa Penangguhan Scheduler: Jika Anda tidak menangguhkan scheduler, LED Biru akan menyala setiap kali kontensi terjadi. Ini menunjukkan bahwa dua tugas mengakses sumber daya secara bersamaan, yang menyebabkan konflik.
- Dengan Penangguhan Scheduler: Dengan menggunakan taskENTER_CRITICAL() dan taskEXIT_CRITICAL(), LED Biru tidak akan menyala, yang menunjukkan bahwa hanya satu tugas yang mengakses sumber daya pada satu waktu, menghindari kontensi.

### 5. Kesimpulan
Percobaan ini menunjukkan bahwa menangguhkan scheduler dapat mencegah kontensi akses ke sumber daya bersama. Dengan menggunakan mekanisme ini, kita dapat memastikan bahwa hanya satu tugas yang mengakses sumber daya pada satu waktu, sehingga menghindari masalah seperti data korupsi atau perilaku sistem yang tidak diinginkan.
Namun, perlu diingat bahwa menangguhkan scheduler dapat mempengaruhi performa sistem secara keseluruhan, karena semua multitasking dihentikan selama akses ke sumber daya. Oleh karena itu, penggunaan teknik ini harus dilakukan dengan hati-hati dan hanya pada bagian kode yang benar-benar memerlukan pengendalian akses ketat.
