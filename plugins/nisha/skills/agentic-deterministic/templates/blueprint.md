# Blueprint: <nama proyek / perubahan>

Tanggal: <YYYY-MM-DD> · Status: draft | disetujui

## Tujuan
<1–3 kalimat: masalah apa yang diselesaikan, untuk siapa>

## Scope
- Termasuk: ...
- Tidak termasuk: ...

## Asumsi (belum diverifikasi)
- ...

## Keputusan (sudah disepakati)
- ...

## Alur bisnis & lifecycle data
<alur utama sebelum UI/DB: data dibuat di mana, berubah kapan, dihapus kapan>

## Acceptance criteria (EARS)
| ID | Kriteria | Bukti |
|---|---|---|
| AC-1 | Ketika <event>, sistem harus <aksi>. | <test / command / smoke check> |
| AC-2 | Jika <kondisi buruk>, maka sistem harus <aksi>. | ... |

## Risiko
| Risiko | Dampak | Mitigasi |
|---|---|---|
| ... | ... | ... |

## Fase / tasklist
- [ ] Phase 1: ...
- [ ] Phase 2: ...

## Otomatis vs approval
- 🤖 Otomatis: ...
- 👤 Butuh approval: deploy production, schema/migrasi, auth, payment, data user, hapus data, ...

## Deploy & rollback
- Preflight: ...
- Langkah: ...
- Smoke check: ...
- Rollback: ...

## Backlog (temuan di luar scope)
- ...
