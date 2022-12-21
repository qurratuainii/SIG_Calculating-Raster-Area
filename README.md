# SIG_Calculating-Raster-Area

*Tutorial Calculating Raster Area

1. Download terlebih dahulu file WDPA_WDOECM_Apr2022_Public_10744_shp_0.zip, terrascope_download_20220422_114733.zip dan ESAWorldCover_ColorLegend.qml

2. Buka file yang diunduh, browser berisi file WDPA_WDOECM_Apr2022_Publicc_10744_shp-polygons.shp dan seret ke kanvas (*Gambar 1*)

3. Sekarang cari petak raster penutup dunia ESA_WorldCover_10m_2020_v100_N24_E093_Map.tif dan letakkan di kanvas QGIS (*Gambar 2*)

4. Kita akan memiliki layer vektor batas dan raster utupan lahan dimuat di panel Layers. Mari potong raster tutupan lahan ke batas taman nasional. Pilih processing lalu Toolbox. Cari dan temukan GDAL lalu Raster extraction lalu clip raster by mask layer. Klik dua kali untuk menampilkannya (*Gambar 3*)

5. Pada Clip raster by mask layer, pilih layer ESA_WorldCover_10m_2020_v100_N24_E093_Map sebagai input layer dan layer WDPA_WDOECM_Apr2022_Publicc_10744_shp-polygons sebagai layer mask. Masukkan -9999 di tetapkan nilai nodata yang ditentukan ke bagian pita keluaran (*Gambar 4*)

6. Sekarang buka bagian Advanced parameters dan pilih High Compression in Profile. Sekarang di bawah Clipped, klik ... dan pilih save To File ... Masukkan nama dile sebagai worldcover_cliped.tif. Klik Run (*Gambar 5*)

7. Sekarang layer worldcover_cliped akan dimat di kanvas QGIS. Klik kanan layer ESA_WorldCover_10m_2020_v100_N24_E093_Map dan pilih Remove Layer (*Gambar 6*)

8. Kedua layer dalam Geographic CRS EPSG:4326. CRS ini memiliki satuan derajat dan tidak cocok untuk menghitung luas. Pertama harus di memproyeksikan ulang layer ke Projected CRS untuk analisis regional seperti ini, UTM adalah pilihan yang baik untuk proyeksi CRS. Kita akan memproyeksikan ulang layer ke CRS untuk zona UTM lokal. Buka Processing toolbox pilih Vector general lalu reprojected layer. Klik dua kali untuk menampilkannya (*Gambar 7*)

9. Pada Reprojected layer pilih WDPA_WDOECM_Apr2022_Publicc_10744_shp-polygons sebagai input layer, klik tombol Select CRS dibawah target CRS (*Gambar 8*)

10. Area minat kita termasuk dalam Zona UTM 46N. Cari 46N dan pilih zona WGS 84 / UTM 46N CRS (*Gambar 9*)

11. Pada bagian reprojected section klik ... dan pilih Save To File, masukkan dengan nama nationalpark_reprojected.gpkg. lalu klik Run (*Gambar 10*)

12. Sekarang layer nationalpark_reprpojected akan dimuat di kanvas. Klik kanan layer WDPA_WDOECM_Apr2022_Publicc_10744_shp-polygons dan pilih Remove layer untuk menghapusnya. Sekarang kita akan memproyeksikan ulang layer raster. Pada processing toolbox cari dan temukan GDAL lalu raster projetions lalu Warp (reporect) (*Gambar 11*)

13. PAda Wrap(reproject) pilih worldcover_cliped sebagai input layer, pilih WGS 84 / UTM zone 46N CRS di target CRS. Buka parameter lanjutan dan pilih High Kompresi pada profile (*Gambar 12*)

14. sekarang di bawah reprojetced klik ... dan pilih Save To File dan masukkan nama sebagai worldcover_reprojected.tif. Klik Run (*Gambar 13*)

15. Sekarang layer worldcover_reprojected akan dimuat di kanvas, hapus layer worldcover_cliped. Mari atur layer proyek ke zona UTM. Klik pada layer mana saja dan pilih layer CCRS lalu Set project CRS from layer (*Gambar 14*)

16. Sekarang proyek CRS akan diperbarui. Mari atur simbologi lapisan raster sesuai nama kelas dan warna kumpulan data ESA WorldCover. Klik kanan pada layer worldcover_reprojected dan klik Properties (*Gambar 15*)

17. Pada layer properties, pilih symbology. Kita dapat melihat warna layer divisualisasikan dalam nada putih-hitam. Untuk memperbaikinya, klik style lalu Load style. terlusuri dan pilih file ESAWorldCOver_ColorLegend.qml (*Gambar 16*)

18. Sekarang kita dapat melihat warna simbol yang diperbarui dan deskripsi class. Klik OK (*Gambar 17*)

19. Perluas layer workdcover_reprojected di panel Layers untuk melihat legenda dengan deskripsi kelas yang benar (*Gambar 18*)

20. Sekarang mari kita hitung luas untuk setiap kelas. Di kotak alat pemrosesan, cari dan temukan alat laporan nilai unik lapisan Raster. Klik dua kali untuk membukanya (*Gambar 19*)

21. Dalam dialog Raster Layer Unique Values Report, pilih layer Input sebagai worldcover_reprojected. Di bawah tabel Unique values klik ... dan pilih Save to File…. Masukkan nama sebagai class_areas.gpkg. Klik Jalankan (*Gambar 20*)

22. Sekarang layer class_areas akan ditambahkan ke panel Layers. Klik kanan pada layer dan klik Open Attribute Table. Kolom m2 memuat luas untuk setiap kelas dalam meter persegi (*Gambar 21*)

23. Mari ubah luas menjadi kilometer persegi. Di Processing Toolbox, cari dan pilih tabel Vektor Kalkulator Bidang (*Gambar 22*)

24. Pada Field calculator pilih layer class_areas di Layer Input. Masukkan nama Bidang sebagai area_sqkm. Di bidang Hasil, pilih Float. Di jendela Ekspresi, masukkan ekspresi di bawah ini. Ini akan mengubah sqmt menjadi sqkm dan membulatkan hasilnya menjadi 2 tempat desimal. Di bawah Terhitung klik pada ... dan pilih Save To File… . Masukkan nama sebagai class_area_sqkm.gpkg. Klik Run (*Gambar 23*)

25. Sekarang lapisan class_area_sqkm akan dimuat di kanvas. Buka tabel Atribut dan periksa kolom area_sqkm yang baru ditambahkan. Kita akan melihat bahwa kolom Nilai berisi angka untuk setiap kelas. Agar hasilnya lebih mudah diinterpretasikan, Mari tambahkan juga deskripsi untuk setiap nomor kelas. Deskripsi kelas tersedia di Panduan Pengguna Produk ESA (*Gambar 24*)

26. Buka Field Calculator, dan pilih layer class_areas_sqkm di Input Layer. Masukkan Nama bidang sebagai tutupan lahan, pada jenis bidang Hasil, pilih String. Di jendela Expression masukkan ekspresi di bawah ini. Ekspresi ini menggunakan pernyataan CASE untuk menetapkan nilai berdasarkan beberapa kondisi. Di bawah Terhitung klik pada ... dan pilih Simpan Ke File… . Masukkan nama sebagai class_area_with_landcover.gpkg. Klik Run (*Gambar 25*)

27. Sekarang lapisan class_area_with_landcover akan dimuat di kanvas. Buka tabel Atribut. Kolom tutupan lahan akan berisi nama tutupan lahan terhadap setiap nilai tutupan lahan (*Gambar 26*)

28. Mari ekspor hasil ini sebagai file excel. Sebelum mengekspor, kita akan mengatur tabel dan menghapus kolom yang tidak diinginkan. Di Processing Toolbox, cari dan pilih Vector table lalu pilih  Refactor field (*Gambar 27*)

29. Dalam dialog Refactor Fields, pilih layer class_area_with_landcover di Layer Input. Pilih semua kolom kecuali area_sqkm dan tutupan lahan, lalu klik Hapus kolom yang dipilih (*Gambar 28*)

30. Kita juga dapat mengubah urutan bidang dalam tabel menggunakan tombol Pindahkan Bidang yang Dipilih. Setelah  selesai mengedit, klik tombol ... di sebelah Refactored dan pilih Save To File…. Pilih File XLSX (*.xlsx) sebagai formatnya. Masukkan nama file sebagai park_area_by_landcover.xlsx dan klik Save. Dalam dialog Bidang Refaktor, klik Jalankan untuk menerapkan perubahan (*Gambar 29*)

31. Hasilnya akan berupa spreadsheet dengan kolom landcover dan area_sqkm (*Gambar 30*)
