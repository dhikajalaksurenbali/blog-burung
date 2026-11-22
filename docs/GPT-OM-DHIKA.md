# Panduan Serah Terima GPT — Blog Burung

Dokumen ini adalah catatan konteks untuk GPT yang membantu mengelola situs Blog Burung. Baca dokumen ini sebelum melakukan perubahan pada repository atau layanan Cloudflare.

## 1. Identitas proyek

- Repository GitHub: https://github.com/dhikajalaksurenbali/blog-burung
- Pemilik/akun GitHub: `dhikajalaksurenbali88`
- Branch utama: `main`
- Situs dibuat dengan Astro dan dipublikasikan melalui Cloudflare.
- Konten artikel Astro menggunakan Markdown. Aset gambar publik disimpan di folder `public/images/`.

## 2. Tujuan GPT

GPT bertugas membantu pemilik menerbitkan artikel ke situs Blog Burung melalui percakapan.

Alur yang diinginkan:
1. Pemilik memberikan judul dan isi tulisan.
2. Jika artikel membutuhkan foto, GPT memberikan tautan formulir upload gambar.
3. Pemilik mengunggah foto melalui formulir tersebut.
4. GPT mengambil URL gambar yang berhasil diunggah.
5. GPT membuat file artikel Markdown di lokasi dan format yang sesuai dengan struktur repo.
6. Cloudflare menjalankan build/deploy dari perubahan GitHub.
7. GPT melaporkan hasil berdasarkan status yang benar-benar dapat diverifikasi.

**Penting:** pertahankan judul dan isi tulisan persis seperti yang diberikan pemilik. Jangan meringkas, menulis ulang, memperbaiki gaya, atau mengubah maksud tanpa diminta. Jangan mengarang URL gambar atau menyatakan artikel sudah terbit sebelum commit dan deployment terverifikasi.

## 3. Worker upload gambar

Worker khusus upload gambar telah dibuat di akun Cloudflare milik Dhika.

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
- Batas ukuran yang diterapkan Worker saat ini: 1.5 MB per file setelah pemrosesan di browser.
- Browser mengubah ukuran foto dengan sisi terpanjang maksimal 1600 px dan mengonversi ke WebP.

Contoh URL publik gambar yang dikembalikan Worker berbentuk:
`https://raw.githubusercontent.com/dhikajalaksurenbali/blog-burung/main/public/images/...`

Jangan menganggap upload sudah berhasil hanya karena halaman upload terbuka. Periksa respons upload dan pastikan file benar-benar ada di GitHub.

## 4. Autentikasi dan rahasia

Worker memiliki binding/secret berikut:
- `SESSIONS`: binding ke KV untuk sesi upload.
- `GITHUB_TOKEN`: Secret yang digunakan Worker untuk menulis file gambar ke GitHub.
- `UPLOAD_API_KEY`: Secret yang diperiksa oleh endpoint `POST /create-session` melalui header `Authorization: Bearer <UPLOAD_API_KEY>`.

**Keamanan:**
- Jangan meminta pemilik mengirim token atau secret melalui percakapan.
- Jangan menampilkan, menyalin ke artikel, commit, log publik, atau file Markdown nilai rahasia apa pun.
- Nilai Secret Cloudflare tidak dapat dibaca kembali oleh GPT melalui daftar nama secret.
- Agar GPT dapat memanggil `POST /create-session`, pemilik harus mengatur kredensial yang sama pada konfigurasi autentikasi Action/API yang memanggil endpoint tersebut. Mengetahui nama `UPLOAD_API_KEY` saja tidak memberikan akses.
- Jika nilai key yang cocok belum tersedia di konfigurasi pemanggil, jangan mengklaim integrasi upload sudah siap. Minta pemilik mengatur ulang/merotasi key melalui konfigurasi aman Cloudflare dan pemanggil, tanpa menempelkan nilainya ke chat.

Contoh permintaan sesi (nilai key tidak ditulis di dokumen ini):
- Method: `POST`
- URL: `https://blog-burung-image-upload.dhikajsbali.workers.dev/create-session`
- Header: `Authorization: Bearer <key yang sama dengan Secret UPLOAD_API_KEY>`

Respons sukses diharapkan berisi `upload_url`, `expires_in`, dan `max_files`. Berikan `upload_url` kepada pemilik untuk membuka halaman upload.

## 5. Aturan kerja repository

- Gunakan repository `dhikajalaksurenbali/blog-burung` dan branch `main`.
- Sebelum membuat artikel, periksa struktur konten dan contoh artikel yang sudah ada agar format frontmatter, slug, dan lokasi file konsisten.
- Jangan menebak lokasi artikel. Periksa file dan konfigurasi Astro yang ada.
- Simpan gambar di `public/images/`; gunakan URL/path gambar yang benar-benar dikembalikan setelah upload.
- Jangan mengubah file konfigurasi, theme, CSS, dependensi, atau struktur situs untuk tugas posting artikel biasa.
- Jangan menghapus atau mengganti artikel/gambar lama kecuali diminta secara eksplisit.
- Jangan melakukan force push.
- Jika build/deploy gagal, jelaskan kegagalan yang teramati; jangan menyatakan publikasi sukses.

## 6. Batas keamanan Cloudflare

Worker upload ini terpisah dari Worker produksi.

**Jangan mengubah, menghapus, atau menimpa Worker produksi bernama `blog-burung`.** Worker lain bernama `koleksi-burung` juga bukan bagian dari tugas upload artikel ini.

Worker yang boleh diperiksa untuk alur upload adalah `blog-burung-image-upload`. Jangan membuat perubahan Cloudflare di luar kebutuhan yang diminta pemilik.

## 7. Status integrasi saat dokumen ini dibuat

- Worker upload: sudah dibuat dan kode telah dipasang.
- KV namespace dan binding: sudah dibuat/dipasang.
- Secret bernama `GITHUB_TOKEN` dan `UPLOAD_API_KEY`: terdeteksi pada Worker.
- Nilai rahasia tidak dicantumkan di repository.
- **Uji end-to-end upload sampai file masuk GitHub belum dinyatakan berhasil.**
- **Uji alur lengkap dari GPT, upload gambar, pembuatan artikel, hingga deployment Cloudflare juga belum dinyatakan berhasil.**

Langkah berikutnya untuk GPT:
1. Periksa struktur repo dan contoh artikel.
2. Pastikan Action/API pemanggil sudah memiliki autentikasi yang cocok untuk `UPLOAD_API_KEY` tanpa meminta nilainya di chat.
3. Uji pembuatan sesi dan pembukaan halaman upload.
4. Lakukan satu upload uji dan verifikasi file WebP muncul di folder tujuan GitHub.
5. Setelah upload teruji, uji penerbitan satu artikel dan verifikasi deployment Cloudflare.
6. Catat hasil uji yang sebenarnya; perbarui bagian status dokumen ini bila ada perubahan penting.

---
Dokumen operasional untuk membantu GPT melanjutkan proyek Blog Burung. Jangan menyimpan token, password, atau nilai secret di file ini.
