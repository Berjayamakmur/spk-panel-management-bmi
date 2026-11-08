# SPK Panel Management System V2.1 — Multi-item Panel

## Perubahan
- Satu SPK hanya mempunyai satu pelanggan dan satu project.
- Satu SPK dapat mempunyai 1–100 item panel.
- Setiap item mempunyai nama_panel, jumlah_unit, spesifikasi.
- Jumlah unit SPK adalah penjumlahan seluruh item panel.
- PDF SPK mencetak setiap panel beserta spesifikasi masing-masing.
- Fitur V2 lain (login, dashboard, foto, laporan, status, log) tetap tersedia.

## Instalasi
1. Backup spreadsheet dan project Apps Script V2.
2. Ganti Code.gs dan Index.html dengan versi V2.1 ini.
3. Jalankan `setupDatabase()` pada project spreadsheet yang sama; sistem membuat sheet `SPKItems` dengan kolom id, spk_id, urutan, nama_panel, jumlah_unit, spesifikasi, created_at, updated_at.
4. Data SPK lama tetap dapat dibuka: jika belum memiliki baris SPKItems, detail menggunakan jenis_panel/jumlah_unit/keterangan lama sebagai satu item sementara. Simpan ulang SPK tersebut untuk membuat item terpisah.
5. Deploy > Manage deployments > Edit > New version > Deploy.
6. Uji membuat satu SPK dengan 3 panel, jumlah unit berbeda, dan spesifikasi berbeda. Periksa total unit, detail, PDF, dan edit.

## Catatan operasional
- Perubahan beberapa sheet di Google Sheets bukan transaksi atomik. Backup sebelum migrasi dan hindari edit bersamaan selama migrasi.
- Item panel dihapus dan ditulis ulang saat SPK diedit; ID item dapat berubah. Integrasi yang bergantung pada ID item perlu disesuaikan.
- `jenis_panel` pada sheet SPK kini hanya ringkasan nama panel untuk kompatibilitas laporan V2; sumber detail adalah SPKItems.
- Foto produksi masih ditautkan ke SPK, bukan per item panel.
- Untuk proyek yang memerlukan audit kuat, pertimbangkan database transaksional dan penyimpanan histori item.


## Logo perusahaan
Logo resmi PT Berjaya Makmur Indonesia sudah ditanam langsung (embedded) pada `Index.html` untuk halaman login/sidebar dan pada `Code.gs` untuk PDF SPK serta laporan produksi. Tidak perlu mengunggah file logo terpisah ke Google Drive. Jika logo diganti, ganti data URI `data:image/jpeg;base64,...` pada kedua file tersebut, lalu deploy versi baru.


## V2.2 — Field Lokasi Proyek
- Form Tambah/Edit SPK memiliki field wajib `Lokasi Proyek` (maks. 250 karakter).
- Lokasi tampil pada detail SPK, tabel daftar SPK, PDF SPK, dan laporan PDF/CSV.
- Kolom `lokasi_proyek` ditambahkan di akhir sheet `SPK` agar posisi kolom V2.1 yang lama tidak bergeser.
- Jalankan `setupDatabase()` sekali setelah mengganti source code. Fungsi ini menambahkan header baru di ujung sheet SPK jika belum tersedia; data lama tetap berada pada kolom sebelumnya.
- Untuk SPK lama, isi lokasi proyek melalui menu Edit SPK sebelum menyimpan kembali.


## Perbaikan patch V2.2.1
Perbaikan sintaks pada validasi `lokasi_proyek` (pemisah baris yang sebelumnya tertulis sebagai karakter literal `\n`). Gunakan paket ZIP terbaru ini; pemeriksaan sintaks JavaScript `Code.gs` dengan Node.js telah lulus.
