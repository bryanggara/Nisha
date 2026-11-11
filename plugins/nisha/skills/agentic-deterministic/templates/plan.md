# Plan: <nama fitur / perubahan>

Tanggal: <YYYY-MM-DD> · Status: draft | disetujui · Spec: <path spec>

> Dengan superpowers: ini bagian **wajib tambahan** untuk plan `writing-plans`
> (`docs/superpowers/plans/YYYY-MM-DD-<topik>.md`), di samping tasklist checkbox bawaannya.
> Plan menjawab **bagaimana**. Goal dan AC tidak ditulis ulang; rujuk ID dari spec.

## Pendekatan
<arsitektur singkat, keputusan teknis + alasan, alternatif yang ditolak + alasan>
<prinsip kode: lihat code-principles.md; sebut abstraksi baru beserta ≥2 pemakainya>

## Kebijakan model
| Fase / peran | Model | Alasan |
|---|---|---|
| Implementer task rutin | sonnet | ... |
| Review jalur sensitif (auth/payment/schema) | opus | ... |
| Eksplorasi / jalankan test | haiku | ... |

Maks 3 putaran fix per task, lalu lapor. Maks ±3 Opus bersamaan.

## Tasklist
Format checkbox `writing-plans`. Setiap task wajib menyebut:
- AC yang dibuktikan (mis. `AC-1, AC-3`)
- Bukti selesai (test/command + expected output)
- 🤖 otomatis atau 👤 butuh approval

## Feature flag
- Nama, tipe (tabel/config | env var), default OFF
- Titik pengecekan flag di kode: ...
- Test untuk jalur flag ON dan OFF: ...

## Deploy & rollback
- Preflight: `git status` bersih, test lulus, build lulus, ...
- Langkah deploy (flag OFF): ...
- Smoke check: <URL/command + expected>
- Rollback: flag OFF → (jika perlu) revert deploy → (migrasi) langkah down/restore backup

## Otomatis vs approval
- 🤖 Otomatis: ...
- 👤 Butuh approval: merge, deploy production, lulus flag QA, release, ...
