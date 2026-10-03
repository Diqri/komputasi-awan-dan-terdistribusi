# Jurnal Proses — Tugas 3

## Percobaan tanpa Lock
- Hasil `processed_count` yang didapat: 26
- Kenapa bisa meleset (jelaskan mekanisme race condition dengan kata sendiri): Hal ini terjadi karena proses penambahan nilai dipecah ke dalam tahapan membaca nilai (current_value = processed_count), lalu menghitungnya (current_value + 1), dan menulisnya kembali. Ketika banyak thread berjalan bersamaan tanpa lock, dua atau lebih thread bisa membaca current_value yang sama persis di waktu yang nyaris bersamaan (misalnya sama-sama membaca angka 10). Akibatnya, saat mereka masing-masing menghitung current_value + 1 (menjadi 11) dan menulisnya kembali ke processed_count, penambahan dari thread sebelumnya tertimpa (overwritten) oleh thread setelahnya. Seharusnya nilai bertambah dua kali, tapi karena nilainya sama, penambahan itu dihitung sebagai satu kali. Inilah penyebab banyak pesanan yang terlewat sehingga total akhirnya di bawah 100.

## Percobaan dengan Lock
- Hasil `processed_count` setelah perbaikan: 100

## Kendala Docker
- Error yang ditemui saat `docker build`/`docker run` dan cara memperbaikinya: ...

## Log Penggunaan AI (Level 2)

> Wajib diisi sesuai kebijakan Level 2 di [`../RUBRIK-UMUM.md`](../RUBRIK-UMUM.md). Tulis "Tidak memakai AI" pada baris pertama jika memang tidak dipakai. Hanya untuk brainstorming ide/outline — bukan untuk kode/analisis/teks akhir.

| Tanggal | Tool AI | Prompt yang diberikan | Ringkasan saran/ide AI | Bagaimana diolah jadi tulisan/kode sendiri |
|---|---|---|---|---|
| 03 Oktober 2026 | Gemini | mengapa perintah with lock dan tanpa with lock hasilnya sama aja yaitu 100, apakah ada kesalahan | Hal tersebut bisa terjadi karena meskipun jumlah worker diubah menjadi 100, operasi penambahan sederhana seperti processed_count += 1 di dalam interpreter Python dieksekusi sangat cepat. Selain itu, jeda time.sleep pada kode sebelumnya diletakkan sebelum proses penambahan, sehingga saat tahap penambahan angka dilakukan, thread sering kali sudah kadung mengeksekusinya secara berurutan tanpa sempat disela oleh thread lain.Agar race condition benar-benar "jebol" dan menghasilkan angka di bawah 100 (misalnya 40 atau 70) saat tanpa lock, kita perlu memecah proses increment menjadi dua langkah (baca $\rightarrow$ jeda/sela $\rightarrow$ tulis ulang) agar ada celah bagi thread lain untuk menimpa data. Ubah bagian fungsi process_order khusus untuk pengujian tanpa Lock menjadi seperti ini: def process_order(order_id: int) -> None: """Proses satu pesanan. Dipanggil oleh tiap thread pekerja.""" global processed_count # Simulasikan kerja nyata (mis. validasi, hitung total harga) time.sleep(random.uniform(0.001, 0.01)) # TODO 2: Increment TANPA Lock (pastikan lurus/sejajar tanpa tab berlebih) processed_count += 1 | Mengganti fungsi def process_order menjadi def process_order(order_id: int) -> None: """Proses satu pesanan. Dipanggil oleh tiap thread pekerja.""" global processed_count # Simulasi kerja awal time.sleep(random.uniform(0.001, 0.005)) # TODO 2: Menambahkan increment `processed_count`. with lock: current_value = processed_count     # 1. Membaca nilai saat ini time.sleep(0.0001)                  # 2. Jeda mikro yang memicu bentrok antar thread processed_count = current_value + 1 # 3. Menulis kembali nilai baru (rawan tertimpa)|