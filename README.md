# Modul-12-Prak.-Mobile-Programming

Tujuan Praktikum
Tujuan dari praktikum ini adalah untuk memahami proses implementasi fitur lokasi perangkat (GPS) dalam aplikasi Flutter. Secara spesifik, praktikum ini mencakup pemahaman mendalam mengenai:
Cara pengambilan data lokasi perangkat (koordinat latitude dan longitude) secara real-time.
Penerapan dan penggunaan paket geolocator untuk berinteraksi dengan sensor GPS perangkat.
Penerapan dan penggunaan paket geocoding untuk melakukan reverse geocoding, yaitu mengubah data koordinat menjadi alamat yang dapat dibaca manusia (seperti kecamatan dan kota).
Proses penanganan izin (permission) akses lokasi di platform Android.
Penanganan error yang mungkin terjadi, seperti saat layanan GPS tidak aktif atau saat pengguna menolak izin lokasi.
Pembuatan antarmuka pengguna (UI) yang interaktif dan responsif, yang dapat menampilkan status berbeda (loading, sukses, error) berdasarkan data lokasi.

Langkah Kerja Singkat
Praktikum dilaksanakan melalui langkah-langkah sistematis berikut:
Konfigurasi Izin Android: Menambahkan izin ACCESS_FINE_LOCATION dan ACCESS_COARSE_LOCATION ke dalam file android/app/src/main/AndroidManifest.xml untuk memberikan aplikasi hak akses ke GPS perangkat.
Penambahan Dependensi: Menambahkan paket geolocator, geocoding dan google_fonts ke dalam file pubspec.yaml dan menjalankan perintah flutter pub get untuk mengintegrasikan paket ke dalam proyek.
Pembuatan UI (Antarmuka): Mendesain tampilan aplikasi menggunakan widget utama Scaffold, AppBar, Card untuk menampung informasi, dan ElevatedButton sebagai tombol pemicu aksi pengambilan lokasi.
Implementasi Logika _getLocation(): Membuat fungsi inti _getLocation yang menangani seluruh alur proses. Fungsi ini mencakup:
Pengecekan status layanan lokasi (GPS) apakah aktif atau tidak.
Pengecekan dan permintaan izin (permission) akses lokasi kepada pengguna.
Pengambilan data posisi (latitude dan longitude) perangkat saat ini.
Pelaksanaan reverse geocoding untuk mengubah data koordinat menjadi alamat.
Manajemen Status dengan setState(): Menggunakan setState() untuk memperbarui variabel status (isLoading, _errorMessage, _kecamatan, _kota) yang kemudian secara otomatis memicu rebuild UI untuk menampilkan perubahan.

Screenshot
Keterangan 1: Tampilan awal aplikasi sebelum lokasi diambil. Menampilkan instruksi "Tekan tombol untuk menampilkan lokasi."
Keterangan 2: Tampilan setelah lokasi berhasil didapatkan. Informasi Kelurahan/Kecamatan dan Kota ditampilkan dengan jelas di dalam kartu.	

Penjelasan Kode dan Alur Logika
Aplikasi ini bekerja secara dinamis berkat manajemen status (state management) yang sederhana namun efektif menggunakan StatefulWidget.
1. Peran Variabel Status Tampilan UI diatur oleh empat variabel status utama. Method build() secara logis memeriksa nilai-nilainya di setiap rebuild:
_isLoading (bool): Jika bernilai true , UI akan menampilkan Circular_Progress_Indicator dan menonaktifkan tombol. Ini menandakan aplikasi sedang sibuk memproses.
_errorMessage (String?): Jika tidak null (berisi pesan error), UI akan menampilkan teks error tersebut (diberi warna merah).
_kecamatan dan _kota (String?): Jika kedua variabel ini null (dan tidak sedang loading/error), UI akan menampilkan teks instruksi awal. Jika sudah berisi data, UI akan menampilkan data lokasi tersebut.
2. Fungsi Inti _getLocation() Ini adalah fungsi async yang menjadi "otak" dari aplikasi. Saat tombol ElevatedButton ditekan, fungsi ini dipanggil dan melakukan langkah-langkah berikut:
Memulai Proses: Memanggil setState() untuk mengatur isLoading = true dan membersihkan data/error sebelumnya (_errorMessage, _kecamatan, _kota di-set null).
Validasi Layanan & Izin: Menggunakan geolocator untuk memeriksa apakah GPS aktif (isLocationEnabled()) dan apakah izin telah diberikan (checkPermission).
Meminta Izin: Jika izin ditolak (denied), ia akan meminta izin kepada pengguna (requestPermission()). Jika ditolak permanen (deniedForever), ia akan melempar (throw) Exception berisi pesan instruksi.
Pengambilan Posisi: Jika semua validasi lolos, ia memanggil Geolocator.getCurrentPosition() untuk mendapatkan koordinat GPS (latitude dan longitude).
Reverse Geocoding: Koordinat tersebut kemudian diubah menjadi alamat yang dapat dibaca manusia menggunakan placemarkFromCoordinates() dari paket geocoding.
Pembaruan Status (Sukses): Data alamat yang relevan (place.subLocality dan place.subAdministrativeArea) disimpan ke variabel _kecamatan dan _kota di dalam setState().
Penanganan Error (Catch): Jika terjadi Exception di langkah mana pun (misal, GPS mati, izin ditolak, tidak ada alamat ditemukan), blok catch akan menangkapnya dan menyimpan pesan error ke _errorMessage via setState().
Proses Selesai (Finally): Blok finally selalu dieksekusi, baik proses sukses maupun gagal, untuk memanggil setState() dan mengatur _isLoading = false, yang akan menghentikan spinner dan mengaktifkan kembali tombol.
3. Peran setState() setState() adalah fungsi fundamental dalam StatefulWidget. Panggilannya memberi sinyal kepada framework Flutter bahwa state (data internal) dari widget ini telah berubah. Ini memicu Flutter untuk menjadwalkan rebuild (pembangunan ulang) method build().
Dalam aplikasi ini, setiap kali _isLoading _errorMessage, atau data _kecamatan & _kota diubah di dalam setState(), method build() akan dieksekusi ulang. Pada eksekusi ulang inilah, logika kondisional di dalam build() (yang memeriksa variabel-variabel tersebut) akan berjalan kembali dan menampilkan widget yang sesuai dengan kondisi terbaru (spinner, teks error, atau data lokasi).

Kesimpulan
Berdasarkan praktikum yang telah dilaksanakan, dapat disimpulkan bahwa:
Aplikasi Flutter dapat menampilkan lokasi perangkat secara real-time dengan efektif menggunakan kombinasi paket geolocator untuk akuisisi koordinat GPS dan geocoding untuk reverse geocoding (mengubah koordinat menjadi alamat).
Penanganan izin (ACCESS_FINE_LOCATION di AndroidManifest) dan pengecekan status layanan (GPS aktif/nonaktif) secara programatik adalah langkah krusial yang harus diimplementasikan sebelum mengambil data lokasi untuk memastikan aplikasi berjalan stabil dan ramah pengguna.
Manajemen status menggunakan setState() adalah kunci untuk membuat antarmuka yang dinamis dan responsif. Dengan memperbarui variabel status seperti _isLoading, _errorMessage, dan data lokasi di dalam setState(), UI dapat secara otomatis di-render ulang untuk mencerminkan kondisi aplikasi saat ini (loading, error, atau sukses).

																																																				
