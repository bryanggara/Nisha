# Spec: <nama fitur / perubahan>

Tanggal: <YYYY-MM-DD> · Status: draft | disetujui · Plan: <path plan setelah dibuat>

> Dengan superpowers: ini bagian **wajib tambahan** untuk design doc `brainstorming`
> (`docs/superpowers/specs/YYYY-MM-DD-<topik>-design.md`). Tanpa superpowers: pakai file ini apa adanya.
> Spec menjawab **apa & kenapa**. Detail implementasi (file, fungsi, urutan task) masuk plan.

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
| AC-3 | Di mana flag `ff_<topik>` nonaktif, sistem harus <perilaku lama>. | ... |

## Risiko
| Risiko | Dampak | Mitigasi |
|---|---|---|
| ... | ... | ... |

## Rencana rollout
- Dilihat user nyata di production? ya / tidak
- Feature flag: `ff_<topik>` (tabel/config | env var) / tidak dipakai, alasan: ...
- Target Flag QA: <user/email/role internal>
- Migrasi DB: tidak ada / ada (expand → deploy → contract, butuh approval terpisah)

## Gate yang dibutuhkan
- 👤 <approval apa saja: production, schema, auth, payment, data user, ...>

## Backlog (temuan di luar scope)
- ...
