# Refleksi XI RPL — 50 Soal, 30 Menit

Website statis untuk GitHub Pages dengan pencatatan hasil ke Google Sheets melalui Google Apps Script.

## Isi ujian

- 15 soal Microsoft Word
- 15 soal Microsoft Excel
- 20 soal logika Basis Data (Medium, Hard, Expert)
- 50 soal pilihan ganda, 4 opsi
- Timer 30 menit, navigasi soal, penilaian, pembahasan, dan cetak hasil

## A. Siapkan Google Sheets

1. Buat Google Spreadsheet baru, misalnya `REKAP REFLEKSI XI RPL`.
2. Ambil Spreadsheet ID dari URL: `https://docs.google.com/spreadsheets/d/SPREADSHEET_ID/edit`.
3. Buka [Google Apps Script](https://script.google.com/) dan buat project baru.
4. Buka file `apps-script/Code.gs` di repository ini, salin seluruh kodenya ke editor Apps Script.
5. Pada Apps Script, ganti `TEMPEL_ID_SPREADSHEET_DI_SINI` dengan ID spreadsheet Anda.
6. Simpan, pilih fungsi `setupSheets`, lalu klik **Run**. Berikan izin yang diminta.
7. Pastikan spreadsheet memiliki tab `RESULTS` dan `ANSWERS`.

## B. Deploy Apps Script sebagai Web App

1. Di Apps Script klik **Deploy → New deployment**.
2. Pilih jenis **Web app**.
3. **Execute as:** Me.
4. **Who has access:** Anyone (jika kebijakan akun Google Workspace mengizinkan).
5. Klik **Deploy**, selesaikan otorisasi, lalu salin URL Web App yang berakhiran `/exec`.
6. Jika organisasi membatasi akses `Anyone`, gunakan akun Google yang mengizinkan deployment publik atau minta administrator mengatur kebijakannya.

## C. Hubungkan endpoint ke website

1. Buka file `config.js`.
2. Ganti `TEMPEL_URL_WEB_APP_APPS_SCRIPT_DI_SINI` dengan URL `/exec` dari langkah B.
3. Jangan menambahkan tanda kutip tambahan di dalam URL. Pertahankan tanda kutip yang sudah ada.
4. Simpan file.

Contoh format (gunakan URL milik project Anda sendiri):

```js
window.APP_CONFIG = {
  GOOGLE_SHEETS_WEB_APP_URL: "https://script.google.com/macros/s/ID_DEPLOYMENT/exec"
};
```

## D. Upload ke GitHub dan aktifkan GitHub Pages

1. Buat repository baru di GitHub, misalnya `refleksi-xi-rpl`.
2. Upload seluruh isi folder ini ke **root** repository. Pastikan `index.html`, `config.js`, `questions.js`, `script.js`, dan `style.css` berada di root, bukan di dalam subfolder lain.
3. Commit perubahan ke branch `main`.
4. Buka **Settings → Pages** pada repository.
5. Pada **Build and deployment**, pilih **Deploy from a branch**.
6. Pilih branch `main` dan folder `/(root)`, lalu klik **Save**.
7. Tunggu proses deployment selesai. Buka URL GitHub Pages yang ditampilkan pada halaman Pages.

> File `apps-script/Code.gs` disimpan sebagai referensi sumber backend. Website tidak memuat file `.gs` dari GitHub Pages; kode tersebut harus ditempel dan di-deploy melalui Google Apps Script.

## E. Tes integrasi

1. Buka website GitHub Pages menggunakan URL HTTPS.
2. Isi nama, kelas, dan nomor absen, lalu klik **MULAI REFLEKSI**.
3. Kerjakan ujian uji coba dan tekan **SELESAIKAN REFLEKSI**.
4. Buka Google Spreadsheet dan periksa:
   - `RESULTS`: satu baris ringkasan untuk setiap pengiriman ujian.
   - `ANSWERS`: 50 baris detail jawaban untuk satu pengiriman.
5. Jika belum tercatat, cek URL di `config.js`, izin deployment Apps Script, dan menu **Executions** di Apps Script.

## Cara update website setelah perubahan

Edit file terkait, lalu commit dan push ke branch `main`. GitHub Pages akan menerbitkan versi baru secara otomatis setelah proses deployment.

## Catatan teknis dan keamanan

- Browser mengirim data dengan `mode: "no-cors"`. Karena batasan browser, halaman tidak dapat membaca balasan dari Apps Script. Pesan di halaman berarti permintaan dikirim; **selalu verifikasi data di Google Sheets**.
- Penghitungan nilai dan kunci jawaban pada versi ini berada di JavaScript publik. Siswa yang memahami developer tools dapat memanipulasi data sebelum pengiriman. Cocok untuk refleksi/latihan, bukan ujian berisiko tinggi.
- Untuk ujian resmi, gunakan autentikasi siswa, simpan kunci jawaban hanya di backend, hitung nilai di Apps Script, dan validasi waktu serta identitas di server.
- Jangan menaruh password, token rahasia, atau data sensitif di `config.js` maupun file frontend publik.
