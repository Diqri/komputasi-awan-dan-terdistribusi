# Jurnal Proses — Tugas 1

> Isi jurnal ini selama proses diskusi berlangsung, bukan ditulis ulang rapi di akhir. Tulis dengan gaya bebas — poin diskusi, kebuntuan, perubahan pikiran.

## [22 september 2026 diskusi 1]
- Peserta: Ahmad Diqri
- Poin diskusi: Aku milih the network is reliable, kalian milih salah satu/bisa lebih dari 8 case pitfall rek.
- Perbedaan pendapat (jika ada): ...

## [22 Septemper 2026 diskusi 2]
- Peserta: Vaylan Christopher
- Point diskusi: Oke diq, aku ambil pitfall 2 aja yg latency is zero 

## Review Silang
- [Nama] mengomentari analisis [Nama lain]: ...

## Log Penggunaan AI (Level 2)

> Wajib diisi sesuai kebijakan Level 2 di [`../RUBRIK-UMUM.md`](../RUBRIK-UMUM.md). Tulis "Tidak memakai AI" pada baris pertama jika memang tidak dipakai. Hanya untuk brainstorming ide/outline — bukan untuk kode/analisis/teks akhir.

| Tanggal | Tool AI | Prompt yang diberikan | Ringkasan saran/ide AI | Bagaimana diolah jadi tulisan/kode sendiri |
|---|---|---|---|---|
| 22 September 2026 | Gemini | saya tertarik pada the network is reliable, berikan saya penjelasannya secara sederhana  | "jaringan selalu tersedia" dalam kenyataannya, jaringan komputer di dunia nyata itu berisik, tidak stabil, dan penuh kejutan. Kabel fiber optik bisa terputus, sinyal Wi-Fi/seluler bisa hilang sesaat, router bisa overheat, atau server pihak ketiga yang diajak bicara tiba-tiba down. | jika terjadi suatu case/masalah yang di mana server atau koneksi dari pihak ke tiga (bank) mengalami gangguan saat proses pembayaran, maka hal ini bisa menjadi fatal. |
|22 September 2026 | Gemini | jelaskan dan jawab maksud dari tugas ini | 1. Kutipan/Paraphrase Skenario: Modul pemesanan memanggil payment gateway pihak ketiga secara synchronous tanpa mekanisme pembatasan waktu tunggu (timeout) atau fallback. Saat payment gateway down atau koneksi terputus, transaksi gagal secara hening atau terus menggantung. 2. Mengapa Asumsi Ini Keliru: Di dunia nyata, jaringan internet publik mengalami packet loss, fluktuasi rute (BGP route flap), pemadaman kabel optik, atau kegagalan router perantara. Komunikasi antar-node dalam jaringan tidak pernah 100% andal. 3. Dampak Konkret ke FoodGo: Ketika server payment gateway mengalami penurunan performa, request dari FoodGo tertahan tanpa henti. Karena thread koneksi terus menunggu, connection pool dan thread web server FoodGo terkuras habis. Akibatnya, calon pelanggan lain bahkan tidak bisa sekadar membuka menu makanan.| 1. Melakukan Pembayaran order makanan non-tunai dari customer kepada pihak FoodGo melalui pihak ke 3 (bank), tanpa adanya mekanisme batas waktu tunggu (timeout), penanganan koneksi putus, maupun pembatasan pemanggilan berulang. 2. Karena, jika terjadi suatu case/masalah yang di mana server atau koneksi dari pihak ke tiga (bank) mengalami gangguan saat proses pembayaran, maka hal ini bisa menjadi fatal. 3. 1. Server FoodGo tidak tahu harus berbuat apa karena tidak adanya batas waktu tunggu (timeout). 2. Aplikasi FoodGo tetap menahan koneksi (hang/freeze) sambil menunggu jawaban Bank. 3. 1 customer menunggu, 2 customer menunggu, lama-lama ratusan customer menunggu proses pembayaran. Hasilnya: Server FoodGo kehabisan kapasitas (thread exhaustion) hanya untuk diam menunggu. Akhirnya aplikasi FoodGo mati total (crash), bahkan orang lain yang sedang melihat menu, jadi tidak bisa menggunakan aplikasi.|
|22 September 2026| Gemini | aku memiliki tugas yang berkaitann dengan komputasi awan dan terdistibusi, contoh skenario (ss studi kasus FoodGo) dengan menggunakan pitfall latency is zero, berikan aku penjelasan mengenai tugas tersebut dan alur pengerjaan untuk kasus FoodGo ini|1.  tidak ada timeout sama sekali pada pemanggilan antar service (modul pesanan memanggil modul pembayaran dan menunggu tanpa batas waktu). 2. *Kenapa ini keliru:* banyak orang mengira suatu jaringan latensi itu nol atau orang bisa mengirim data tanpa loading sama sekali. 3. *Dampak ke FoodGo:* socket tcp ke sistem pembayaran habis dikarenakan koneksi yang lambat karean tidak pernah ditutup atau dilepas kembali ke pool. 4. *Solusi desain awal:* yaitu dengan menghilangkan ketergantungan blocking synchronous antar-service. 5. *Trade-off:* alur transaksi tidak lagi linier. pengguna tidak langsung mendapat konfirmasi sukses seketika.|1. **Bukti di skenario:** tidak ada timeout sama sekali pada pemanggilan antar service (modul pesanan memanggil modul pembayaran dan menunggu tanpa batas waktu). 2. **Kenapa ini keliru:** 
banyak orang yang mengira suatu jaringan dengan latensi nol orang bisa mengirim data tanpa loading sama sekali. 3. **Dampak ke FoodGo:**
terjadi kehabisan ketersediaan port TCP pada sistem pembayaran. hal ini disebabkan oleh tumpukan koneksi yang sangat lambat yang tidak pernah ditutup secara otomatis maupun dikembalikan ke dalam connection pool untuk dapat digunakan kembali oleh proses lain. 4. **Solusi desain awal:** 
menerapkan perbaikan arsitektur dengan cara menghilangkan ketergantungan komunikasi yang bersifat mengunci dan sinkron (blocking synchronous) pada proses pemanggilan antar service. 5. **Trade-off:**
Alur pemrosesan transaksi tidak lagi berjalan secara terurut atau linier dalam satu waktu. Dampaknya, pengguna tidak bisa lagi langsung menerima status konfirmasi keberhasilan transaksi secara instan pada detik yang sama saat pemesanan dilakukan.
|