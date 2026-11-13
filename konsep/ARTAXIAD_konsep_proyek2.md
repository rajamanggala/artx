# Konsep Proyek: Situs Resmi ARTAXIAD

Catatan ini merangkum keputusan, status, dan pertanyaan terbuka dari diskusi sebelumnya. Diperbarui 13 November 2026. Tempel isinya di awal chat baru, lalu lampirkan juga `index.html` terbaru supaya konteksnya langsung terbawa.

## 1. Ringkasan

- **Apa:** situs resmi band lokal bernama **ARTAXIAD** (Artaxiad Gamelan Syndicate), dari daerah Jawa (Boyolali, Jawa Tengah).
- **Menu:** Home, Tour, News, Music, Media, Merch.
- **Bahasa situs:** isi halaman berbahasa Indonesia. Label menu saat ini masih bahasa Inggris (lihat bagian 10).
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
| Ikon | Font Awesome 6.5.2 lewat CDN (cdnjs) | Sudah dipasang di `<head>`. |
| Data awal | Data dummy untuk halaman selain Home | Teks, jadwal, harga, dan judul rilisan halaman lain masih contoh. |

## 3. Desain landing page (Home)

### Palet warna (hanya tiga)
- Hitam `#000000`: bingkai, tab, menu, footer, panel popup.
- Merah `#D62420`: tombol utama, penanda menu aktif, cincin preloader, garis atas popup.
- Putih `#FFFFFF`: semua tulisan.

### Tipografi
- **Martian Mono** (Google Fonts) untuk semua teks.

### Tata letak (desktop)
- Hero setinggi layar (`100dvh`) dengan **video latar** `bg.mp4`.
- Bingkai hitam di tepi layar dengan sudut membulat. Tab logo (kiri atas) dan tab menu (kanan atas) menyatu dengan bingkai lewat lengkung cekung. Tab footer di kiri dan kanan bawah.
- **Logo tengah:** gambar `img/logo.png` (tulisan ARTAXIAD Gamelan Syndicate), lebar `clamp(280px,70vw,1020px)`. Tagline teks sudah dihapus.
- **Logo pojok kiri atas:** gambar `img/circle-logo.png`, 84px (64px di HP).
- **Kartu kaca rilisan di kiri bawah (diperkecil):** sampul diambil langsung dari thumbnail YouTube, judul **Pyrophobic**, deskripsi "Artaxiad Gamelan.", tombol merah "Play Now" dengan ikon Font Awesome `fa-play`. Sampul dan tombol membuka video YouTube di tab baru. Link: `https://www.youtube.com/watch?v=rOLVCdr5AdE` (video "Pyrophobic", Artaxiad Gamelan, rilis 2023).
- Tombol Follow dan Subscribe di kanan bawah.
- Menu kanan atas berukuran 16px, jarak antarmenu 32px.

### Tampilan HP (di bawah 820px)
- Menu berubah menjadi **ikon garis tiga** (bukan tulisan "Menu").
- Tab kiri atas (logo) dan kanan atas (garis tiga) **sama besar 92x92px** supaya simetris.
- **Follow dan Subscribe berada di bawah kartu rilisan**, rata tengah, tepat di atas footer.
- Tab footer kiri dan kanan sama-sama tampil dengan teks kecil (9px).
- Layar menu penuh **meluncur dari kiri ke kanan** saat dibuka (tombol "Tutup" di pojok kanan atas layar menu). Resolusi efek embun diturunkan.

### Preloader
- Layar hitam dengan `circle-logo.png` yang berdenyut pelan (3,2 detik), dikelilingi **dua lingkaran animasi**: cincin merah di luar (searah jarum jam, 1,6 detik) dan cincin putih di dalam (berlawanan arah, 2,2 detik).
- Hilang (fade 0,6 detik) setelah halaman dan video siap. Minimal tampil 1,4 detik, maksimal 6 detik.

### Popup Follow
- Klik **Follow** membuka popup di tengah layar. Latar memburam pelan, panel naik sambil membesar, baris-baris muncul bergantian. Bisa ditutup dengan tombol ✕, klik area gelap, atau tombol Esc.
- Desain: panel hitam pekat dengan garis merah di atas. Bagian **Listen** berisi tombol merah "Listen on Spotify". Bagian **Follow** berisi tombol sosial 2 kolom (1 kolom di layar sempit) dengan ikon dan nama.
- Tautan:
  - Spotify: `https://open.spotify.com/intl-id/artist/23QCPAlvzfpBvWV6s8SQoL`
  - Instagram: `https://www.instagram.com/artaxiadgamelan/`
  - YouTube: `https://www.youtube.com/@artaxiadgamelanofficial2780`
  - Facebook: `https://www.facebook.com/artaxiadgamelan/` (bagian `/photos/` sengaja dibuang supaya membuka halaman utama; kembalikan jika memang ingin ke tab foto)
  - TikTok: `https://www.tiktok.com/@artaxiadgamelan`
- Tombol **Subscribe** belum terhubung. Usulan: `https://www.youtube.com/@artaxiadgamelanofficial2780?sub_confirmation=1` (memunculkan dialog konfirmasi berlangganan).

### Efek utama: kaca berembun dengan air (dilap kursor)
Urutan evolusi: kabut bergerak, lalu tetesan bulat (dinilai tidak realistis), lalu **versi sekarang** yang mengikuti foto referensi pengguna.
- Video latar diburamkan (bokeh) dengan pita vertikal samar dan butiran halus.
- **Garis air panjang** yang merayap turun, berkelok, meninggalkan jalur bening. Tiga lapis kolom dengan jarak berbeda.
- **Bintik embun kecil** yang banyak, tetesan besar sengaja langka.
- **Peningkatan realisme (13 Nov 2026):** embun bercak tebal-tipis (noise `fbm`, variabel `pch`), tetesan membiaskan gambar lebih kuat, tepi tetesan gelap di atas dan terang di bawah, kilap lebih tajam. Belum diuji menyeluruh di berbagai perangkat; angkanya masih tebakan awal dan bisa disetel.
- **Mouse atau jari menjadi pengelap:** area yang dilap jernih, lalu embun kembali perlahan (sekitar 2 detik tahan, total sekitar 7 detik).
- Video dipakai sebagai tekstur WebGL. Ada mode cadangan (lapisan embun tanpa bias) dan cadangan CSS bila WebGL tidak ada.

### Parameter yang bisa disetel (di dalam `index.html`)
- `float br=.017*wet*band*pch;` : ketebalan embun.
- `float pch=.7+.6*fbm(q*3.+7.);` : kekuatan bercak (kecilkan `.6` untuk lebih rata).
- `samp(uv-n*.024)` : kekuatan pembiasan tetesan (naikkan untuk lebih kuat).
- `spec=...*dm*.9` : kilap tetesan.
- `streak(q,22.,1.)`, `15.`, `11.` : kerapatan kolom garis air. `if(a>.72)` : peluang garis muncul.
- `.012+.02*s` : kecepatan garis air. `float w=(.0042+.0048*c);` : lebar garis air.
- `LIFE = 7000`, `HOLD = 2000` : lama embun kembali setelah dilap (milidetik). `brush` : ukuran lap.
- Preloader: `MIN=1400`, `MAX=6000`; kecepatan denyut `3.2s`; ukuran `clamp(220px,40vmin,340px)`.
- HP: `bottom:48px` (`.cta`) dan `bottom:92px` (`.card`), selisih sekitar 44px.
- Kecepatan slide menu HP: `.55s` (dua baris `.sheet`).

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
| Music | judul, jenis (single, EP, album), tahun, cover, link Spotify, link Apple Music, link YouTube |
| Media | judul, link YouTube, jenis (video klip, live, galeri) |
| Merch | nama, foto, harga, link beli |
| Pengaturan | link sosial media (Spotify, Instagram, YouTube, Facebook, TikTok), rilisan yang ditampilkan di hero (judul, link YouTube, thumbnail) |

### Panel admin untuk orang awam (`/admin`)
- Login sederhana (email dan password).
- Satu formulir per menu, label bahasa Indonesia.
- Foto otomatis dikecilkan saat diupload.
- Tombol "Simpan sebagai draf" dan "Terbitkan", plus pratinjau.
- Daftar item dengan Edit dan Hapus (ada konfirmasi sebelum hapus).
- Keamanan dasar: password terenkripsi, CSRF, validasi file upload.
- Alternatif yang pernah disarankan jika membuat sendiri terasa berat: paket admin siap pakai seperti Filament. Belum diputuskan.

## 5. Status file

Isi folder lokal `C:\xampp\htdocs\artaxiad\` saat ini: `img/`, `index.html`, `konsep/`, `video/`.

| File | Isi |
|---|---|
| `index.html` | Landing page versi sekarang. **File utama.** Lampirkan di chat baru. |
| `img/logo.png` | Logo tengah (ARTAXIAD Gamelan Syndicate). |
| `img/circle-logo.png` | Logo lingkaran (pojok kiri atas dan preloader). |
| `video/bg.mp4` (atau `bg.mp4`) | Video latar milik pengguna. Kode menerima kedua jalur. |
| `img/poster.jpg` | Opsional: foto cadangan sebelum video termuat. Belum ada. |
| `konsep/` | Folder catatan konsep. |

Versi cadangan lama (`index_v2.html`, `index_bintik.html`, `index_kabut.html`) tidak terlihat lagi di folder; belum dipastikan apakah masih disimpan di tempat lain.

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
- Efek ini cukup berat. Resolusi kanvas sudah diturunkan di HP (55%) dan desktop (75%). Jika terasa patah-patah di laptop lemah, bobot efek perlu diturunkan. Ide pengaman: deteksi FPS rendah lalu otomatis pindah ke mode cadangan CSS.
- Saat pengguna memilih mode hemat data atau kurangi gerak, video tidak dimuat dan efek tidak bergerak.
- **Garis tembus di pojok kiri atas:** diatasi dengan menghapus `overflow:hidden` dari `.stage` dan memberi `border-radius` langsung ke video, canvas, `.shade`, dan `.fogcss`. Jangan kembalikan `overflow:hidden` ke `.stage`.
- **GLSL:** jangan memakai nama variabel `patch` (kata cadangan, bisa membuat shader gagal kompilasi); dipakai `pch`. Jika embun hilang total, buka Console (F12) untuk melihat error shader.
- **Layar menu HP:** memakai `transform:translateX(-100%)` plus `visibility`, bukan `display:none`, supaya bisa dianimasikan.
- **Thumbnail YouTube:** `https://i.ytimg.com/vi/ID_VIDEO/maxresdefault.jpg` (cadangan `hqdefault.jpg`). Untuk video "Topic", gambar berisi sampul persegi di tengah bingkai 16:9, jadi dipakai `object-fit:cover` pada kotak persegi. Lebih aman disimpan ke `img/` agar tidak bergantung pada YouTube.
- Semua menu dan beberapa tombol (Subscribe, Privacy Policy, Terms of Use, menu) masih berupa `#`. Perlu diarahkan ke halaman dan link sebenarnya.
- Hak cipta: desain, gambar, dan konten linkinpark.com tidak boleh disalin. Hanya konsep umumnya yang dipakai sebagai inspirasi.

## 8. Yang masih placeholder

- Link Subscribe, Privacy Policy, Terms of Use, dan semua menu (`#`).
- Link Apple Music, tiket, dan toko.
- Jadwal tour, berita, daftar rilisan, dan harga merch pada halaman lain.
- Formulir newsletter belum dibuat.
- Poster cadangan `img/poster.jpg` belum ada.
- Gambar Open Graph (pratinjau saat link dibagikan ke WhatsApp/Instagram) belum ada.

## 9. Langkah berikutnya (urutan usulan)

1. Konfirmasi paket hosting ke chat support (poin di bagian 6), lalu beli. Bisa berjalan paralel dengan langkah 2.
2. Desain halaman Tour, News, Music, Media, Merch (statis dengan data dummy dulu, berbeda dari landing page). Usulan: mulai dari Tour karena strukturnya paling sederhana dan jadi pola untuk halaman lain.
3. Siapkan Laravel di XAMPP: instal Composer, buat proyek, migrasi tabel, model, dan route.
4. Bangun panel admin `/admin` dengan formulir sederhana per menu.
5. Pindahkan halaman statis menjadi templat Blade yang membaca dari database.
6. Deploy ke hosting (atur struktur `public_html`, `.env`, migrasi database, `storage:link`).
7. Pasang domain dan SSL, lalu uji di HP dan berbagai browser.
8. Tambahan: meta Open Graph, halaman Tour saat kosong ("Belum ada jadwal, pantau terus"), preloader hanya sekali per kunjungan (`sessionStorage`) setelah ada halaman lain.

## 10. Pertanyaan terbuka untuk diskusi berikutnya

- **Nama menu: bahasa Indonesia atau Inggris?** Sedang dibahas. Saran dari diskusi: tetap Inggris untuk label menu (Tour, News, Music, Media, Merch) karena sudah baku di situs band, "Merch" tidak punya padanan enak, dan lebih pendek untuk tab; isi halaman tetap Indonesia. Alternatif: penuh Indonesia (Beranda, Jadwal, Berita, Musik, Media, Merch). Keputusan tergantung target pengunjung (fans lokal atau komunitas musik yang lebih luas).
- Seperti apa karakter desain halaman Tour, News, Music, Media, dan Merch yang diinginkan? Apakah ada referensi?
- Apakah domain yang diinginkan (misalnya `artaxiad.com`) sudah dicek ketersediaannya?
- Foto dan cover asli lain apa yang sudah ada?
- Apakah perlu formulir newsletter (Subscribe), dan ke mana datanya dikirim? Atau Subscribe cukup diarahkan ke YouTube?
- Apakah perlu halaman "About" atau profil personel?
- Apakah perlu pemutar musik (embed Spotify atau YouTube) di halaman Music?
- Siapa yang akan mengelola konten, dan seberapa sering?

## 11. Cara kerja yang disukai pengguna

- Berbahasa Indonesia, to the point.
- Kode disalin-tempel, jadi perubahan dijelaskan per bagian (ganti baris mana dengan apa). Kalau pengguna meminta "full kode", berikan seluruh file `index.html` lengkap.
- Kalau pengguna berkata "jangan buat kode, cermati dulu", analisis dulu tanpa menulis kode.
- Untuk bagian Laravel: kerjakan satu langkah kecil per giliran dan tunggu hasil dicek sebelum lanjut. Kalau mengarahkan membuat file Blade, controller, dan sejenisnya, sertakan perintah artisan-nya, jangan menyuruh membuat file manual.

## 12. Teks pembuka untuk chat berikutnya (salin dan tempel)

> Saya sedang membuat situs resmi band ARTAXIAD (Artaxiad Gamelan Syndicate) dengan Laravel + MySQL, di hosting shared (kemungkinan Hostinger Premium). Landing page-nya sudah jadi (file `index.html` saya lampirkan): HTML statis dengan video latar dan efek kaca berembun yang bisa dilap dengan mouse (WebGL), preloader dua lingkaran, popup Follow, kartu rilisan dengan thumbnail YouTube, warna #000, #D62420, putih, font Martian Mono, ikon Font Awesome. Menu: Home, Tour, News, Music, Media, Merch. Halaman selain Home harus berdesain berbeda dari landing page. Admin adalah orang awam, jadi butuh panel admin yang sangat mudah. Saya mau lanjut ke: [isi topik, misalnya "desain halaman Tour" atau "nama menu: Indonesia atau Inggris"]. Berikut catatan proyek lengkapnya: [tempel isi file ini].
