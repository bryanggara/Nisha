# Aturan Model & Subagent

Tujuan: hasil benar dengan biaya serendah mungkin. Default = kerjakan sendiri; delegasi hanya kalau jelas lebih murah atau lebih akurat.

## 1. Spawn atau kerjakan sendiri?

| Situasi | Keputusan |
|---|---|
| Beberapa tool call, target sudah diketahui | **Inline**, jangan spawn |
| Baca/cari lintas banyak file, cukup kesimpulannya | Subagent `Explore` (haiku) |
| ≥2 pekerjaan independen (tidak saling butuh hasil, tidak menyentuh file sama) | Subagent paralel |
| Butuh review yang tidak terpengaruh kesimpulan sendiri | Subagent reviewer, beri **kode/bukti**, bukan kesimpulanmu |
| Audit/migrasi/review banyak file, riset multi-sumber | Workflow tool — **hanya dengan opt-in**, tawarkan scope + jumlah agent + estimasi biaya |
| Sudah delegasi | Jangan kerjakan ulang hal yang sama; tunggu hasilnya |

## 2. Model per tugas

| Model / effort | Untuk | Jangan untuk |
|---|---|---|
| **haiku / low** | Eksplorasi read-only, grep, baca log, jalankan test, ekstraksi data | Logika kompleks, multi-file, keputusan |
| **sonnet / medium** (default) | Implementasi yang antarmukanya sudah jelas, edit, test, dokumentasi, review rutin, worker per-file | — |
| **opus / medium** | Rencana & arsitektur, desain skema DB, auth/RBAC/permission, mesin approval, payment, review keamanan, verifier temuan penting | Boilerplate, dokumen, pekerjaan yang spesifikasinya sudah rinci, sesi utama rutin |
| **fable** | Masalah ambigu/panjang yang gagal di opus | Rutin apa pun |

- **Sesi utama (Nisha) = sonnet/medium.** Opus dipanggil lewat subagent (`Plan`, verifier, jalur sensitif), bukan dengan menaikkan model sesi utama.
- **Tulis kebijakan model di rencana sebelum meluncurkan agent**, jangan diputuskan diam-diam.
- **Maks ±3 Opus bersamaan** (lebih dari itu sering kena 429). Total agent paralel default ≤5; Workflow < 10 agent kecuali Bryan minta.
- Pakai parameter `model` per agent. Jangan set `SUBAGENT_MODEL_FORCE`.
- Subagent yang diluncurkan skill superpowers (implementer, reviewer) ikut tabel ini, bukan aturan model bawaan superpowers. Putaran fix maks 3, bukan 5.
- **Eskalasi, jangan ulang:** verifikasi gagal → naik satu tingkat model/effort (haiku→sonnet→opus→fable). Mengulang di tingkat yang sama dengan request identik dilarang.

## 3. Brief subagent (satu-satunya konteks yang dia punya)

Wajib memuat:
1. **Goal** + kenapa
2. **Yang sudah dicoba / dikesampingkan**
3. **Scope**: file/folder yang boleh disentuh, yang dilarang
4. **Acceptance criteria** (EARS) + bukti yang diminta
5. **Format output** (mis. tabel `file:line | temuan | bukti`, ringkas)
6. **Stop condition**: kapan berhenti, maks 3 percobaan per masalah, kapan lapor `blocked`
7. **Batas keamanan**: read-only? tanpa push/deploy/migrasi/hapus

Kalau tidak bisa menulis brief yang jelas, berarti belum paham tugasnya — jangan delegasi dulu.

## 4. Agent yang menulis kode

- Satu writer per set file. Agent paralel yang menulis → **worktree terpisah** + kepemilikan path eksplisit.
- File bersama (lockfile, `package.json`, `index.ts`, barrel/registry): tunjuk satu pemilik, atau beri izin eksplisit menambah satu baris. Lockfile: ambil dari main lalu `npm install --package-lock-only`.
- Helper bersama dibuat di awal, bukan ditemukan ulang tiap agent.
- Setelah merge: hapus worktree & klon review (disk cepat penuh).
- Jangan `git add -A` di worktree yang sedang dipakai agent lain (reviewer, e2e): berkas sementara mereka (config test, output) ikut ter-commit. Stage path eksplisit dan cek `git status` sebelum commit.
- Batas berkas terlarang di brief ditulis presisi (daftar path), bukan folder umum. Larangan terlalu lebar (mis. "semua `scripts/*`") membuat agent melewatkan pekerjaan sah yang lalu harus disusul.

## 5. Verifikasi hasil agent

- Laporan agent = klaim, bukan bukti. **Hitung ulang 3–5 klaim kunci** ke sumbernya (angka, status, file:line).
- Temuan berdampak besar → verifier independen (opus) yang mencoba **membantah**.
- Review kode penting: klon bersih + jalankan CI + mutasi beberapa titik + cari fail-open.
- Temuan dikirim balik ke **agent yang sama** (`SendMessage`), bukan agent baru.
- Skill yang diminta dipakai? Cek di transkrip, jangan percaya klaim.
- Status setelah jeda panjang: cek `git log`/MEMORY dulu sebelum menyebutnya.
- "Flaky" dari agent bukan penyebab. Jalankan ulang sendiri test yang disebut flaky sebelum merge; baru terima bila lulus berulang dan sebabnya masuk akal.
- Container/sesi restart: worktree di scratchpad dan berkas output agent biasanya selamat. Ambil laporan terakhir agent dengan script (pesan assistant terakhir di output-nya), jangan luncurkan ulang pekerjaannya.

## 6. Biaya

- >3 agent atau Workflow → sebut estimasi (jumlah agent × model) sebelum jalan.
- Uji di slice kecil dulu, baru skala penuh.
- Output agent minta ringkas; dump file mentah tetap di agent, kesimpulan saja yang kembali.
