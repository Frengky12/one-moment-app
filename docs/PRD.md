# PRD — One Moment: Aplikasi Kamera Acara untuk Vendor (WO/Fotografer)

**Status:** Draft v1
**Terakhir diperbarui:** 2026-07-14

---

## 1. Latar Belakang & Masalah

Pasangan/host acara (pernikahan, ulang tahun, gathering) hanya mendapat dokumentasi dari sudut pandang fotografer profesional — momen candid dari sudut pandang tamu sering terlewat. Produk sejenis (satualbum, Lense, POV) sudah menjawab ini untuk konsumen langsung, tapi:

- Model konsumen-langsung berarti kompetisi head-to-head lawan pemain yang sudah mapan (satualbum di Indonesia, Lense secara global).
- Vendor acara (Wedding Organizer, fotografer) belum punya tools yang bisa mereka **jual ulang sebagai bagian paket layanan mereka sendiri**.

**Peluang:** membangun tools kamera-acara yang didistribusikan lewat WO/fotografer (B2B2C), bukan dijual langsung ke pasangan/host.

## 2. Tujuan Produk

- Memberi WO/fotografer added-value yang bisa mereka tawarkan ke klien dengan branding mereka sendiri.
- Model revenue recurring (subscription ke vendor) alih-alih one-time per acara.
- Diferensiasi lewat fitur **live slideshow** di layar resepsi (bukan cuma galeri pasca-acara).

## 3. Target Pengguna

| Peran | Deskripsi | Kebutuhan utama |
|---|---|---|
| **Vendor (WO/Fotografer)** — *primary customer* | Mengelola banyak acara klien, butuh branding sendiri | Dashboard multi-event, branding custom, harga terjangkau untuk di-mark-up ke klien |
| **Host acara** (pasangan/pemilik acara) | Penerima manfaat langsung, tapi tidak bayar langsung ke kita | Setup mudah lewat vendor, hasil foto bagus & mudah diakses |
| **Tamu acara** | Pengguna akhir yang motret | Tanpa install app, tanpa daftar akun, langsung bisa motret dari QR |

## 4. Ruang Lingkup MVP

### Termasuk (MVP)
- Autentikasi & dashboard vendor (multi-event)
- Buat event: judul, preset film, limit foto/tamu, mode reveal
- Branding custom ringan (logo & warna vendor di halaman tamu)
- Halaman kamera tamu (web, tanpa akun — cukup nama)
- Filter preset film (diterapkan di sisi client)
- Mekanisme reveal (live / setelah acara / terjadwal)
- Galeri hasil (download individual & ZIP semua foto)
- Live slideshow untuk layar resepsi (real-time)
- Subscription billing vendor via Midtrans (QRIS, GoPay, VA, kartu)

### Tidak termasuk di MVP (fase berikutnya)
- Native mobile app (web mobile-first sudah cukup)
- Sistem multi-role/tim di dalam akun vendor
- Analytics mendalam (retensi, funnel, dsb.)
- Cetak foto fisik / integrasi percetakan
- Video (hanya foto di MVP)

## 5. Functional Requirements

### 5.1 Vendor Dashboard
- FR-1: Vendor bisa daftar/login (email + password atau magic link)
- FR-2: Vendor bisa membuat event baru dengan: judul, slug otomatis, preset film, limit foto/tamu, mode reveal, waktu reveal (jika terjadwal)
- FR-3: Vendor bisa upload logo & pilih warna brand yang tampil di halaman tamu
- FR-4: Vendor bisa melihat daftar semua event miliknya beserta jumlah tamu & foto
- FR-5: Vendor bisa generate & download QR code untuk tiap event
- FR-6: Vendor bisa download semua foto sebuah event sebagai ZIP
- FR-7: Vendor bisa melihat status langganan & riwayat pembayaran

### 5.2 Halaman Tamu
- FR-8: Tamu membuka link/scan QR → diminta isi nama saja (tanpa password)
- FR-9: Tamu mengambil foto langsung dari browser (akses kamera device)
- FR-10: Foto otomatis diberi filter preset sesuai pengaturan event
- FR-11: Tamu dibatasi jumlah foto sesuai limit yang diatur vendor
- FR-12: Tamu bisa melihat galeri foto yang sudah "terungkap" (revealed)

### 5.3 Reveal & Slideshow
- FR-13: Sistem mendukung 3 mode reveal: langsung, setelah waktu tertentu, manual oleh vendor
- FR-14: Halaman live slideshow menampilkan foto baru secara real-time untuk ditayangkan di layar/proyektor venue

### 5.4 Billing
- FR-15: Vendor berlangganan paket bulanan via Midtrans (recurring)
- FR-16: Sistem menerima webhook status pembayaran dari Midtrans dan memperbarui status langganan

## 6. Requirement Desain (UI/UX)

### Prinsip desain
- **Mobile-first mutlak** untuk halaman tamu — mayoritas diakses dari HP di venue.
- **Zero-friction onboarding tamu**: dari scan QR sampai bisa motret harus ≤ 2 tap, tanpa form panjang.
- **Estetika "film/analog"** konsisten dengan preset yang dipilih (grain, vignette, warna sesuai preset — FunSaver/QuickSnap/Portra/dsb.) supaya pengalaman terasa premium & nostalgic, bukan generik.
- **Branding vendor terasa menonjol**, bukan branding produk kita — logo/warna vendor harus jadi elemen visual utama di halaman tamu (vendor harus merasa ini "tools mereka").
- **Dashboard vendor**: fungsional & bersih, prioritas kecepatan input (buat event baru harus bisa <1 menit).

### Layar utama yang perlu didesain
1. Landing/marketing page (untuk approach vendor & self-serve signup nanti)
2. Vendor: Login/Register
3. Vendor: Dashboard (list event)
4. Vendor: Form buat/edit event (termasuk upload logo & pilih warna)
5. Vendor: Detail event (statistik, download ZIP, QR code)
6. Tamu: Landing acara (isi nama, branding vendor tampil)
7. Tamu: Kamera (viewfinder + preset filter preview)
8. Tamu: Galeri (foto revealed)
9. Live slideshow (fullscreen, untuk ditampilkan di TV/proyektor)
10. Vendor: Billing/subscription

## 7. Skema Database

```
users                         -- akun vendor (WO/fotografer)
- id (uuid, pk)
- email (unique)
- password_hash
- business_name
- logo_url
- brand_color
- plan_tier            enum(free_trial, subscribed)
- created_at

events
- id (uuid, pk)
- user_id (fk -> users.id)
- title
- slug                  (unique, dipakai di URL QR)
- film_preset           enum(FunSaver, QuickSnap, Portra400, Ektar100, HP5Plus, CineStill800T)
- photo_limit_per_guest int
- reveal_mode           enum(live, after_event, scheduled)
- reveal_at             timestamp (nullable)
- status                enum(active, ended)
- created_at

guests
- id (uuid, pk)
- event_id (fk -> events.id)
- name
- session_token         (unique, disimpan di cookie/localStorage device tamu)
- joined_at

photos
- id (uuid, pk)
- event_id (fk -> events.id)
- guest_id (fk -> guests.id)
- storage_path
- preset_applied
- taken_at
- revealed              bool, default false
- order_index

subscriptions
- id (uuid, pk)
- user_id (fk -> users.id)
- plan
- status                enum(active, past_due, canceled)
- midtrans_subscription_id
- current_period_end
```

**Relasi:** `users` 1—N `events` 1—N `guests` 1—N `photos`. `users` 1—1 `subscriptions` (aktif).

## 8. Arsitektur Teknis

- **Frontend + API:** Next.js + TypeScript (satu codebase, SSR untuk halaman tamu agar cepat di koneksi venue yang lemah)
- **Database, Auth, Storage, Realtime:** Supabase
- **Filter foto:** diproses di client (Canvas API) sebelum upload — tidak ada image processing berat di server
- **Live slideshow:** Supabase Realtime channel, subscribe per `event_id`
- **Reveal terjadwal:** Supabase Edge Function + cron
- **Payment:** Midtrans Subscription API + webhook endpoint
- **Hosting:** Vercel

## 9. Non-Functional Requirements

- **Performa:** halaman tamu harus interactive dalam <3 detik di koneksi 3G/4G lemah
- **Privasi:** foto milik vendor/host acara, tidak dipakai untuk keperluan lain tanpa izin; kebijakan privasi jelas untuk tamu yang uploads fotonya
- **Skalabilitas:** desain harus mampu tangani lonjakan singkat (puncak: ratusan foto ter-upload dalam rentang beberapa jam saat acara berlangsung)
- **Reliabilitas saat live:** kegagalan upload foto satu tamu tidak boleh mengganggu tamu lain (upload independen, retry otomatis di client)

## 10. Metrik Keberhasilan

- Jumlah vendor aktif berlangganan (target awal: 5-10 vendor di 3 bulan pertama)
- Jumlah acara dijalankan per bulan
- Retensi vendor (apakah vendor pakai lagi di acara berikutnya)
- Rating/testimoni dari host acara & tamu

## 11. Risiko & Pertanyaan Terbuka

- Apakah vendor bersedia bayar subscription di muka sebelum ada klien yang pakai, atau perlu model pay-per-event dulu di awal untuk kurangi friksi adopsi?
- Bagaimana penanganan foto jika koneksi venue benar-benar buruk (offline-first upload queue perlu dipertimbangkan di fase berikutnya)?
- Perlu validasi: apakah fitur live slideshow benar-benar jadi pembeda kuat, atau vendor lebih peduli soal harga & branding?
