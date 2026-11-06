# Ular Tangga Rimba

Permainan ular tangga 3D bergaya buku cerita: lempar dadu, naik tangga bambu, meluncur di ular, petik buah emas, dan kumpulkan stiker di rimba Nusantara. Dimainkan langsung di browser dan bisa dipasang ke layar utama HP.

Situs: https://ular-tangga.dafenka.com

## Isi folder

```
public/                  semua yang di-deploy
  index.html             seluruh permainan dalam satu file
  manifest.webmanifest   supaya bisa dipasang di HP (layar penuh, mendatar)
  sw.js                  service worker: tetap bisa main offline setelah kunjungan pertama
  _headers               aturan header dan cache untuk Cloudflare Pages
  favicon.svg, icons/    ikon situs dan aplikasi
  og-image.png           gambar pratinjau saat tautan dibagikan
  robots.txt, sitemap.xml
  vendor/three@0.160.0/  Three.js r160 yang di-host sendiri (lisensi MIT)
```

## Aset biner di repo ini

Berkas PNG (ikon di `public/icons/` dan `public/og-image.png`) tidak disimpan sebagai biner di repo ini, melainkan base64 di folder `assets-src/` — jalur unggah yang dipakai untuk mengisi repo hanya menerima berkas teks. Setelah mengkloning, kembalikan berkas aslinya sekali dengan:

```bash
node scripts/restore-assets.mjs
```

Situs yang live menyajikan berkas PNG asli; penyimpanan base64 ini hanya soal repo.

## Coba di komputer sendiri

Jalankan salah satu perintah ini dari folder repository:

```
npx serve public
python3 -m http.server 8080 --directory public
```

Lalu buka alamat yang muncul (misalnya http://localhost:8080). Membuka `index.html` dengan klik dua kali tidak akan jalan karena modul JavaScript butuh server.

## Unggah ke GitHub

Lewat website GitHub: buat repository baru (misalnya `ular-tangga`), pilih "uploading an existing file", seret `README.md` dan folder `public`, lalu Commit.

Lewat terminal:

```
git init
git add .
git commit -m "Ular Tangga Rimba"
git branch -M main
git remote add origin https://github.com/NAMA-AKUN/ular-tangga.git
git push -u origin main
```

## Deploy ke Cloudflare Pages

1. Di dasbor Cloudflare, buka **Workers & Pages**, buat proyek **Pages** baru, lalu hubungkan ke repository GitHub tadi.
2. Pengaturan build:
   - Framework preset: **None**
   - Build command: **kosongkan**
   - Build output directory: **`public`**
3. Simpan dan deploy. Situs langsung tersedia di alamat `nama-proyek.pages.dev`.

Setiap commit baru ke branch `main` akan di-deploy ulang otomatis. Kalau tidak ingin memakai GitHub, pilih opsi upload langsung (Direct Upload) lalu seret folder `public`.

## Pasang domain ular-tangga.dafenka.com

1. Di proyek Pages, buka tab **Custom domains**, lalu tambahkan `ular-tangga.dafenka.com`.
2. Kalau DNS `dafenka.com` dikelola Cloudflare, catatan CNAME-nya dibuat otomatis.
3. Kalau DNS ada di penyedia lain, buat CNAME `ular-tangga` yang mengarah ke `nama-proyek.pages.dev`.
4. Tunggu sampai statusnya aktif. Sertifikat HTTPS dibuat otomatis.

## Memperbarui game

Ganti `public/index.html`, lalu naikkan `VERSION` di `public/sw.js` (misalnya `ut-v2`) supaya pemain lama langsung mendapat versi terbaru.

## Catatan

- Font Gaegu dimuat dari Google Fonts. Kalau gagal dimuat, game memakai font cadangan.
- Kemajuan pemain (stiker, buah emas, rekor, permainan yang belum selesai) tersimpan di browser masing-masing pemain.
- Three.js © three.js authors, lisensi MIT (lihat `public/vendor/three@0.160.0/LICENSE`).
