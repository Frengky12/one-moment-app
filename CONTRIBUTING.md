# Cara Kerja Version Control (Tim 2 Orang)

## Struktur branch

- `main` — selalu dalam kondisi stabil/deployable. Tidak pernah commit langsung ke sini.
- `feature/nama-fitur` — satu branch per fitur/task (misal `feature/vendor-dashboard`, `feature/guest-camera`).
- `fix/nama-bug` — untuk perbaikan bug.

## Alur kerja

1. Buat branch baru dari `main` terbaru:
   ```
   git checkout main
   git pull
   git checkout -b feature/nama-fitur
   ```
2. Commit kecil & sering, dengan pesan yang jelas (fokus ke "kenapa", bukan cuma "apa"):
   ```
   git commit -m "Tambah validasi limit foto per tamu"
   ```
3. Push branch, buka Pull Request ke `main`:
   ```
   git push -u origin feature/nama-fitur
   ```
4. Karena tim hanya 2 orang, cukup saling review PR satu sama lain sebelum merge (tidak perlu approval berlapis) — tapi tetap wajib direview, jangan self-merge tanpa dilihat partner.
5. Setelah merge, hapus branch fitur yang sudah selesai.

## Commit message

Format bebas tapi konsisten, contoh:
- `feat: tambah upload foto dengan filter preset`
- `fix: reveal foto tidak muncul di mode scheduled`
- `chore: update dependency next.js`

## Environment variables

Jangan pernah commit file `.env`/`.env.local` (sudah masuk `.gitignore`). Simpan kredensial Supabase/Midtrans di masing-masing environment lokal, dan di dashboard Vercel untuk production.
