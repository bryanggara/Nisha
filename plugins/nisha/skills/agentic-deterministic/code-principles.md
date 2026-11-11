# Prinsip Kode

Urutan prioritas: **benar → sederhana → mudah diubah**. Dipakai saat menulis plan (bagian Pendekatan) dan saat review kode.

## 1. KISS dan YAGNI dulu
- Tulis kode paling sederhana yang memenuhi AC. Jangan membangun untuk kebutuhan yang belum ada.
- Abstraksi baru (interface, base class, factory, layer, wrapper, helper generik) hanya bila:
  - sudah ada **≥2 pemakai nyata** sekarang, bukan "nanti mungkin", atau
  - berada di **batas sistem eksternal** (API pihak ketiga, DB, queue, file system) supaya bisa dites tanpa memanggil yang asli.
- Tiga baris kode yang mirip lebih baik daripada abstraksi yang salah. Ekstrak saat pola ketiga muncul.
- Jangan menambah opsi/konfigurasi, cek defensif, atau test yang tidak diminta AC tanpa alasan tertulis.

## 2. SOLID versi ringan

| Prinsip | Kapan dipakai | Bentuk praktis |
|---|---|---|
| **S** — Single Responsibility | Selalu | Satu modul/fungsi = satu alasan untuk berubah. File >300 baris atau fungsi >50 baris → pertimbangkan pecah. Pisahkan I/O (HTTP, DB) dari logika bisnis |
| **O** — Open/Closed | Saat ada ≥2 varian nyata | Varian baru (mis. marketplace TikTok/Shopee/Meta) = tambah adapter, bukan menambah `if` di banyak tempat |
| **L** — Liskov Substitution | Jarang; hanya bila ada pewarisan | Turunan tidak boleh melempar error/menolak input yang diterima induknya. Lebih baik komposisi daripada pewarisan |
| **I** — Interface Segregation | Interface dipakai banyak pihak | Tipe parameter sekecil yang dibutuhkan (`Pick<User, 'id' \| 'email'>`), bukan objek besar |
| **D** — Dependency Inversion | Di batas eksternal | Logika bisnis menerima dependency (client API, repo DB, clock) sebagai parameter, sehingga test bisa memberi fake. Bukan wajib DI container |

## 3. Penanda di review
- 🚩 Interface/abstraksi dengan satu implementasi tanpa alasan batas eksternal → hapus.
- 🚩 Fungsi yang melakukan fetch + transformasi + simpan + kirim notifikasi sekaligus → pecah.
- 🚩 `switch`/`if` per varian tersebar di >2 file → pertimbangkan adapter.
- 🚩 Logika bisnis memanggil `fetch`/ORM/`Date.now()` langsung sehingga tidak bisa dites → injeksi dependency.
- ✅ Nama jelas, fungsi kecil, efek samping di pinggir, logika inti murni.

## 4. Konvensi repo menang
Pola yang sudah ada di repo (struktur folder, gaya error handling, library) diikuti, walau berbeda dari dokumen ini. Mengubah konvensi = keputusan terpisah yang ditulis di spec.
