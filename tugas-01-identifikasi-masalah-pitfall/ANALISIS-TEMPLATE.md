# Tugas 1 — Analisis Pitfall FoodGo

**Kelompok:** [10]

| Nama | NIM | Kontribusi |
|---|---|---|
| Ahmad Diqri Wirayudha | 103072400084 | The Network Is Reliable|
| [nama 2] | [nim] | [pitfall/bagian yang dikerjakan] |
| [nama 3] | [nim] | [pitfall/bagian yang dikerjakan] |

## Pitfall 1: The Network Is Reliable — ditulis oleh Ahmad Diqri Wirayudha

**Bukti di skenario:** Melakukan Pembayaran order makanan non-tunai dari customer kepada pihak FoodGo melalui pihak ke 3 (bank), tanpa adanya mekanisme batas waktu tunggu (timeout), penanganan koneksi putus, maupun pembatasan pemanggilan berulang.

**Kenapa ini keliru:** 
Karena, jika terjadi suatu case/masalah yang di mana server atau koneksi dari pihak ke tiga (bank) mengalami gangguan saat proses pembayaran, maka hal ini bisa menjadi fatal.

**Dampak ke FoodGo:**
1. Server FoodGo tidak tahu harus berbuat apa karena tidak adanya batas waktu tunggu (timeout).
2. Aplikasi FoodGo tetap menahan koneksi (hang/freeze) sambil menunggu jawaban Bank.
3. 1 customer menunggu, 2 customer menunggu, lama-lama ratusan customer menunggu proses pembayaran.

Hasilnya: Server FoodGo kehabisan kapasitas (thread exhaustion) hanya untuk diam menunggu. Akhirnya aplikasi FoodGo mati total (crash), bahkan orang lain yang sedang melihat menu, jadi tidak bisa menggunakan aplikasi.

**Solusi desain awal:** 
1. Pasang Timeout (Batas waktu tunggu):
Memberi aturan: "Kalau dalam 5 detik Bank tidak membalas, anggap koneksi putus dan lepaskan antrean server."

2. Circuit Breaker (Sekring Listrik):
Mirip saklar MCB listrik di rumah. Semisal jika dalam 5 menit ada 100 transaksi ke Bank yang gagal terus-menerus, maka putus sementara jalurnya (open circuit). Beri tahu user: "Sistem pembayaran sedang gangguan, silakan gunakan metode lain" tanpa perlu membebani server FoodGo untuk mencoba koneksi yang jelas-jelas sedang rusak.

3. Idempotency (Cegah Bayar Double):
Saat jaringan putus dan sistem mencoba kirim ulang (retry), pastikan ada nomor unik transaksi agar saldo pelanggan tidak terpotong dua kali.

**Trade-off:**
1. Karena adanya fitur timeout dan circuit breaker, potensi rugi dari kedua pihak menaik : Transaksi yang sebenarnya mungkin bisa berhasil jika "ditunggu sedikit lebih lama" terpaksa digugurkan demi menyelamatkan kestabilan server

2. Risiko Serangan Balik (Self-Inflicted DDoS): Jika ratusan pelanggan checkout bersamaan dan jaringannya sedang goyang, fitur retry akan menyerang server pembayaran secara berkali-kali lipat (retry storm). koneksi yang sedang bermasalah justru bisa mati total karena kebanjiran request kirim ulang (retry) dari FoodGo sendiri.
---

## Pitfall 2: [nama pitfall] — ditulis oleh [nama]

(ulangi struktur di atas)

---

## Pitfall 3: [nama pitfall] — ditulis oleh [nama]

(ulangi struktur di atas)

---

## Kesimpulan Kelompok

[Ringkasan: jika FoodGo memperbaiki ketiga pitfall ini, apa arsitektur yang disarankan secara garis besar? Kaitkan dengan Tugas 2.]
