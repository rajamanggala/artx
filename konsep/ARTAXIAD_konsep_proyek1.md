# Konsep Proyek: Situs Resmi ARTAXIAD

Catatan ini merangkum keputusan, status, dan pertanyaan terbuka dari diskusi sebelumnya. Tempel isinya di awal chat baru supaya konteksnya langsung terbawa.

## 1. Ringkasan

- **Apa:** situs resmi band lokal bernama **ARTAXIAD**, dari daerah Jawa (Boyolali, Jawa Tengah).
- **Menu:** Home, Tour, News, Music, Media, Merch.
- **Bahasa situs:** Indonesia.
- **Pengguna admin:** orang awam yang hanya perlu mengetik dan memposting konten.
- **Referensi gaya:** linkinpark.com (konsep, bukan tiruan). Yang dipakai hanya prinsipnya: hero layar penuh, bingkai hitam di tepi layar, tab menu yang menyatu dengan bingkai, kartu kaca (glassmorphism), tombol berbentuk pil. Logo, warna, font, efek, dan isi dibuat sendiri.
- **Aturan desain:** halaman Home adalah landing page dengan efek khusus. Halaman menu lain **sengaja didesain berbeda** dari landing page.

## 2. Keputusan yang sudah diambil

| Topik | Keputusan | Catatan |
|---|---|---|
| Teknologi landing page | HTML + CSS + JavaScript biasa | Tanpa npm, tanpa build, tanpa Node.js. Cukup upload ke hosting. |
| Backend | **Laravel** | Pilihan pengguna. Masih menunggu konfirmasi SSH dan Composer di hosting. |
| Database | MySQL | Tersedia di hampir semua paket hosting. |
| WordPress | Tidak dipakai | Dinilai pengguna terlalu tidak fleksibel. |
| Hosting | Paket **Premium**, durasi **12 bulan** | Belum dibeli. Lihat bagian 6. |
| Pratinjau lokal | XAMPP, alamat `http://localhost/artaxiad/` | Wajib lewat server, bukan klik dua kali (lihat bagian 7). |
| Merch | Hanya menampilkan produk dengan tombol beli ke marketplace atau WhatsApp | Tanpa keranjang dan pembayaran sendiri. |
| Media | Cukup tempel link YouTube | Tidak upload video ke hosting. |
| Data awal | Data dummy | Semua teks, jadwal, harga, dan judul rilisan masih contoh. |

## 3. Desain landing page (Home)

### Palet warna (hanya tiga)
- Hitam `#000000`: bingkai, tab logo, menu, footer.
- Merah `#D62420`: tombol utama, penanda menu aktif.
- Putih `#FFFFFF`: semua tulisan.

### Tipografi
- **Martian Mono** (Google Fonts) untuk semua teks. Wordmark ARTAXIAD besar dengan letter-spacing rapat.

### Tata letak
- Hero setinggi layar (`100dvh`) dengan **video latar** `bg.mp4`.
- Bingkai hitam di tepi layar dengan sudut membulat. Tab logo (kiri atas) dan tab menu (kanan atas) menyatu dengan bingkai lewat lengkung cekung. Tab footer di kiri dan kanan bawah.
- Wordmark ARTAXIAD dan tagline di tengah.
- Kartu kaca rilisan terbaru di kiri bawah (cover, judul, tombol merah "Dengarkan").
- Tombol Follow dan Subscribe di kanan bawah.
- Menu kanan atas berukuran 16px, jarak antarmenu 32px.
- **Mobile (di bawah 820px):** menu menjadi tombol "Menu" yang membuka layar penuh, Follow dan Subscribe pindah ke atas tengah, resolusi efek diturunkan.

### Efek utama: kaca berembun dengan air (dilap kursor)
Ini bagian yang paling banyak berevolusi. Urutannya:
1. Kabut bergerak (WebGL) yang tersibak mouse. Disimpan sebagai `index_kabut.html`.
2. Embun dengan tetesan bulat. Dinilai tidak realistis (terlihat seperti bola hitam). Disimpan sebagai `index_bintik.html`.
3. **Versi sekarang:** kaca berembun mengikuti foto referensi pengguna.
   - Video latar diburamkan (bokeh), sedikit lebih terang, dengan pita vertikal samar dan butiran halus.
   - **Garis air panjang** yang merayap turun, berkelok, dan meninggalkan jalur bening dengan tepi gelap tipis. Tiga lapis kolom dengan jarak berbeda.
   - **Bintik embun kecil** yang banyak, tetesan besar sengaja langka.
   - **Mouse atau jari menjadi pengelap:** area yang dilap jernih, lalu embun dan garis kembali perlahan (sekitar 2 detik tahan, total sekitar 7 detik).
   - Video dipakai sebagai tekstur WebGL supaya tetesan dan garis membiaskan gambar. Ada mode cadangan (lapisan embun tanpa bias) dan cadangan CSS bila WebGL tidak ada.

### Parameter yang bisa disetel (di dalam `index.html`)
- `float br=.017*wet*band;` : ketebalan embun (makin besar makin buram).
- `streak(q,22.,1.)`, `15.`, `11.` : kerapatan kolom garis air. `if(a>.72)` : peluang garis muncul.
- `.012+.02*s` : kecepatan garis air.
- `float w=(.0042+.0048*c);` : lebar garis air.
- `LIFE = 7000`, `HOLD = 2000` : lama embun kembali setelah dilap (milidetik).
- `brush` : ukuran lap.

## 4. Arsitektur yang direncanakan

### Frontend
- Halaman publik berupa HTML + JavaScript biasa (atau templat Blade di Laravel).
- Landing page tetap statis. Halaman lain mengambil data dari Laravel.

### Backend (Laravel + MySQL)
Entitas data yang diperlukan:

| Menu | Data |
|---|---|
| Tour | tanggal, kota, tempat, link tiket, status "tiket habis" |
| News | judul, isi, foto, status draf atau terbit, tanggal |
| Music | judul, jenis (single, EP, album), tahun, cover, link Spotify, link Apple Music |
| Media | judul, link YouTube, jenis (video klip, live, galeri) |
| Merch | nama, foto, harga, link beli |
| Pengaturan | link sosial media, teks tagline, rilisan yang ditampilkan di hero |

### Panel admin untuk orang awam (`/admin`)
- Login sederhana (email dan password).
- Satu formulir per menu, label bahasa Indonesia.
- Foto otomatis dikecilkan saat diupload.
- Tombol "Simpan sebagai draf" dan "Terbitkan", plus pratinjau.
- Daftar item dengan Edit dan Hapus (ada konfirmasi sebelum hapus).
- Keamanan dasar: password terenkripsi, CSRF, validasi file upload.

## 5. Status file

| File | Isi |
|---|---|
| `index.html` | Landing page versi sekarang (embun dan garis air). **File utama.** |
| `index_v2.html` | Versi sebelum embun garis air. Cadangan. |
| `index_bintik.html` | Versi tetesan bulat. Cadangan. |
| `index_kabut.html` | Versi kabut bergerak. Cadangan. |
| `bg.mp4` (atau `video/bg.mp4`) | Video latar milik pengguna. Kode menerima kedua jalur. |
| `img/poster.jpg` | Opsional: foto cadangan sebelum video termuat. |

Struktur folder lokal: `C:\xampp\htdocs\artaxiad\`

## 6. Hosting dan biaya

- **Belum dibeli.** Pilihan: paket **Premium**, durasi **12 bulan**. Halaman checkout yang dilihat tampak seperti **Hostinger** (hPanel).
- Perkiraan biaya Premium 12 bulan: sekitar **Rp478.800** ditambah pajak sekitar 11%, jadi sekitar **Rp531.000** sekali bayar untuk setahun.
- Perkiraan tahun kedua: harga normal sekitar **Rp84.900 per bulan** (belum dikonfirmasi).
- Paket **Unlimited** tidak perlu (total checkout yang terlihat Rp797.868 untuk 12 bulan). Tawaran 48 bulan juga tidak disarankan karena mengunci pengguna lama.
- Harga per bulan di halaman hosting adalah harga promo yang **dibayar sekaligus di muka** untuk seluruh durasi.
- Garansi uang kembali 30 hari tersedia.

### Yang harus dikonfirmasi ke chat support sebelum membayar
1. Apakah Premium sudah termasuk **akses SSH**? (Sumber yang ditemukan tidak sepakat.)
2. Apakah **Composer 2** tersedia? (Dokumentasi resmi menyebut Composer terpasang di Premium.)
3. Versi **PHP** tertinggi yang bisa dipilih (Laravel baru butuh PHP 8.2 atau 8.3 ke atas).
4. Apakah bisa menjalankan `php artisan` lewat SSH?
5. Apakah **document root** bisa diarahkan ke folder `public`? (Di Hostinger, web root biasanya tetap `public_html`, jadi struktur Laravel perlu disesuaikan: kode di luar `public_html`, isi `public` diarahkan ke sana.)

Jika SSH tidak ada di Premium, opsinya naik ke paket Business atau memakai PHP terstruktur tanpa Laravel.

## 7. Catatan teknis penting (pelajaran selama pengembangan)

- **Wajib lewat server.** Efek air memakai video sebagai tekstur WebGL. Jika halaman dibuka dengan klik dua kali (`file://`), browser memblokir video itu, sehingga embun tampil tanpa pembiasan. Lewat `http://localhost/artaxiad/` (XAMPP, nyalakan Apache) efek penuh berjalan. Di hosting juga tidak masalah.
- Jalur video: kode mencoba `video/bg.mp4` dulu, lalu `bg.mp4`.
- Efek ini cukup berat. Resolusi kanvas sudah diturunkan di HP (55%) dan desktop (75%). Jika terasa patah-patah di laptop lemah, bobot efek perlu diturunkan.
- Saat pengguna memilih mode hemat data atau kurangi gerak, video tidak dimuat dan efek tidak bergerak.
- Semua tombol dan menu masih berupa `#`. Perlu diarahkan ke halaman dan link sebenarnya.
- Hak cipta: desain, gambar, dan konten linkinpark.com tidak boleh disalin. Hanya konsep umumnya yang dipakai sebagai inspirasi.

## 8. Yang masih placeholder

- Semua teks: tagline "Musik keras dari tanah Jawa.", judul "Gerbang Senja", deskripsi rilisan.
- Jadwal tour, berita, daftar rilisan, dan harga merch pada halaman lain (contoh ada di `artaxiad.html`).
- Cover single (masih gradasi CSS dengan huruf "A").
- Logo (masih segitiga dalam lingkaran buatan sendiri).
- Link Follow, Subscribe, Spotify, Apple Music, tiket, dan toko.
- Formulir newsletter belum dibuat.

## 9. Langkah berikutnya (urutan usulan)

1. Konfirmasi paket hosting ke chat support (poin di bagian 6), lalu beli.
2. Desain halaman Tour, News, Music, Media, Merch (statis dengan data dummy dulu, berbeda dari landing page).
3. Siapkan Laravel di XAMPP: instal Composer, buat proyek, migrasi tabel, model, dan route.
4. Bangun panel admin `/admin` dengan formulir sederhana per menu.
5. Pindahkan halaman statis menjadi templat Blade yang membaca dari database.
6. Deploy ke hosting (atur struktur `public_html`, `.env`, migrasi database, `storage:link`).
7. Pasang domain dan SSL, lalu uji di HP dan berbagai browser.

## 10. Pertanyaan terbuka untuk diskusi berikutnya

- Seperti apa karakter desain halaman Tour, News, Music, Media, dan Merch yang diinginkan? Apakah ada referensi?
- Apakah domain yang diinginkan (misalnya `artaxiad.com`) sudah dicek ketersediaannya?
- Apakah logo asli band sudah ada? Foto dan cover asli?
- Apakah perlu formulir newsletter (Subscribe), dan ke mana datanya dikirim?
- Apakah perlu halaman "About" atau profil personel?
- Apakah perlu pemutar musik (embed Spotify) di halaman Music?
- Siapa yang akan mengelola konten, dan seberapa sering?

## 11. Teks pembuka untuk chat berikutnya (salin dan tempel)

> Saya sedang membuat situs resmi band ARTAXIAD dengan Laravel + MySQL, di hosting shared (kemungkinan Hostinger Premium). Landing page-nya sudah jadi: HTML statis dengan video latar dan efek kaca berembun yang bisa dilap dengan mouse (WebGL), warna #000, #D62420, putih, font Martian Mono. Menu: Home, Tour, News, Music, Media, Merch. Halaman selain Home harus berdesain berbeda dari landing page. Admin adalah orang awam, jadi butuh panel admin yang sangat mudah. Saya mau lanjut ke: [isi topik, misalnya "desain halaman Tour" atau "menyiapkan Laravel di XAMPP"]. Berikut catatan proyek lengkapnya: [tempel isi file ini].
