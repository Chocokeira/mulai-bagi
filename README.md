# Mulai Bagi

Web app chatbot untuk split bill di **Cafe Mula** sekaligus mencatat pengeluaran pribadi.

Foto bill → sebut siapa pesan apa → bagian tiap orang (sudah termasuk service charge dan pajak) siap dibagikan ke WhatsApp bersama nomor rekening tujuan. Bagian milik pengguna ("aku") otomatis tercatat sebagai pengeluaran.

## Fitur

- **Chatbot split bill**
  - Baca foto struk: nama menu, qty, harga, service, pajak, diskon
  - Kartu menu yang bisa diedit, dengan peringatan kalau total tidak cocok dengan struk
  - Pembagian lewat chat bebas ("aku 1 dan 3, Budi 2, fries dibagi semua") atau tap nama per menu
  - Menu bisa dibagi beberapa orang, termasuk dengan bobot
  - Service, pajak, dan diskon dibagi proporsional; pembulatan dijaga supaya jumlahnya sama persis dengan total bill
  - Share ke WhatsApp (grup atau per orang) dan salin teks
- **Pengeluaran**: total per bulan, perbandingan dengan bulan lalu, grafik 6 bulan, daftar transaksi, catatan manual, dan uang talangan yang bisa ditandai lunas
- **Rekening**: simpan beberapa rekening bank atau e-wallet, pilih rekening utama
- **Landing page**

## Struktur

```
index.html   seluruh aplikasi: HTML, CSS, dan JavaScript dalam satu file
README.md
```

Routing memakai hash: `#beranda`, `#chat`, `#pengeluaran`, `#rekening`.

## Menjalankan di komputer sendiri

Tidak perlu build. Jalankan server statis dari folder repo:

```bash
npx serve .
# atau
python3 -m http.server 8000
```

Lalu buka `http://localhost:3000` (serve) atau `http://localhost:8000` (python).

Di luar claude.ai:
- Data pengeluaran dan rekening tersimpan di `localStorage` browser.
- Baca foto bill **belum aktif**. Gunakan **Isi manual** atau **Coba bill contoh** untuk mencoba alurnya.

## Ketergantungan pada claude.ai

Versi ini berjalan sebagai artifact di claude.ai dan memakai tiga kemampuan dari `window.claude.use(...)`:

| Kemampuan | Dipakai untuk | Jika tidak tersedia |
|---|---|---|
| `sample` (dengan gambar) | Membaca foto struk, memahami pembagian yang diketik bebas | Isi manual; pembagian pakai format `Nama: 1, 2; Nama: 3` |
| `db` + `user` | Menyimpan profil, rekening, dan pengeluaran per akun | `localStorage` |

Semua pemanggilan sudah punya fallback, jadi `index.html` tetap jalan di hosting biasa.

## Rencana: hosting sendiri (Vercel)

1. Tambah serverless function `api/scan` yang memanggil Anthropic API untuk membaca foto. API key disimpan di environment variable Vercel, **jangan ditaruh di frontend**.
2. Ganti `askJSON()` di `index.html` supaya memanggil `/api/scan` ketika `window.claude` tidak ada.
3. (Opsional) Database dan login, misalnya Supabase, supaya data tersinkron antar perangkat.

## Cara kolaborasi

1. Buat branch baru untuk setiap perubahan: `git checkout -b fitur/nama-fitur`
2. Commit dengan pesan yang jelas, lalu push branch tersebut
3. Buka Pull Request ke `main` dan minta review
4. Setelah di-merge, `git pull` di `main` sebelum mulai fitur berikutnya

Kalau mengubah `index.html`, cek di layar HP (lebar sekitar 390px) dan di desktop, dalam mode terang maupun gelap.

## Catatan data

Nomor rekening dan pengeluaran adalah data pribadi. Jangan commit data asli ke repo, termasuk di contoh atau seed data.
