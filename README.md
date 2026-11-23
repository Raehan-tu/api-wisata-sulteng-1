# API Wisata Sulawesi Tengah

REST API berbasis Node.js untuk layanan data destinasi wisata Sulawesi Tengah dengan dukungan *API key authentication* dan *API versioning* (melalui *path prefix* maupun *request header*).

## Prasyarat

* **Node.js** (disarankan versi 20.6.0 ke atas untuk dukungan `--watch` dan `--env-file`)

* **Git**

## Cara Menjalankan Project dari Repository Baru

1. **Clone repository**

   ```
   git clone https://github.com/Raehan-tu/api-wisata-sulteng-1.git
   cd api-wisata-sulteng-1
   
   ```

2. **Install dependensi**

   ```
   npm install
   
   ```

3. **Buat file konfigurasi dan data lokal yang belum ada di repository**
   Lihat bagian **File yang Harus Dibuat Sendiri** di bawah ini.

4. **Jalankan server**

   ```
   node --watch --env-file=.env server.js
   
   ```

## File yang Harus Dibuat Sendiri

Karena file konfigurasi sensitif dan data kunci akses tidak disertakan di dalam repository, buat file berikut secara lokal sebelum menjalankan server:

1. **`.env`** (di root folder project)
   Berisi konfigurasi rahasia lingkungan server, seperti *token secret*:

   ```
   TOKEN_SECRET=contoh-token-secret-anda
   
   ```

2. **`data/api-keys.json`**
   Berisi daftar API key yang valid untuk autentikasi melalui header `x-api-key`. Contoh isi file:

   ```
   [
     "contoh-api-key-1",
     "contoh-api-key-2"
   ]
   
   ```

## Versi API yang Tersedia

API ini menyediakan dua versi layanan (`v1` dan `v2`) yang dapat diakses melalui dua cara:

1. **Path / URL Prefix**

   * `/v1/...` (contoh: `/v1/destinations`, `/v1/destinations/1`)

   * `/v2/...` (contoh: `/v2/destinations`, `/v2/destinations/1`, `/v2/destinations?regionId=1`)

2. **Request Header (`x-api-version`)**

   * Mengakses *endpoint* tanpa awalan versi (contoh: `/destinations` atau `/destinations/1`) dengan menyertakan header:

     * `x-api-version: 1` untuk menggunakan versi `v1`

     * `x-api-version: 2` untuk menggunakan versi `v2`

Jika mengakses *endpoint* tanpa awalan versi dan **tanpa** menyertakan header `x-api-version`, server akan mengembalikan respons dengan status **`400`** beserta pesan kesalahan yang menjelaskan kedua cara penyebutan versi di atas.

## Masa Berlaku & Tanggal Berakhirnya `v1`

* Versi **`v1`** sudah berstatus *deprecated* dan **akan berakhir pada `2027-06-30T00:00:00Z`** (`Wed, 30 Jun 2027 00:00:00 GMT`).

* Setiap respons dari versi `v1` akan menyertakan header **`Sunset`** yang merujuk pada tanggal berakhirnya versi `v1` tersebut.

* Pengguna disarankan untuk segera beralih menggunakan **`v2`** (melalui `/v2/...` atau `x-api-version: 2`), yang tidak menyertakan header `Sunset` serta mendukung fitur tambahan seperti filter `regionId` pada daftar destinasi.