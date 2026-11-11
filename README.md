# Tutorial Linux Home: Debian XFCE di Android

Panduan ini memasang desktop Debian XFCE di Android tanpa root dan tanpa custom
ROM. Linux berjalan di userspace melalui Termux + `proot-distro`; kernel dan
Android tetap berjalan seperti biasa. Termux:X11 menampilkan desktop grafis.
Ini bukan dual-boot atau Linux yang menggantikan sistem operasi HP.

## Yang perlu disiapkan

- HP Android 8.0 atau lebih baru.
- Koneksi internet stabil dan ruang kosong beberapa GB. Debian dan XFCE perlu
  mengunduh banyak paket.
- APK Linux Home: `app/build/outputs/apk/debug/app-debug.apk`.
- Termux dari [F-Droid](https://f-droid.org/packages/com.termux/) atau
  [rilis resmi GitHub](https://github.com/termux/termux-app/releases).
- Aplikasi Android Termux:X11 dari
  [rilis resmi Termux:X11](https://github.com/termux/termux-x11/releases/tag/nightly).

Jangan mencampur Termux dengan add-on Termux dari sumber/tanda tangan berbeda.
Untuk Termux:X11, pilih APK biasa bernama
`termux-x11-universal-debug.apk`; jangan pilih varian `sharedUid` untuk
penyiapan ini.

## Langkah instalasi

### 1. Pasang dan siapkan Termux

1. Pasang Termux dari F-Droid atau GitHub resmi, kemudian buka Termux sekali.
2. Tunggu proses persiapan awal selesai sampai prompt terminal muncul.
3. Jangan pasang Termux dari Play Store versi lama atau mencampur instalasi dari
   sumber yang berbeda.

### 2. Pasang Linux Home

1. Pasang `app-debug.apk` di atas pada HP. Jika Android memperingatkan sumber
   tidak dikenal, izinkan instalasi untuk aplikasi file/browser yang sedang
   dipakai, lalu lanjutkan.
2. Buka **Linux Home**. Tidak wajib menjadikannya aplikasi Home/default launcher
   Android untuk menjalankan desktop Linux.

### 3. Instal Debian dan XFCE

1. Di Linux Home, tekan **Siapkan Debian + XFCE**, lalu konfirmasi.
2. Linux Home akan meminta Termux menjalankan perintah instalasi. Jika Termux
   terbuka, biarkan prosesnya berjalan sampai prompt kembali. Jangan tutup
   Termux atau matikan layar selama instalasi.
3. Jika diminta, buka pengaturan Termux dan aktifkan **Allow external apps**.
   Kembali ke Linux Home dan tekan **Siapkan Debian + XFCE** lagi bila perlu.
4. Instalasi pertama membutuhkan internet dan bisa memakan waktu cukup lama.
   Jangan jalankan tombol setup berulang kali saat perintah masih berjalan.

Perintah yang dipasang oleh langkah ini setara dengan:

```sh
pkg update -y
pkg install -y x11-repo
pkg update -y
pkg install -y termux-x11-nightly proot-distro
proot-distro install debian
proot-distro login debian --shared-tmp -- /bin/bash -lc \
  'apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y xfce4 dbus-x11'
```

### 4. Pasang aplikasi Termux:X11

1. Kembali ke Linux Home, tekan **Pasang aplikasi Termux:X11**.
2. Di halaman rilis resmi, unduh APK biasa bernama
   `termux-x11-universal-debug.apk` (bukan APK `sharedUid`).
3. Buka file yang diunduh dan pasang. Jika Android meminta izin instalasi dari
   browser/file manager, izinkan untuk aplikasi tersebut.
4. Kembali ke Linux Home. Status harus menunjukkan Termux dan Termux:X11
   terpasang.

Termux:X11 terdiri dari dua bagian: APK Android yang baru dipasang dan paket
`termux-x11-nightly` yang diinstal ke Termux pada langkah setup.

### 5. Jalankan desktop

1. Tekan **Jalankan desktop XFCE**.
2. Saat pertama kali, Android mungkin meminta izin Linux Home untuk menjalankan
   perintah di Termux. Izinkan, lalu ulangi tombol desktop jika diperlukan.
3. Jendela Termux:X11 akan terbuka dan menampilkan desktop Debian XFCE.
4. Untuk mengetik lebih nyaman, gunakan keyboard/mouse Bluetooth. Keduanya
   opsional.
5. Untuk keluar, gunakan menu **Log out** XFCE atau tutup sesi Termux:X11.

Tombol **Buka shell Debian** membuka terminal Debian tanpa desktop grafis.

## Jika ingin Linux Home menjadi launcher utama

Ini opsional dan tidak diperlukan untuk membuka desktop. Buka pengaturan
Android **Aplikasi default > Aplikasi Home/Digital assistant & Home app**,
pilih **Linux Home**. Nama menu bisa berbeda menurut merek HP. Untuk kembali ke
launcher sebelumnya, ubah pilihan Home app di menu yang sama.

## Troubleshooting

- **Termux belum terpasang:** pasang Termux dari salah satu sumber resmi di
  bagian persiapan, buka sekali, lalu kembali ke Linux Home.
- **Perintah tidak jalan / izin Termux:** buka Termux, aktifkan
  **Settings > Allow external apps**, lalu coba lagi. Pastikan juga Android
  mengizinkan Linux Home berjalan di latar depan saat meminta perintah.
- **`proot-distro` atau `termux-x11-nightly` tidak ditemukan:** buka Termux dan
  jalankan satu per satu:

  ```sh
  pkg update -y
  pkg install -y x11-repo
  pkg update -y
  pkg install -y proot-distro termux-x11-nightly
  ```

  Setelah selesai, tekan **Siapkan Debian + XFCE** lagi.
- **Desktop hitam atau tidak muncul:** pastikan aplikasi Termux:X11 dan paket
  `termux-x11-nightly` sama-sama terpasang. Tutup jendela Termux:X11 yang lama,
  lalu tekan **Jalankan desktop XFCE** lagi.
- **Debian sudah terpasang tetapi setup terhenti saat memasang XFCE:** buka
  shell Debian dari Linux Home lalu jalankan:

  ```sh
  apt update
  DEBIAN_FRONTEND=noninteractive apt install -y xfce4 dbus-x11
  ```

- **Penyimpanan penuh / unduhan gagal:** kosongkan ruang, pastikan internet
  stabil, buka Termux, lalu jalankan kembali setup. Jangan hapus data Termux
  kecuali memang ingin menghapus Debian dan seluruh data Linux di dalamnya.

## Build APK dari source

Memerlukan JDK 17 dan Android SDK Platform 35. Dari folder project:

```sh
./gradlew assembleDebug
```

APK hasil build ada di `app/build/outputs/apk/debug/app-debug.apk`. Ini APK
debug untuk instalasi/pengujian, bukan release yang ditandatangani untuk
distribusi publik.
