# Nisha (marketplace `askara`)

Plugin berisi agent `nisha`, skill `agentic-deterministic`, dan skill `nisha-profile`.

## Alur kerja

Skill `agentic-deterministic` mengatur full path 7 tahap: **spec → plan → tasklist → checklist → deploy → flag QA → release**. Plugin [superpowers](https://github.com/obra/superpowers) menjadi mesin alurnya (brainstorming, writing-plans, subagent-driven-development, TDD, review, finishing branch); skill ini menambahkan gate, kriteria EARS, kebijakan model, batas retry, feature flag, dan release. Kalau bentrok, aturan skill ini yang dipakai.

Satu fitur menghasilkan 3 file: spec (`docs/superpowers/specs/`), plan (`docs/superpowers/plans/`), dan release (`docs/releases/`). Template ada di `plugins/nisha/skills/agentic-deterministic/templates/`.

**Butuh plugin superpowers terpasang.** Tanpa superpowers, tahap yang sama tetap bisa dijalankan manual dengan template.

Pakai di repo mana pun: salin `settings.snippet.json` ke `.claude/settings.json` repo itu, lalu commit.
Aturan routing dari `~/.claude/CLAUDE.md` tidak bisa dibawa plugin; salin ke `CLAUDE.md` repo bila perlu.

Nama agent di setelan `agent` memakai format `<plugin>:<agent>`, yaitu `nisha:nisha`. Format `nisha@askara:nisha` tidak dikenali: Nisha hanya muncul sebagai subagent, bukan agent utama (dites 2026-11-07).

## Install

```
/plugin install nisha --marketplace bryanggara/Nisha
```

## Claude Code cloud

Sesi cloud tidak memasang marketplace dari `extraKnownMarketplaces` secara otomatis. Tambahkan ke setup script environment, dan pilih repo `bryanggara/Nisha` di bagian Repositories environment itu supaya clone-nya diizinkan:

```bash
claude plugin marketplace add bryanggara/Nisha && claude plugin install nisha@askara --scope user || true
```
