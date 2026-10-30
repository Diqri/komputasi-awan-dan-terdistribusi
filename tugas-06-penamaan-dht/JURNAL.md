# Jurnal Proses — Tugas 6

## Implementasi find_successor
- Kesalahan pertama yang ditemui (mis. lupa wrap-around di ring): Sempat lupa menangani kondisi wrap around ring apabila nilai `key_hash` lebih besar daripada seluruh ID node yang terdaftar di dalam ring
- Cara memperbaikinya: dengan menambahkan mekanisme fallback yg mengembalikan elemen pertama dari daftar ID node yang sudah diurutkan (`sorted_node_ids[0]`) jika perulangan penelusuran node tidak menemukan ID yang memenuhi syarat $\get key\_hash$

## Skenario join/leave
- Hasil sebelum & sesudah node baru bergabung: Sebelum `node-medan-1` (id=123) bergabung, `kurir-001` (hash=50) dikelola oleh Node ID 136,setelah `node-medan-1` bergabung, sistem menjalankan proses migrasi data secara otomatis untuk `kurir-001` dan `kurir-002` dari successor lama ke node baru (id=123) karena posisinya kini menjadi penanggung jawab data tersebutd
- Hasil sebelum & sesudah node keluar: Sebelum `node-jakarta-1` (id=177) keluar, lookup data `kurir-001` dan `kurir-002` mengembalikan alamat IP dengan benar, setelah node tersebut keluar (`[LEAVE]`), hasil lookup tetap konsisten dan sukses mengembalikan IP kurir karena struktur ring dan pemetaan data tetap dipertahankan oleh node-node aktif yang tersisa

## Log Penggunaan AI (Level 2)

> Wajib diisi sesuai kebijakan Level 2 di [`../RUBRIK-UMUM.md`](../RUBRIK-UMUM.md). Tulis "Tidak memakai AI" pada baris pertama jika memang tidak dipakai. Hanya untuk brainstorming ide/outline — bukan untuk kode/analisis/teks akhir.

| Tanggal | Tool AI | Prompt yang diberikan | Ringkasan saran/ide AI | Bagaimana diolah jadi tulisan/kode sendiri |
|---|---|---|---|---|
| 16/10/2026 | Gemini | aku memiliki tugas komputasi awan terdistribusi dengan 3 anggota kelompok, bantu aku membagi jobdesk dan berikan penjelasan tugasnya | Memberikan rancangan lengkap logika fungsi `find_successor`, `join`, `leave`, `register_kurir`, dan `lookup` pada Chord DHT dan memberikan pembagian jobdesk anggota 2 dan 3| Kode dimasukkan ke dalam file `chord_simulation.py` dan running melalui terminal hingga menghasilkan output migrasi data yang sesuai dengan file 'output week6.png'|