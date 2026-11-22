# Panduan Serah Terima GPT — Blog Burung DHIKA JS BALI

Dokumen ini adalah sumber konteks operasional untuk GPT mana pun yang membantu mengelola website DHIKA JS BALI. GPT yang baru mengambil alih wajib membaca dokumen ini sebelum mengubah repository atau layanan Cloudflare. Utamakan kondisi repository terbaru, bukan asumsi atau percakapan lama.

## 1. Identitas proyek

- Website: https://dhikajsbali.com/
- Halaman daftar postingan: https://dhikajsbali.com/burung/
- Repository GitHub: https://github.com/dhikajalaksurenbali/blog-burung
- Pemilik/akun GitHub: `dhikajalaksurenbali88`
- Branch utama: `main`
- Teknologi: Astro, Markdown/MDX, dan Cloudflare Workers.
- File artikel: `src/content/blog/`
- File gambar publik: `public/images/`
- Halaman daftar artikel: `src/pages/burung/index.astro`
- Halaman depan mengambil tiga artikel terbaru dari koleksi blog: `src/pages/index.astro`
- Halaman detail artikel: `src/pages/burung/[...slug].astro`

## 2. Tujuan dan prinsip kerja GPT

GPT membantu Om Dhika menyiapkan dan menerbitkan postingan burung yang benar-benar serius, akurat, rapi, dan siap dipublikasikan di website DHIKA JS BALI.

Prinsip wajib:
- Jangan membuat artikel demo, artikel tes, contoh palsu, atau postingan asal-asalan.
- Jangan mengarang fakta tentang burung, asal-usul, usia, jenis kelamin, kesehatan, legalitas, stok, harga, atau ketersediaan.
- Pertahankan judul dan isi tulisan yang diberikan Om Dhika. Jangan meringkas, menulis ulang, memperbaiki gaya, atau mengubah maksud tanpa diminta.
- Rapikan format Markdown seperlunya tanpa mengubah kata-kata, emoji, urutan, atau maksud tulisan.
- Jika informasi penting belum jelas, tanyakan satu hal yang paling diperlukan. Jangan menebak.
- Jangan mengubah desain, halaman, kode, konfigurasi, atau sistem upload untuk tugas posting artikel biasa.
- Jangan menghapus atau mengganti artikel maupun gambar lama kecuali Om Dhika meminta secara eksplisit.
- Jangan pernah menyimpan token, password, API key, atau nilai secret di artikel maupun file dokumentasi.

## 3. Alur posting yang diinginkan Om Dhika

Alur percakapan posting:

1. Om Dhika mengirimkan **judul** dan **tulisan** kepada GPT. Tulisan bisa berisi beberapa baris, emoji, tanda centang, dan format sederhana.
2. GPT menyiapkan artikel berdasarkan teks tersebut tanpa mengubah isi atau maksud.
3. Jika ada foto, GPT membuat/meminta sesi upload melalui Worker upload dan memberikan tautan upload sementara kepada Om Dhika.
4. Om Dhika membuka tautan tersebut, mengunggah foto, lalu menyalin URL gambar hasil upload.
5. Om Dhika kembali ke percakapan dan mengirimkan URL gambar hasil upload kepada GPT.
6. GPT memeriksa bahwa URL gambar benar-benar tersedia di repository, lalu membuat file Markdown artikel di `src/content/blog/`.
7. GPT melakukan commit ke branch `main`.
8. GPT memeriksa hasil commit dan, jika tersedia, status build/deployment Cloudflare.
9. GPT memberi laporan yang membedakan dengan jelas: file sudah di-commit, deployment sedang/berhasil, atau website sudah diverifikasi menampilkan artikel.

Jangan mengatakan artikel sudah tayang hanya karena file berhasil di-commit. Deployment dan tampilan website harus diperiksa secara terpisah. Jika GPT tidak mempunyai akses untuk membuat sesi upload atau memeriksa deployment, jelaskan keterbatasannya dan minta Om Dhika melakukan langkah yang diperlukan. Jangan mengarang tautan upload sementara.

## 4. Format artikel dan tanggal

Semua artikel baru disimpan sebagai file Markdown di `src/content/blog/`.

Skema frontmatter yang diterapkan di `src/content.config.ts`:
- `title`: wajib, string.
- `description`: wajib, string.
- `pubDate`: wajib, tanggal/waktu yang dapat dibaca sebagai Date.
- `updatedDate`: opsional.
- `heroImage`: opsional menurut skema, berisi URL/path gambar utama.

Pola frontmatter artikel:

```yaml
---
title: "Judul artikel dari Om Dhika"
description: "Ringkasan singkat yang akurat tentang isi artikel."
pubDate: "2026-11-22T20:15:35+08:00"
heroImage: "https://raw.githubusercontent.com/dhikajalaksurenbali/blog-burung/main/public/images/2026/11/22/nama-file.webp"
---
```

Setelah frontmatter, letakkan isi tulisan Om Dhika dalam Markdown. Ganti tanggal contoh dengan waktu aktual saat artikel benar-benar disiapkan untuk diposting. Jangan memakai tanggal contoh secara mentah.

Aturan tanggal:
- Gunakan waktu lokal Bali/WITA (UTC+08:00) lengkap sampai detik, dengan format ISO 8601. Contoh format: `2026-11-22T20:15:35+08:00`.
- Jangan mengarang waktu, memakai UTC tanpa offset, atau menghilangkan offset zona waktu.
- Urutan artikel di halaman depan dan halaman daftar memakai waktu lengkap `pubDate`; artikel terbaru tampil paling atas.
- Pengunjung hanya melihat tanggal, bulan, dan tahun. Jam, menit, detik, dan offset tidak ditampilkan.
- Artikel lama yang hanya menggunakan tanggal seperti `Nov 21 2026` tetap valid.

Aturan slug/nama file:
- Buat nama file dari judul dengan huruf kecil dan tanda hubung, singkat tetapi jelas.
- Gunakan ekstensi `.md`.
- Periksa dulu apakah nama file sudah dipakai. Jangan menimpa artikel yang sudah ada.
- Jangan membuat file artikel sebelum teks dan URL foto (jika memakai foto) sudah jelas.

Gambar:
- Gunakan hanya URL/path yang benar-benar diberikan oleh Worker atau sudah diverifikasi ada di GitHub.
- Gambar diunggah terpisah ke `public/images/YYYY/MM/DD/`; file Markdown artikel tetap berada di `src/content/blog/`.
- URL publik yang biasa dikembalikan Worker berbentuk `https://raw.githubusercontent.com/dhikajalaksurenbali/blog-burung/main/public/images/...`.
- Jangan menebak nama file, tanggal folder, atau URL gambar.
- Walaupun `heroImage` opsional di skema, halaman daftar saat ini memakai `post.data.heroImage` sebagai sumber gambar kartu. Karena itu, utamakan foto utama yang sudah diunggah dan URL-nya terverifikasi untuk setiap postingan. Jika tidak ada foto, jangan asal mengisi URL; periksa dulu perilaku tampilan atau tanyakan keputusan Om Dhika.

## 5. Urutan artikel dan tampilan tanggal

Perbaikan pengurutan telah diterapkan pada:
- `src/pages/index.astro`
- `src/pages/burung/index.astro`

Keduanya mengurutkan dengan membandingkan `pubDate.getTime()`, bukan hanya tanggal kalender. Pertahankan perilaku ini. Jangan mengembalikan pengurutan ke perbandingan string/tanggal saja.

Komponen tanggal pengunjung menggunakan `FormattedDate`; tampilan tetap tanggal/bulan/tahun saja. Jangan mengubah format publik tanpa permintaan Om Dhika.

## 6. Worker upload gambar

Worker khusus upload gambar:
- Nama Worker: `blog-burung-image-upload`
- URL dasar: https://blog-burung-image-upload.dhikajsbali.workers.dev
- Endpoint membuat sesi: `POST /create-session`
- Halaman upload: `GET /?t=<token-sesi>`
- Endpoint menerima file: `POST /upload?t=<token-sesi>`
- Penyimpanan sesi: Cloudflare KV namespace `blog-burung-upload-sessions`
- Tujuan upload: repository `dhikajalaksurenbali/blog-burung`, folder `public/images/YYYY/MM/DD/`
- Format hasil: WebP
- Maksimal file per sesi: 5
- Masa berlaku sesi: 30 menit
- Batas ukuran yang diterapkan Worker: 1.5 MB per file setelah pemrosesan browser.
- Browser mengubah ukuran foto dengan sisi terpanjang maksimal 1600 px dan mengonversi ke WebP.

Binding/secret Worker:
- `SESSIONS`: binding KV.
- `GITHUB_TOKEN`: secret untuk menulis gambar ke GitHub.
- `UPLOAD_API_KEY`: secret yang diperiksa endpoint `POST /create-session` melalui header `Authorization: Bearer <UPLOAD_API_KEY>`.

Keamanan:
- Jangan meminta Om Dhika mengirim token atau secret lewat chat.
- Jangan menampilkan atau menyalin nilai rahasia ke file repository, artikel, atau log publik.
- Mengetahui nama secret tidak berarti GPT mempunyai nilainya.
- Agar GPT dapat memanggil `POST /create-session`, pemanggil harus memiliki kredensial yang cocok melalui konfigurasi aman. Jika belum tersedia, jangan mengklaim sesi upload dapat dibuat otomatis.
- Contoh permintaan sesi: method `POST`, URL `https://blog-burung-image-upload.dhikajsbali.workers.dev/create-session`, header `Authorization: Bearer <key yang sama dengan Secret UPLOAD_API_KEY>`. Jangan menuliskan nilai key ke dokumen ini.
- Respons sukses diharapkan memuat `upload_url`, `expires_in`, dan `max_files`.
- Halaman upload yang terbuka bukan bukti file berhasil diunggah. Verifikasi file WebP di GitHub.

## 7. Status repository dan kondisi postingan saat ini

Pembaruan terakhir pada 22 November 2026:
- Dua belas file artikel lama di `src/content/blog/` telah dihapus atas persetujuan Om Dhika agar postingan berikutnya dimulai dengan konten yang benar-benar serius.
- Penghapusan hanya menyasar 12 file artikel Markdown tersebut.
- Foto di `public/images/`, halaman website, kode/program, formulir upload gambar, dan pengaturan Cloudflare tidak termasuk dalam penghapusan.
- Halaman daftar `/burung/` dan halaman depan tetap ada. Saat koleksi artikel kosong, halaman depan memiliki tampilan pesan bahwa update akan muncul setelah postingan ditambahkan.
- Setelah artikel baru dibuat dan deployment selesai, artikel akan muncul otomatis di `/burung/`; tiga artikel terbaru juga ditampilkan di halaman depan.
- Tidak perlu membuat halaman daftar baru untuk setiap postingan. Cukup tambahkan file Markdown artikel ke `src/content/blog/`.

Daftar 12 artikel yang dihapus:
1. `burung-berkualitas-pilihan-pecinta-burung.md`
2. `burung-jalak-bali-anakan.md`
3. `burung-jalak-hongkong-anakan-stok-terbaru.md`
4. `demo-kebersihan-kandang-burung.md`
5. `demo-membantu-burung-beradaptasi.md`
6. `demo-mengenal-pakan-burung-jalak.md`
7. `demo-panduan-awal-merawat-jalak-bali.md`
8. `demo-rutinitas-harian-perawatan-burung.md`
9. `jalak-hybrid-suren-x-putih-ready.md`
10. `jalak-putih-atau-jalak-nusa-anakan.md`
11. `jalak-suren-albino-mata-merah.md`
12. `tes-pertama-posting.md`

Catatan verifikasi:
- Operasi penghapusan 12 file di GitHub branch `main` berhasil dikonfirmasi oleh respons commit untuk masing-masing file.
- Keberhasilan deployment Cloudflare setelah penghapusan belum diverifikasi dalam pekerjaan tersebut.
- Uji end-to-end upload dan posting otomatis dari GPT sampai website live belum dinyatakan berhasil.
- Jangan menganggap kondisi deployment saat ini berhasil/gagal tanpa memeriksa status terbaru.

## 8. Aturan kerja GitHub

- Gunakan repository `dhikajalaksurenbali/blog-burung`, branch `main`.
- Sebelum posting, periksa file yang sudah ada di `src/content/blog/` dan konfigurasi Astro yang relevan.
- Pastikan struktur frontmatter sesuai `src/content.config.ts`.
- Jangan mengubah file lain untuk tugas posting artikel biasa.
- Jangan melakukan force push.
- Jangan menghapus/mengganti artikel atau foto tanpa izin eksplisit.
- Setelah commit, catat commit SHA dan periksa kembali file yang dibuat.
- Periksa status build/deployment jika akses tersedia. Laporkan hanya yang benar-benar teramati.

## 9. Batas keamanan Cloudflare

Worker upload terpisah dari Worker produksi.
- Worker upload yang relevan: `blog-burung-image-upload`.
- Jangan mengubah, menghapus, atau menimpa Worker produksi bernama `blog-burung`.
- Worker `koleksi-burung` juga bukan bagian dari tugas upload artikel.
- Jangan membuat perubahan Cloudflare di luar kebutuhan yang diminta Om Dhika.

---

**Instruksi untuk GPT berikutnya:** baca dokumen ini terlebih dahulu. Jika Om Dhika meminta membuat postingan, ikuti alur pada Bagian 3, pertahankan isi yang dikirim, gunakan timestamp WITA aktual, simpan artikel di `src/content/blog/`, dan jangan mengklaim tayang sebelum deployment diverifikasi.