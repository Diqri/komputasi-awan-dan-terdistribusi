# Jurnal Proses — Tugas 2

## Diqri 24 September 2026
- Opsi arsitektur yang dipertimbangkan: SOA & Pub-Sub
- Alasan memilih kombinasi SOA & Pub-Sub: Kombinasi ini memecah sistem monolitik yang tightly coupled menjadi layanan independen (decoupled). Jika modul kurir mengalami gangguan jaringan atau lonjakan antrean, maka modul pembayaran dan pemesanan pelanggan di FoodGo tetap dapat berjalan normal tanpa ikut lumpuh/macet.
1. Fungsi SOA: Pemisahan Modul secara jelas, di tugas 1 sistem FoodGo bersifat monolitik sehingga jika modul pembayaran error atau kurir overload, seluruh aplikasi ikut down. Dengan SOA, setiap modul (Pesanan, Pembayaran, Resto, Kurir) dipecah menjadi layanan terpisah yang memiliki tanggung jawab mandiri (service-oriented).
2. Fungsi Pub-Sub: Sangat efektif untuk proses notifikasi dan distribusi tugas ke banyak pihak sekaligus tanpa membuat sistem utama menunggu (blocking).
Contoh: Begitu pembayaran sukses, modul Pembayaran cukup menerbitkan sebuah peristiwa (event) bernama ORDER_PAID ke message broker (Pub-Sub). Secara bersamaan dan otomatis, modul Kurir (untuk mencari driver terdekat) dan modul Katalog Resto (untuk mencetak tiket pesanan di dapur) akan merespons event tersebut secara asinkron, tanpa mengganggu proses utama pelanggan.

- Interaksi antar komponen:
1. API Gateway: Berfungsi sebagai pintu gerbang tunggal (single entry point) yang menerima seluruh permintaan masuk dari aplikasi klien (web/mobile). Secara sederhananya yaitu sebagai penghubung antar aplikasi/web.
2. Modul Pesanan (Order Service): Mengelola keranjang belanja, validasi item, dan status pembuatan pesanan.
3. Modul Pembayaran (Payment Service): Memproses transaksi keuangan dan berkoordinasi secara sinkron dengan Modul Pesanan.
4. Message Broker (Pub-Sub Broker): Infrastruktur perantara (seperti RabbitMQ atau Apache Kafka) untuk menyebarkan informasi secara asinkron.
5. Modul Katalog Resto (Restaurant Service): Mengelola menu, status operasional toko, dan penerimaan pesanan di dapur.
6. Modul Kurir / Notifikasi (Courier Service): mengelola daftar driver & panggilan dari pihak restaurant untuk menemukan driver terdekat guna mengambil & mengantar makanan ke customer.
- Diagram: ![alt text](<Diagram Tugas 2.jpg>)
- Analisis:
1. Alasan Arsitektur Ini Mengatasi Masalah Coupling dari Tugas 1 : Pada Tugas 1, FoodGo menggunakan sistem monolitik di mana seluruh modul berada dalam satu proses; jika modul kurir mengalami lonjakan trafik atau error, seluruh aplikasi ikut down. Dengan arsitektur SOA + Pub-Sub, modul-modul ini terdecoupling secara fisik dan logis. Modul pembayaran dan pemesanan tetap dapat berjalan lancar meskipun modul kurir mengalami kendala jaringan sementara.
2. Analisis Trade-off (Konsekuensi Negatif & Kompleksitas):
Sulit Dilacak (Complex Debugging), Karena alurnya berjalan secara asinkron (tidak lurus/linear), mencari letak bug atau error saat sistem gagal akan lebih rumit dibanding sistem monolitik.
Adanya Pengelolaan Tambahan membuat Kita harus mengelola pelayan pesan (message broker) tambahan agar tidak ada data pengumuman yang terlewat jika salah satu modul mendadak mati.
- Revisi diagram (versi 1 → versi 2, apa yang berubah dan kenapa): -
- Revisi Jawaban: -

## [Nama-Tanggal]

## [Nama-Tanggal]





## Log Penggunaan AI (Level 2)

> Wajib diisi sesuai kebijakan Level 2 di [`../RUBRIK-UMUM.md`](../RUBRIK-UMUM.md). Tulis "Tidak memakai AI" pada baris pertama jika memang tidak dipakai. Hanya untuk brainstorming ide/outline — bukan untuk kode/analisis/teks akhir.

| Tanggal | Tool AI | Prompt yang diberikan | Ringkasan saran/ide AI | Bagaimana diolah jadi tulisan/kode sendiri |
|---|---|---|---|---|
| 24 September 2026 | Gemini | berikan jawaban secara lengkap dan jelas sesuai perintah tugasnya | Gaya arsitektur yang dipilih untuk FoodGo adalah Kombinasi Hybrid: Service-Oriented Architecture (SOA) dan Publish-Subscribe (Pub-Sub). Justifikasi Kombinasi:SOA digunakan untuk layanan inti transaksional (Modul Pesanan dan Modul Pembayaran) yang membutuhkan komunikasi langsung yang konsisten, aman, dan real-time berbasis request-response (sinkron). Publish-Subscribe (Pub-Sub) digunakan untuk proses yang sifatnya menyebar dan tidak boleh menghambat proses utama, seperti pengiriman notifikasi ke kurir dan pencetakan tiket pesanan di dapur resto secara asinkron.   Kombinasi ini dipilih karena mampu menghilangkan sifat monolithic tight coupling dari Tugas 1, di mana modul-modul kini berdiri sendiri sehingga kegagalan di satu modul tidak akan melumpuhkan seluruh sistem.| Alasan memilih kombinasi SOA & Pub-Sub: Kombinasi ini memecah sistem monolitik yang tightly coupled menjadi layanan independen (decoupled). Jika modul kurir mengalami gangguan jaringan atau lonjakan antrean, maka modul pembayaran dan pemesanan pelanggan di FoodGo tetap dapat berjalan normal tanpa ikut lumpuh/macet. 1. Fungsi SOA:Pemisahan Modul secara jelas, di Tugas 1 sistem FoodGo bersifat monolitik sehingga jika modul pembayaran error atau kurir overload, seluruh aplikasi ikut down. Dengan SOA, setiap modul (Pesanan, Pembayaran, Resto, Kurir) dipecah menjadi layanan terpisah yang memiliki tanggung jawab mandiri (service-oriented). 2. Fungsi Pub-Sub: Sangat efektif untuk proses notifikasi dan distribusi tugas ke banyak pihak sekaligus tanpa membuat sistem utama menunggu (blocking). Contoh: Begitu pembayaran sukses, modul Pembayaran cukup menerbitkan sebuah peristiwa (event) bernama ORDER_PAID ke message broker (Pub-Sub). Secara bersamaan dan otomatis, modul Kurir (untuk mencari driver terdekat) dan modul Katalog Resto (untuk mencetak tiket pesanan di dapur) akan merespons event tersebut secara asinkron, tanpa mengganggu proses utama pelanggan.|
|24 September 2026| Gemini | berikan jawaban secara lengkap dan jelas sesuai perintah tugasnya | Bagaimana Arsitektur Ini Mengatasi Masalah Coupling dari Tugas 1:Pada Tugas 1, FoodGo menggunakan sistem monolitik di mana seluruh modul berada dalam satu proses; jika modul kurir mengalami lonjakan trafik atau error, seluruh aplikasi ikut down. Dengan arsitektur SOA + Pub-Sub, modul-modul ini terdecoupling secara fisik dan logis. Modul pembayaran dan pemesanan tetap dapat berjalan lancar meskipun modul kurir mengalami kendala jaringan sementara.   Analisis Trade-off (Konsekuensi Negatif & Kompleksitas):Kompleksitas Debugging: Karena komunikasi berbasis Pub-Sub berjalan secara asinkron dan tidak linear, pelacakan bug (tracing) menjadi lebih rumit ketika terjadi kegagalan di tengah jalan.   Konsistensi Data (Eventual Consistency): Membutuhkan mekanisme penanganan tambahan (seperti retry mechanism atau dead-letter queue) apabila salah satu subscriber (misalnya server kurir mati sesaat) gagal menerima event dari message| Analisis: 1. Alasan Arsitektur Ini Mengatasi Masalah Coupling dari Tugas 1 : Pada Tugas 1, FoodGo menggunakan sistem monolitik di mana seluruh modul berada dalam satu proses; jika modul kurir mengalami lonjakan trafik atau error, seluruh aplikasi ikut down. Dengan arsitektur SOA + Pub-Sub, modul-modul ini terdecoupling secara fisik dan logis. Modul pembayaran dan pemesanan tetap dapat berjalan lancar meskipun modul kurir mengalami kendala jaringan sementara. 2. Analisis Trade-off (Konsekuensi Negatif & Kompleksitas): Sulit Dilacak (Complex Debugging), Karena alurnya berjalan secara asinkron (tidak lurus/linear), mencari letak bug atau error saat sistem gagal akan lebih rumit dibanding sistem monolitik. Adanya Pengelolaan Tambahan membuat Kita harus mengelola pelayan pesan (message broker) tambahan agar tidak ada data pengumuman yang terlewat jika salah satu modul mendadak mati.|
