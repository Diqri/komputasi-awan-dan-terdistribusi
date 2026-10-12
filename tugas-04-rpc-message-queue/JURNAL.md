# Jurnal Proses — Tugas 4

## Jalur yang dipilih
- saya mengerjakan jalur A (RPC) untuk mensimulasikan komunikasi sinkron antara modul pesanan dan modul pembayaran. Operasi seperti pengecekan saldo dan pemotongan saldo memerlukan respons instan (blocking / request-response) agar transaksi dapat dipastikan valid seketika sebelum pesanan dilanjutkan. (Vaylan)

## Kendala teknis
- Pada saat running jalur A (RPC) tidak ada kendala sama sekali

## Uji "pesan tidak hilang" (khusus Jalur B)
- Langkah uji: matikan consumer → jalankan publisher → nyalakan consumer
- Hasil yang diamati: *Bagian ini dikerjakan dan didokumentasikan oleh anggota kelompok yang mengambil Jalur B.*

## Log Penggunaan AI (Level 2)

> Wajib diisi sesuai kebijakan Level 2 di [`../RUBRIK-UMUM.md`](../RUBRIK-UMUM.md). Tulis "Tidak memakai AI" pada baris pertama jika memang tidak dipakai. Hanya untuk brainstorming ide/outline — bukan untuk kode/analisis/teks akhir.

| Tanggal | Tool AI | Prompt yang diberikan | Ringkasan saran/ide AI | Bagaimana diolah jadi tulisan/kode sendiri |
|---|---|---|---|---|
| 12 Oktober 2026 | Gemini | jelaskan aku mengenai RPC pada modul berikut dan berikan aku alur pengerjaan untuk jalur A dan apakah bisa mengerjakan RPC tanpa library eksternal untuk modul pembayaran dan pesanan?| AI memberikan kerangka kode implementasi SimpleXMLRPCServer untuk mendaftarkan fungsi cek_saldo serta proses_pembayaran di server, dan penggunaan xmlrpc.client.ServerProxy di sisi client. | Saya memodifikasi logika validasi saldo di server.py agar mengembalikan dictionary status transaksi, lalu mengintegrasikannya ke client.py untuk menguji waktu respons sinkron. |