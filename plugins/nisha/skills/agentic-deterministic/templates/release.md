# Release: <nama fitur / perubahan>

Spec: <path> · Plan: <path> · PR: <url> · Flag: `ff_<topik>` / tanpa flag (alasan: ...)

> File ini mencatat tahap 5–7 (deploy → flag QA → release). Isi bukti, bukan klaim.

## 5. Deploy (flag OFF)
| Langkah | Hasil / bukti | Waktu |
|---|---|---|
| Approval merge + production dari Bryan | <kutip persetujuan> | |
| Preflight (test, build) | <command → hasil> | |
| Deploy | <commit/digest/versi> | |
| Smoke check | <URL/command → status> | |
| Log error 10 menit pertama | <bersih / temuan> | |

Rollback deploy (jika gagal): <apa yang dilakukan + hasil>

## 6. Flag QA (internal)
Flag ON untuk: <user/email/role>

| AC | Skenario QA | Hasil | Bukti |
|---|---|---|---|
| AC-1 | <jalur normal> | ✅ / ❌ | <screenshot/output/status> |
| AC-2 | <jalur error> | | |
| AC-3 | <flag OFF tetap perilaku lama> | | |

Keputusan: ⏳ menunggu / ✅ lulus (Bryan, <tanggal>) / ❌ gagal → flag OFF, kembali ke tahap 3

## 7. Release
- Approval release: <kutip persetujuan>
- Rollout: semua / bertahap (<persen/kelompok> → ...)
- Pantau selama <jendela>: <metrik + threshold, mis. error rate < 1%, p95 < 800 ms>
- Hasil pantau: ...
- Rollback release: flag OFF (tanpa deploy) — dipakai? ya / tidak

## Setelah release
- [ ] Task hapus flag + jalur lama masuk backlog (target tanggal: ...)
- [ ] Keputusan tahan lama dicatat (Titen / CLAUDE.md repo)
