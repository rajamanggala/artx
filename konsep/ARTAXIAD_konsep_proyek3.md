# Konsep Proyek: Situs Resmi ARTAXIAD

Catatan ini merangkum keputusan, status, dan pertanyaan terbuka dari semua diskusi sebelumnya. Diperbarui 13 November 2026 (menggantikan versi `konsep_proyek2`). Tempel isinya di awal chat baru, lalu lampirkan juga file halaman yang mau dikerjakan (`index.html`, `perform.html`, `news.html`, `music.html`, `media.html`) supaya konteksnya langsung terbawa.

## 1. Ringkasan

- **Apa:** situs resmi band lokal bernama **ARTAXIAD** (Artaxiad Gamelan Syndicate), dari daerah Jawa (Boyolali, Jawa Tengah). Sering dipanggil mengiringi performing arts (tari, teater, wayang), selain pentas sendiri.
- **Menu (final):** Home, **Perform** (sebelumnya "Tour"), News, Music, Media, Merch.
- **Bahasa situs:** isi halaman berbahasa Indonesia. Label menu bahasa Inggris (Home, Perform, News, Music, Media, Merch).
- **Pengguna admin:** orang awam yang hanya perlu mengetik dan memposting konten.
- **Referensi gaya:** linkinpark.com, metallica.com (konsep, bukan tiruan). Yang dipakai hanya prinsipnya: hero layar penuh, bingkai hitam di tepi layar, tab menu yang menyatu dengan bingkai, kartu kaca, tombol berbentuk pil, serta susunan grid halaman daftar. Logo, warna, font, efek, dan isi dibuat sendiri.
- **Aturan desain:** Home adalah landing page dengan efek khusus. Halaman menu lain memakai **template yang sama satu sama lain** (bingkai, video latar, efek embun, gradient gelap, area isi yang bisa digulir), tetapi **tampilan isinya berbeda per halaman**.

## 2. Keputusan yang sudah diambil

| Topik | Keputusan | Catatan |
|---|---|---|
| Teknologi halaman | HTML + CSS + JavaScript biasa | Tanpa npm, tanpa build, tanpa Node.js. Cukup upload ke hosting. |
| Backend | **Laravel** | Pilihan pengguna. Masih menunggu konfirmasi SSH dan Composer di hosting. |
| Database | MySQL | Tersedia di hampir semua paket hosting. |
| WordPress | Tidak dipakai | Dinilai pengguna terlalu tidak fleksibel. |
| Hosting | Paket **Premium**, durasi **12 bulan** | Belum ada konfirmasi sudah dibeli. Lihat bagian 7. |
| Pratinjau lokal | XAMPP, alamat `http://localhost/artaxiad/` | Wajib lewat server, bukan klik dua kali (lihat bagian 8). |
| Nama menu "Tour" | Diganti **Perform** | Grup ini pentas, bukan tour. Isi halaman: event yang melibatkan ARTAXIAD. |
| Tagline halaman Perform | "Jejak panggung ARTAXIAD." | Tagline lama dinilai norak. |
| Merch | Hanya menampilkan produk dengan tombol beli ke marketplace atau WhatsApp | Tanpa keranjang dan pembayaran sendiri. Halaman belum dibuat. |
| Media | Foto, bergaya galeri, berisi cuplikan dari Instagram | Foto disimpan di hosting sendiri (lihat bagian 5.5). |
| Ikon | Font Awesome 6.5.2 lewat CDN (cdnjs) | Sudah dipasang di `<head>`. |
| Data awal | Data dummy ditulis manual dalam array JavaScript di tiap halaman | Nanti diganti data dari Laravel. |

## 3. Desain landing page (Home)

### Palet warna (hanya tiga)
- Hitam `#000000`: bingkai, tab, menu, footer, panel popup.
- Merah `#D62420`: tombol utama, penanda menu aktif, cincin preloader, garis atas popup, kartu Perform.
- Putih `#FFFFFF`: semua tulisan (dan panel isi di News dan Media).

### Tipografi
- **Martian Mono** (Google Fonts) untuk semua teks.

### Tata letak (desktop)
- Hero setinggi layar (`100dvh`) dengan **video latar** `bg.mp4`.
- Bingkai hitam di tepi layar dengan sudut membulat. Tab logo (kiri atas) dan tab menu (kanan atas) menyatu dengan bingkai lewat lengkung cekung. Tab footer di kiri dan kanan bawah.
- **Logo tengah:** gambar `img/logo.png` (tulisan ARTAXIAD Gamelan Syndicate), lebar `clamp(280px,70vw,1020px)`.
- **Logo pojok kiri atas:** gambar `img/circle-logo.png`, 84px (64px di HP).
- **Kartu kaca rilisan di kiri bawah:** sampul dari thumbnail YouTube, judul **Pyrophobic**, deskripsi "Artaxiad Gamelan.", tombol merah "Play Now". Sampul dan tombol membuka `https://www.youtube.com/watch?v=rOLVCdr5AdE` di tab baru.
- Tombol Follow dan Subscribe di kanan bawah.
- Menu kanan atas berukuran 16px, jarak antarmenu 32px.

### Tampilan HP (di bawah 820px)
- Menu berubah menjadi **ikon garis tiga**.
- Tab kiri atas (logo) dan kanan atas (garis tiga) **sama besar 92x92px**.
- **Follow dan Subscribe berada di bawah kartu rilisan**, rata tengah, tepat di atas footer.
- Layar menu penuh **meluncur dari kiri ke kanan** saat dibuka (tombol "Close" di pojok kanan atas layar menu). Resolusi efek embun diturunkan.

### Preloader
- Layar hitam dengan `circle-logo.png` berdenyut, dikelilingi dua lingkaran animasi (merah searah jarum jam, putih berlawanan arah).
- Hilang setelah halaman dan video siap. Minimal tampil 1,4 detik, maksimal 6 detik.
- Saat ini **tampil di setiap halaman** (termasuk halaman lain). Ide: tampil sekali per kunjungan lewat `sessionStorage`.

### Popup Follow
- Klik **Follow** membuka popup di tengah layar (panel hitam, garis merah di atas). Bisa ditutup dengan tombol ✕, klik area gelap, atau Esc.
- Tautan:
  - Spotify: `https://open.spotify.com/intl-id/artist/23QCPAlvzfpBvWV6s8SQoL`
  - Instagram: `https://www.instagram.com/artaxiadgamelan/`
  - YouTube: `https://www.youtube.com/@artaxiadgamelanofficial2780`
  - Facebook: `https://www.facebook.com/artaxiadgamelan/`
  - TikTok: `https://www.tiktok.com/@artaxiadgamelan`
- Tombol **Subscribe** belum terhubung. Usulan: `https://www.youtube.com/@artaxiadgamelanofficial2780?sub_confirmation=1`.

### Efek utama: kaca berembun dengan air (dilap kursor)
- Video latar diburamkan (bokeh) dengan embun bercak tebal-tipis, **garis air panjang**, dan **bintik embun kecil**. Video dipakai sebagai tekstur WebGL, dengan mode cadangan bila tekstur gagal dan cadangan CSS bila WebGL tidak ada.
- **Mouse atau jari menjadi pengelap:** area yang dilap jernih, lalu embun kembali perlahan (sekitar 2 detik tahan, total sekitar 7 detik).
- **Revisi terbaru (13 Nov 2026), berdasarkan umpan balik pengguna:**
  - Garis air tadinya terlalu berkelok ("seperti ulat"). Sekarang **hampir lurus ke bawah** dengan goyang sangat halus: `float cx=(id+.5)*colw+(b-.5)*colw*.35+.0010*sin(q.y*7.+c*6.28);`
  - **Bintik kecil diperbanyak:** kepadatan `b1=.4`, `b2=.65`, `b3=.75`, ditambah lapisan baru `b4=beads(q+37.7,120.,.8,.25)` (ikut dimasukkan ke `n` dan `dm`).

### Parameter yang bisa disetel (di dalam `index.html` dan di tiap halaman yang memuat efek ini)
- `.0010*sin(q.y*7.+c*6.28)` : kelokan garis air (kecilkan untuk lebih lurus, `0.` untuk lurus total).
- `beads(...)` : angka ke-3 tiap baris adalah kepadatan bintik (maks. `1.`). Hapus `b4` jika berat.
- `float br=.017*wet*band*pch;` : ketebalan embun. `float pch=.7+.6*fbm(q*3.+7.);` : kekuatan bercak.
- `samp(uv-n*.024)` : kekuatan pembiasan tetesan. `spec=...*dm*.9` : kilap tetesan.
- `streak(q,22.,1.)`, `15.`, `11.` : kerapatan kolom garis air. `if(a>.72)` : peluang garis muncul.
- `.012+.02*s` : kecepatan garis air. `float w=(.0042+.0048*c);` : lebar garis air.
- `LIFE = 7000`, `HOLD = 2000` : lama embun kembali setelah dilap (ms). `brush` : ukuran lap.
- Preloader: `MIN=1400`, `MAX=6000`. HP: `bottom:48px` (`.cta`) dan `bottom:92px` (`.card`).

## 4. Template halaman dalam (Perform, News, Music, Media)

Dibuat dari `index.html`, dengan perbedaan:

- **Halaman tidak bergeser.** Seperti index, `html,body` memakai `overflow:hidden`. Yang bisa digulir hanya area isi `.page` di dalam bingkai (scrollbar tipis merah, tepi atas dan bawah memudar lewat `mask-image`).
- Posisi `.page`: `top:120px; bottom:62px` di desktop, `top:108px; bottom:58px` di bawah 820px. Padding samping `clamp(14px,2.4vw,36px)`.
- Judul halaman (`h1`, huruf besar tebal) dan tagline pendek di atas daftar.
- **Lapisan gelap `.dim`** (gradient hitam .74 sampai .92) diletakkan **di atas canvas**, supaya video terlihat samar. Penting: canvas WebGL menggambar video secara opaque, jadi `.shade` yang ada di bawah canvas tidak terlihat; gradient harus di atas canvas.
- Efek embun dan lap kursor tetap hidup di semua halaman. Canvas `pointer-events:none`, jadi kartu dan tautan tetap bisa diklik.
- Tab kiri atas (logo) mengarah ke `index.html`.
- Menu aktif memakai `aria-current="page"` (garis merah di bawah menu).
- Isi daftar dirender dari **array JavaScript** di akhir file, sehingga mudah diganti data dari database.

## 5. Halaman yang sudah dibuat

### 5.1 `perform.html` (Perform)
- Grid **3 kolom** (2 di tablet, 1 di HP). **Kartu merah `#D62420` bertulisan hitam**, mengikuti referensi kartu tour linkinpark. Saat disorot kartu berubah hitam bertulisan putih.
- Isi kartu: tanggal, kota dan provinsi (judul besar), tempat, **Peran** ARTAXIAD (Pentas sendiri / Pengiring tari / Pengiring teater / Kolaborasi), **Bersama** (nama seniman atau grup yang diiringi, bila ada), keterangan tiket, tautan "Info lengkap" (masih `#`).
- Event yang sudah selesai: tampil agak pudar dengan keterangan dicoret.
- Data: array `EVENTS`, 9 event dummy (7 mendatang, 2 selesai).
- Tagline: "Jejak panggung ARTAXIAD." (di file `perform.html` yang dikirim ke pengguna masih tertulis tagline lama; pengguna sudah diberi potongan untuk menggantinya).

### 5.2 `news.html` (News)
- **Panel putih flat bersudut lancip** (`border-radius:0`), lebar maks. 1600px, `padding:30px`, jarak antarberita `44px`.
- Kartu memanjang **70% gambar : 30% teks**, **selang-seling** (ganjil gambar di kiri, genap gambar di kanan dengan teks rata kanan), `aspect-ratio:3/1`. Tombol "Baca selengkapnya" hitam, menjadi merah saat disorot.
- Di HP (di bawah 760px): gambar di atas, teks di bawah, satu kolom.
- **Data (ditulis manual):** 8 artikel dari blog `https://gamelanartxplay.wordpress.com/category/di-balik-panggung-gamelan/` (3 sampai 28 Juni 2024, penulis tim redaksi "Gamelan Hari Ini oleh ARTX.PLAY"). Judul dan tanggal sesuai aslinya, ringkasan ditulis ulang manual, thumbnail dimuat langsung dari server blog, tombol membuka artikel asli di tab baru.
- Catatan: ringkasan hanya berdasar pembukaan tiap artikel. Gambar yang dimuat dari server WordPress bisa hilang bila blog menghapusnya; sebaiknya diunduh ke `img/news/`. Status hak pakai tulisan dan foto blog **belum dikonfirmasi** (ditanyakan, belum dijawab).
- Struktur data dummy: `d` (tanggal), `t` (judul), `s` (ringkasan), `img` (thumbnail), `u` (link).
- Tagline: "Kabar terbaru ARTAXIAD."

### 5.3 `music.html` (Music)
- Mengikuti referensi linkinpark.com/music: grid **3 kolom**, tanpa kartu. **Sampul persegi di kiri, judul besar putih di kanan**, di bawahnya "tanggal • YouTube" kecil keabuan. Judul menjadi merah saat disorot.
- **Enam rilisan dari link YouTube** (id video): `dlJXFrDe8Jk`, `CpR7s5f7PUk`, `rOLVCdr5AdE`, `EElXmoP3jXQ`, `D1rFsa9nfsw`, `B87-RIEKNXg`. Klik sampul atau judul membuka video di tab baru.
- **Thumbnail otomatis** dari `https://i.ytimg.com/vi/ID/maxresdefault.jpg` (cadangan `hqdefault.jpg`), dipotong persegi dengan `object-fit:cover`.
- **Judul diambil otomatis** lewat YouTube oEmbed dari browser (sudah terbukti jalan di browser pengguna). Judul yang tampil di layar: Jantung Ibu; Bully; Pyrophobic (2023); MCPR feat Artaxiad Gamelan – Belajar Belajar (Official Video); Kisah Kasih Rama Sinta I Ramayana I Dhimas Artaxiad Gamelan I Drayang (Drama Wayang) Swargaloka; Artaxiad Gamelan x Teori – Jantung Ibu.
- **Tanggal upload belum terisi** (kecuali "2023" untuk Pyrophobic). Tidak bisa diambil otomatis tanpa YouTube Data API; diisi manual di kolom `d` pada array `ITEMS`. Asisten tidak bisa membuka halaman YouTube (diblokir), jadi tidak mengambil judul dan tanggal sendiri.
- **Pemotongan judul panjang:** usulan `var MAX=24; function cut(s){return s.length>MAX?s.slice(0,MAX).trim()+"…":s}` dipakai di render dan oEmbed (judul penuh jadi tooltip), plus `.rl h2{overflow-wrap:anywhere}`. Belum dipastikan sudah dipasang di file pengguna.
- Filter jenis (seperti LIVE, ALBUMS di referensi) belum dibuat karena kategori rilisan belum jelas.
- Tagline: "Putar karya ARTAXIAD."

### 5.4 `media.html` (Media)
- Mengikuti referensi metallica.com (galeri foto): **panel putih bersudut lancip** seperti News, di dalamnya grid **3 kolom** (2 di tablet, 1 di HP). Tiap item: gambar 16:9, tanggal kecil, judul tebal. Penanda **"+N"** di tengah gambar bila satu postingan berisi banyak foto. Tanggal dan judul bergaris bawah saat disorot.
- Data dummy ditulis manual: array `MEDIA` berisi 9 item, kolom `d` (tanggal), `t` (judul), `img` (`img/media/1.jpg`, dst.), `c` (jumlah "+N"), `u` (link postingan; sementara mengarah ke profil Instagram).
- **Foto belum ada.** Selama `img/media/*.jpg` belum ada, semua item menampilkan logo ARTAXIAD di kotak hitam.
- Tagline: "Cuplikan dari Instagram ARTAXIAD."

### 5.5 Soal mengambil foto dari Instagram
- **Tidak bisa disematkan langsung** dari HTML biasa: Instagram tidak punya feed publik, alamat gambarnya sementara (kedaluwarsa dalam hitungan hari), gambar sering diblokir bila dipanggil dari situs lain, dan embed resmi tidak bisa ditata menjadi grid sendiri.
- **Asisten juga tidak bisa membuka** Instagram dan Facebook (akses otomatis dilarang), jadi tidak bisa melihat isi akun atau link foto Facebook yang diberikan.
- **Cara yang dipilih sementara:** simpan foto sendiri di `img/media/` (nama `1.jpg`, `2.jpg`, dst., lebar sekitar 1600px, di bawah 300 KB). Pengguna bisa mengirim fotonya sebagai gambar ke chat agar asisten menuliskan judul dan array `MEDIA`.
- **Cara otomatis nanti (setelah Laravel jadi):** Instagram API with Instagram Login. Syarat: akun **Business atau Creator**, aplikasi di Meta for Developers, token sekitar 60 hari yang diperpanjang berkala dan disimpan di server. Laravel mengunduh dan menyimpan fotonya. Status jenis akun ARTAXIAD belum diketahui.
- **Alternatif:** layanan widget pihak ketiga (belum dicek) atau upload manual lewat panel admin.

## 6. Arsitektur yang direncanakan

### Frontend
- Halaman publik berupa HTML + JavaScript biasa (atau templat Blade di Laravel).
- Landing page tetap statis. Halaman lain nanti mengambil data dari Laravel (array dummy diganti data database).

### Backend (Laravel + MySQL)
Entitas data yang diperlukan:

| Menu | Data |
|---|---|
| Perform | tanggal, kota, tempat, **peran** (pentas sendiri, pengiring tari, pengiring teater, kolaborasi), **bersama** (nama seniman atau grup), keterangan tiket, link info, status selesai |
| News | judul, ringkasan, isi, foto, link sumber, status draf atau terbit, tanggal |
| Music | judul, tanggal upload, link YouTube (thumbnail otomatis), jenis (single, EP, album), link Spotify, link Apple Music |
| Media | judul, tanggal, foto, jumlah foto tambahan, link postingan Instagram |
| Merch | nama, foto, harga, link beli |
| Pengaturan | link sosial media (Spotify, Instagram, YouTube, Facebook, TikTok), rilisan yang ditampilkan di hero (judul, link YouTube, thumbnail) |

### Panel admin untuk orang awam (`/admin`)
- Login sederhana (email dan password). Satu formulir per menu, label bahasa Indonesia.
- Foto otomatis dikecilkan saat diupload. Tombol "Simpan sebagai draf" dan "Terbitkan", plus pratinjau.
- Daftar item dengan Edit dan Hapus (ada konfirmasi sebelum hapus).
- Keamanan dasar: password terenkripsi, CSRF, validasi file upload.
- Untuk Perform: kolom pilihan "Peran" agar tetap mudah bagi orang awam.
- Alternatif jika membuat sendiri terasa berat: paket admin siap pakai seperti Filament. Belum diputuskan.

## 7. Status file

Isi folder lokal `C:\xampp\htdocs\artaxiad\`:

| File atau folder | Isi |
|---|---|
| `index.html` | Landing page (Home). Garis air sudah lurus, bintik sudah diperbanyak. Menu atas sudah diisi tautan; **menu HP perlu dicek** (lihat bagian 8). |
| `perform.html` | Halaman Perform (kartu merah). |
| `news.html` | Halaman News (panel putih, kartu 70/30 selang-seling). |
| `music.html` | Halaman Music (grid sampul YouTube). |
| `media.html` | Halaman Media (grid foto, menunggu foto asli). |
| `img/logo.png`, `img/circle-logo.png` | Logo tengah dan logo lingkaran. |
| `video/bg.mp4` (atau `bg.mp4`) | Video latar. Kode menerima kedua jalur. |
| `img/poster.jpg` | Opsional, foto cadangan sebelum video termuat. Belum ada. |
| `img/news/` | (disarankan) tempat menyimpan thumbnail berita bila diunduh dari blog. |
| `img/media/` | Foto untuk halaman Media (`1.jpg`, `2.jpg`, dst.). Belum diisi. |
| `konsep/` | Folder catatan konsep (taruh file `.md` ini di sini). |

### Tautan menu yang perlu diseragamkan
Pola yang benar di setiap halaman: Home `index.html` (di `index.html` sendiri cukup `#`), Perform `perform.html`, News `news.html`, Music `music.html`, Media `media.html`, Merch `#` sampai halamannya ada. Halaman yang sedang dibuka diberi `aria-current="page"`. Ini berlaku untuk **menu atas** dan **menu HP** (di `<div class="sheet">`).
- `index.html`: menu atas sudah benar sampai Music; menu HP masih versi lama (label "Tour", semua `#`). Pengguna sudah diberi baris pengganti. **Media** masih `#`.
- `perform.html`, `news.html`: Music dan/atau Media masih `#` (dan di `perform.html` milik pengguna, News juga). Pengguna menunda pengerjaan ini ("nanti dulu").
- `music.html` dan `media.html` (versi dari asisten) sudah benar untuk halaman yang ada saat dibuat; `media.html` mengarah ke `media.html`.
- Logo kiri atas di halaman dalam harus `href="index.html"`.

## 8. Hosting dan biaya

- **Belum ada konfirmasi pembelian.** Pilihan: paket **Premium**, durasi **12 bulan**. Halaman checkout yang dilihat tampak seperti **Hostinger** (hPanel).
- Perkiraan biaya Premium 12 bulan: sekitar **Rp478.800** ditambah pajak sekitar 11%, jadi sekitar **Rp531.000** sekali bayar untuk setahun.
- Perkiraan tahun kedua: harga normal sekitar **Rp84.900 per bulan** (belum dikonfirmasi).
- Paket **Unlimited** tidak perlu (total checkout yang terlihat Rp797.868 untuk 12 bulan). Tawaran 48 bulan tidak disarankan karena mengunci pengguna lama.
- Harga per bulan di halaman hosting adalah harga promo yang **dibayar sekaligus di muka** untuk seluruh durasi. Garansi uang kembali 30 hari tersedia.

### Yang harus dikonfirmasi ke chat support sebelum membayar
1. Apakah Premium sudah termasuk **akses SSH**? (Sumber yang ditemukan tidak sepakat.)
2. Apakah **Composer 2** tersedia? (Dokumentasi resmi menyebut Composer terpasang di Premium.)
3. Versi **PHP** tertinggi yang bisa dipilih (Laravel baru butuh PHP 8.2 atau 8.3 ke atas).
4. Apakah bisa menjalankan `php artisan` lewat SSH?
5. Apakah **document root** bisa diarahkan ke folder `public`? (Di Hostinger, web root biasanya tetap `public_html`, jadi struktur Laravel perlu disesuaikan: kode di luar `public_html`, isi `public` diarahkan ke sana.)

Jika SSH tidak ada di Premium, opsinya naik ke paket Business atau memakai PHP terstruktur tanpa Laravel.

## 9. Catatan teknis penting (pelajaran selama pengembangan)

- **Wajib lewat server.** Efek air memakai video sebagai tekstur WebGL. Jika halaman dibuka dengan klik dua kali (`file://`), browser memblokir video itu, sehingga embun tampil tanpa pembiasan. Lewat `http://localhost/artaxiad/` (XAMPP, nyalakan Apache) efek penuh berjalan.
- Jalur video: kode mencoba `video/bg.mp4` dulu, lalu `bg.mp4`.
- Efek ini cukup berat. Resolusi kanvas diturunkan di HP (55%) dan desktop (75%). Ide pengaman: deteksi FPS rendah lalu pindah ke mode cadangan CSS.
- Saat pengguna memilih mode hemat data atau kurangi gerak, video tidak dimuat dan efek tidak bergerak.
- **Garis tembus di pojok kiri atas:** diatasi dengan menghapus `overflow:hidden` dari `.stage` dan memberi `border-radius` langsung ke video, canvas, `.shade`, `.fogcss`, dan `.dim`. Jangan kembalikan `overflow:hidden` ke `.stage`.
- **GLSL:** jangan memakai nama variabel `patch` (kata cadangan); dipakai `pch`. Jika embun hilang total, buka Console (F12) untuk melihat error shader.
- **Layar menu HP:** memakai `transform:translateX(-100%)` plus `visibility`, bukan `display:none`, supaya bisa dianimasikan.
- **Thumbnail YouTube:** `https://i.ytimg.com/vi/ID_VIDEO/maxresdefault.jpg` (cadangan `hqdefault.jpg`). Untuk video "Topic", gambar berisi sampul persegi di tengah bingkai 16:9, jadi dipakai `object-fit:cover` pada kotak persegi.
- **Gambar dari situs lain** (blog WordPress): pakai `referrerpolicy="no-referrer"` dan fallback `onerror` (logo ARTAXIAD). Lebih aman diunduh ke hosting sendiri.
- **`aspect-ratio` + `overflow:hidden` pada kartu:** tinggi kartu dikunci oleh rasio, isi yang terlalu panjang akan terpotong. Kecilkan font atau ubah rasio bila ada judul sangat panjang.
- Hak cipta: desain, gambar, dan konten situs referensi tidak boleh disalin. Tulisan dan foto blog dan Instagram adalah milik penulis dan pemiliknya; pastikan hak pakainya sebelum diterbitkan.

## 10. Yang masih placeholder

- Link Subscribe, Privacy Policy, Terms of Use, Merch (semua `#`).
- Link Apple Music, tiket, dan toko.
- Tautan "Info lengkap" di Perform (`#`). Belum ada halaman detail event maupun detail berita.
- Tanggal upload di Music (kecuali satu), foto asli di Media, link postingan Instagram asli.
- Data Perform dan Media adalah contoh buatan, bukan jadwal dan foto sebenarnya.
- Formulir newsletter belum dibuat.
- Poster cadangan `img/poster.jpg` dan gambar Open Graph (pratinjau saat link dibagikan) belum ada.

## 11. Langkah berikutnya (urutan usulan)

1. Rapikan **tautan menu** di semua halaman (bagian 7), termasuk menu HP di `index.html`.
2. Isi **tanggal upload** di Music dan **foto asli** di Media (kirim foto ke chat agar array ditulis).
3. Buat halaman **Merch** (halaman terakhir yang belum ada). Referensi desain belum ditentukan.
4. Putuskan soal hak pakai artikel blog di News; unduh thumbnail ke `img/news/` bila aman.
5. Konfirmasi paket hosting ke chat support (bagian 8), lalu beli.
6. Siapkan Laravel di XAMPP: instal Composer, buat proyek, migrasi tabel, model, dan route.
7. Bangun panel admin `/admin` dengan formulir sederhana per menu.
8. Pindahkan halaman statis menjadi templat Blade yang membaca dari database.
9. Deploy ke hosting (atur struktur `public_html`, `.env`, migrasi database, `storage:link`).
10. Pasang domain dan SSL, lalu uji di HP dan berbagai browser.
11. Tambahan: meta Open Graph, tampilan saat daftar kosong ("Belum ada jadwal, pantau terus"), preloader hanya sekali per kunjungan, halaman detail event dan berita, ambil foto Instagram otomatis lewat API.

## 12. Pertanyaan terbuka untuk diskusi berikutnya

- **Hak pakai blog Gamelan Hari Ini (ARTX.PLAY):** apakah blog itu milik ARTAXIAD atau timnya, atau sudah ada izin?
- Akun Instagram ARTAXIAD sudah **Business atau Creator**? (menentukan apakah API resmi bisa dipakai)
- Kapan foto Media dan tanggal upload Music akan disiapkan?
- Seperti apa karakter desain halaman **Merch**? Apakah ada referensi?
- Apakah domain yang diinginkan (misalnya `artaxiad.com`) sudah dicek ketersediaannya?
- Foto dan cover asli lain apa yang sudah ada?
- Apakah perlu formulir newsletter (Subscribe), dan ke mana datanya dikirim? Atau Subscribe cukup diarahkan ke YouTube?
- Apakah perlu halaman "About" atau profil personel?
- Apakah Music perlu filter jenis rilisan (single, EP, album)?
- Siapa yang akan mengelola konten, dan seberapa sering?
- Apakah hosting sudah dibeli, dan apa jawaban chat support soal SSH dan Composer?

## 13. Cara kerja yang disukai pengguna

- Berbahasa Indonesia, to the point.
- **Perubahan kecil:** jangan menulis ulang seluruh file. Berikan potongan kode ("ganti baris ini dengan ini") supaya pengguna menempel sendiri. Pernah dikoreksi pengguna karena file dibuat ulang untuk perubahan kecil.
- **Full kode** hanya bila pengguna meminta ("full kode", "ful kodenya") atau untuk file baru.
- Kalau pengguna berkata "jangan buat kode, cermati dulu", analisis dulu tanpa menulis kode.
- Pengguna kadang mengirim screenshot hasil di browser; baca dengan teliti apa yang sebenarnya terlihat sebelum menjawab (pernah terjadi salah tafsir "putih transparan").
- Pengguna memberi referensi lewat screenshot situs lain dan meminta strukturnya ditiru (bukan isinya).
- Untuk data contoh: pengguna biasanya meminta asisten menuliskannya manual.
- Untuk bagian Laravel: kerjakan satu langkah kecil per giliran dan tunggu hasil dicek sebelum lanjut. Kalau mengarahkan membuat file Blade, controller, dan sejenisnya, sertakan perintah artisan-nya, jangan menyuruh membuat file manual.
- Asisten tidak bisa membuka YouTube, Instagram, dan Facebook. Untuk konten dari sana, minta pengguna mengirim gambar atau teksnya.

## 14. Teks pembuka untuk chat berikutnya (salin dan tempel)

> Saya sedang membuat situs resmi band ARTAXIAD (Artaxiad Gamelan Syndicate) dengan rencana Laravel + MySQL, di hosting shared (kemungkinan Hostinger Premium). Saat ini semuanya masih HTML statis dengan data dummy. Halaman yang sudah jadi: `index.html` (landing page dengan video latar dan efek kaca berembun yang bisa dilap mouse), `perform.html` (kartu merah), `news.html` (panel putih, kartu 70/30 selang-seling), `music.html` (grid sampul YouTube), `media.html` (grid foto). Warna #000, #D62420, putih; font Martian Mono; ikon Font Awesome. Menu: Home, Perform, News, Music, Media, Merch (Merch belum dibuat). Halaman dalam memakai template yang sama (bingkai, video, embun, gradient gelap, hanya area isi yang bisa digulir). Admin adalah orang awam, jadi butuh panel admin yang sangat mudah. Cara kerja: perubahan kecil berupa potongan kode untuk saya tempel sendiri, full kode hanya kalau saya minta. Saya mau lanjut ke: [isi topik, misalnya "halaman Merch" atau "rapikan tautan menu"]. Berikut catatan proyek lengkapnya: [tempel isi file ini].
