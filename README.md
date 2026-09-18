# OfficeCraft Studio

Aplikasi web untuk belajar **Microsoft Word, Excel, dan PowerPoint** lewat modul singkat, latihan interaktif langsung di browser, dan kuis berjenjang — dibuat untuk siswa.

## Fitur

- **Login & Daftar** — siswa mendaftar sendiri; guru/admin mendaftar dengan kode admin.
- **18 modul belajar** (6 modul Word, 6 modul Excel, 6 modul PowerPoint), masing-masing dipecah menjadi **5 level** berjenjang yang harus diselesaikan berurutan.
- **90 level** total, masing-masing punya materi, latihan interaktif, dan kuis sendiri (180 soal kuis keseluruhan).
- **Latihan interaktif** sesuai topik: editor teks dengan format Word, tabel & rumus Excel yang terhitung otomatis, canvas slide PowerPoint dengan tema/transisi, dan lainnya.
- **Studio Latihan** — tempat berlatih bebas di ketiga aplikasi, hasilnya bisa disimpan.
- **Galeri Karya** — menyimpan semua hasil latihan siswa.
- **Dashboard Admin** — guru bisa memantau XP, level selesai, persentase pemahaman, dan jumlah karya setiap siswa.
- **Sistem XP** — siswa mendapat poin setiap menyelesaikan level, kuis, atau menyimpan karya.

## Struktur File

Proyek ini adalah **satu file HTML mandiri** (`index.html`) — semua CSS dan JavaScript ada di dalamnya, tidak perlu proses build atau instalasi apa pun.

## Menjalankan Secara Online (Firebase)

Aplikasi ini menyimpan data (akun, progres, karya) secara online lewat **Firebase Firestore**, sehingga siswa bisa login dan melanjutkan progres dari perangkat berbeda.

### Langkah setup (sekali saja)

1. Buka [console.firebase.google.com](https://console.firebase.google.com), klik **Add project**, beri nama (misalnya `OfficeCraft-Studio`), ikuti proses pembuatan project (gratis).
2. Di sidebar project, buka **Build > Firestore Database > Create database**. Pilih **Start in test mode** untuk memulai (bisa diperketat nanti lewat Firestore Rules).
3. Buka **Project settings** (ikon gerigi) → scroll ke bagian **Your apps** → klik ikon web (`</>`) → beri nama app. Firebase akan menampilkan objek `firebaseConfig` berisi `apiKey`, `projectId`, dan lainnya.
4. Buka `index.html`, cari bagian berikut di dekat awal tag `<script>`:

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

5. Simpan file, lalu unggah ke GitHub (lihat bagian di bawah).

Jika `firebaseConfig` belum diisi, aplikasi akan menampilkan peringatan di halaman login dan login/daftar tidak akan berfungsi.

> **Catatan keamanan:** mode "test mode" pada Firestore membuat data bisa dibaca/ditulis siapa saja yang tahu alamat project selama masa uji coba (biasanya 30 hari). Untuk pemakaian jangka panjang, atur **Firestore Rules** agar lebih aman, atau tanyakan ke pengembang/gurumu yang paham Firebase.

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

## Lisensi

Bebas digunakan dan dimodifikasi untuk keperluan pembelajaran di sekolah.
