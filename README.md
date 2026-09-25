# OfficeCraft Studio 2.0

Buku belajar digital interaktif untuk **Microsoft Word, Excel, dan PowerPoint** — bertahap, bergamifikasi, dan dibuat untuk siswa SD kelas tinggi, SMP, hingga SMPLB/SMALB.

## Fitur

- **Login & Daftar** aman lewat **Firebase Authentication** — siswa mendaftar sendiri; guru/admin mendaftar dengan kode admin.
- **19 modul belajar** (7 modul Word, 6 modul Excel, 6 modul PowerPoint), masing-masing dipecah menjadi **5 level** berjenjang.
- **95 level** total dengan status 4 tingkat: 🔒 Terkunci, ▶️ Belum dimulai, 🟡 Sedang belajar, 🟢 Selesai — 190 soal kuis keseluruhan.
- **Beranda** dengan kartu "Lanjutkan Belajar" dan roadmap visual perjalanan Word → Excel → PowerPoint.
- **Profil Saya** — level pengguna, total XP, streak belajar harian, progress bar per aplikasi.
- **Office Studio berbasis proyek** — ribbon fungsional ala Office asli (Home/Insert/Layout/dst) per aplikasi; **15 proyek terarah** (5 per aplikasi: Biodata Saya, Surat Sederhana, Jadwal Pelajaran, Daftar Kegiatan, Poster Sederhana di Word; Tabel Nilai, Daftar Belanja, Catatan Keuangan, Data Barang, Rekap Nilai Olahraga di Excel; Presentasi Tentang Saya, Bagian Tumbuhan, Sekolahku, Budaya Indonesia, Hobi Saya di PowerPoint) lengkap dengan tujuan, langkah, dan checklist — atau Latihan Bebas seperti biasa.
- **Karya Saya** — portofolio dengan status 🟢 Selesai / 🟡 Sedang Dikerjakan; menyelesaikan proyek (checklist lengkap) memberi bonus +50 XP.
- **Mode ABK** — Baca / Visual / Audio / Praktik, bisa dipilih satu, beberapa, atau semua lewat menu terpisah.
- **Mode gelap/terang**.
- **Dashboard Admin/Guru** — breakdown progres per aplikasi (Word/Excel/PowerPoint), badge tiap siswa, filter (Semua/per aplikasi/Selesai Semua/Belum Selesai), dan jumlah karya.
- **Sistem badge & XP dengan bahasa positif** — 11 badge otomatis (Word Pemula, Excel Explorer, PowerPoint Creator, Data Explorer, Presentation Maker, Office Beginner, Office Creator, Kolektor Proyek, Bendahara Cilik, Pencerita Budaya, Office Master), muncul sebagai notifikasi melayang saat didapat; umpan balik kuis pakai bahasa membangun ("Hebat!"/"Coba lagi", bukan "Salah").

## Struktur File

Proyek ini adalah **satu file HTML mandiri** (`index.html`) — semua CSS dan JavaScript ada di dalamnya, tidak perlu proses build atau instalasi apa pun.

## Menjalankan Secara Online (Firebase)

Aplikasi ini memakai **Firebase Authentication** untuk login (password dikelola aman oleh Firebase, tidak lagi disimpan sebagai teks biasa) dan **Firebase Firestore** untuk data akun, progres, XP, dan karya siswa.

### Langkah setup (sekali saja)

1. Buka [console.firebase.google.com](https://console.firebase.google.com), klik **Add project**, beri nama (misalnya `OfficeCraft-Studio`), ikuti proses pembuatan project (gratis).
2. Di sidebar project, buka **Build > Authentication > Get started**, buka tab **Sign-in method**, aktifkan provider **Email/Password**, klik **Save**.

   > Catatan: siswa tetap login memakai **username** biasa, bukan email. Di balik layar, aplikasi otomatis mengubah username jadi `username@officecraft.local` supaya bisa dipakai Firebase Authentication — siswa tidak perlu punya email asli.

3. Di sidebar project, buka **Build > Firestore Database > Create database**. Pilih **Start in test mode** untuk memulai.
4. Setelah database dibuat, buka tab **Rules**, hapus isinya, lalu ganti dengan ini:

   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /users/{userId} {
         allow read: if request.auth != null;
         allow write: if request.auth != null && request.auth.uid == userId;
       }
     }
   }
   ```

   Klik **Publish**. Aturan ini mengizinkan semua pengguna yang sudah login untuk membaca data (dibutuhkan Dashboard Admin untuk melihat daftar siswa), tapi setiap pengguna hanya bisa mengubah datanya sendiri — jauh lebih aman dibanding versi sebelumnya karena sudah memanfaatkan status login dari Firebase Authentication.
5. Buka **Project settings** (ikon gerigi) → scroll ke bagian **Your apps** → klik ikon web (`</>`) → beri nama app. Firebase akan menampilkan objek `firebaseConfig` berisi `apiKey`, `projectId`, dan lainnya.
6. Buka `index.html`, cari bagian berikut di dekat awal tag `<script>`:

   ```js
   const firebaseConfig = {
     apiKey: "GANTI_DENGAN_API_KEY_FIREBASE_MU",
     authDomain: "GANTI_DENGAN_PROJECT_ID.firebaseapp.com",
     projectId: "GANTI_DENGAN_PROJECT_ID",
     storageBucket: "GANTI_DENGAN_PROJECT_ID.appspot.com",
     messagingSenderId: "GANTI_DENGAN_SENDER_ID",
     appId: "GANTI_DENGAN_APP_ID"
   };
   ```

   Ganti semua nilai `"GANTI_DENGAN_..."` dengan nilai asli dari Firebase Console.

7. Simpan file, lalu unggah ke GitHub (lihat bagian di bawah).

Jika `firebaseConfig` belum diisi, aplikasi akan menampilkan peringatan di halaman login dan login/daftar tidak akan berfungsi. Jika provider **Email/Password** belum diaktifkan di langkah 2, pendaftaran akun baru akan gagal dengan pesan error.

## Deploy ke GitHub Pages

1. Buat repository baru di GitHub, misalnya `officecraft-studio`.
2. Upload file `index.html` (dan `README.md` ini) ke repository tersebut.
3. Buka **Settings → Pages** pada repository.
4. Di bagian **Source**, pilih branch `main` dan folder `/root`, lalu klik **Save**.
5. Tunggu 1–2 menit, situs akan aktif di:
   `https://<username-github>.github.io/officecraft-studio/`

## Akun Admin

Saat mendaftar, pilih peran **Guru / Admin** dan masukkan kode admin berikut:

```
OFFICE2026
```

Kode ini bisa diganti langsung di kode HTML pada bagian `const ADMIN_CODE = "OFFICE2026";` jika ingin diamankan lebih lanjut.

## Roadmap Pengembangan

Versi ini adalah **Fase 1** dari rencana pengembangan OfficeCraft Studio 2.0:

- ✅ **Fase 1** — Firebase Authentication, identitas visual baru, halaman Profil, roadmap Beranda, status level 4 tingkat, streak harian.
- ✅ **Fase 2** — Data materi didokumentasikan sebagai satu blok "COURSES" yang mudah ditambah, plus modul baru Word: **Edit Cepat (Copy, Cut, Paste, Undo, Redo)**.
- ✅ **Fase 3** — Sistem badge otomatis (7 badge, tersimpan di Firestore), notifikasi badge melayang, bahasa kuis & hasil belajar dibuat positif.
- ✅ **Fase 4** — Office Studio berbasis proyek terarah (15 proyek: 5 per aplikasi) dengan tujuan/langkah/checklist, dan Karya Saya jadi portofolio berstatus.
- ✅ **Fase 5** — Dashboard Guru diperluas: kolom progres per aplikasi (Word/Excel/PowerPoint), kolom badge, filter siswa (per aplikasi / selesai semua / belum selesai).

Kelima fase dari rencana awal OfficeCraft Studio 2.0 sudah selesai. Pengembangan lanjutan (proyek baru, badge baru, dsb) tinggal ditambahkan lewat blok data yang sudah terdokumentasi di dalam kode.

## Lisensi

Bebas digunakan dan dimodifikasi untuk keperluan pembelajaran di sekolah.

