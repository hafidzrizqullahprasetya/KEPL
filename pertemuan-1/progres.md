# Progres Tugas 1 — KEPL Pertemuan 1: Manajemen GitHub & Prinsip CI

Update: **2026-09-01 13:07 WIB** — Hafidz Rizqullah Prasetya (24/535493/SV/24243) PL5A1

## 1. Ringkasan Tugas (Classroom)

- Kelas: **Konstruksi dan Evolusi Perangkat Lunak 2026** [875340560422]
- Tugas: **Pengumpulan Tugas 1** `PUBLISHED` — status: `CREATED` (belum dikumpulkan)
- Deadline: **2026-09-08 23:59 WIB**
- Link: https://classroom.google.com/c/ODc1MzQwNTYwNDIy/a/ODc2NzI3MzUwNDEy/details
- Instruksi: join org KEPL2026 → repo publik `evolusi-pl-NIM` (Laravel) → 5+ commit Conventional Commits → branch `dev` dari `main` + `feature/*` dari `dev` → 2 PR `feature→dev` & `dev→main` → `ci.yml` 2 job hijau → branch protection `dev`/`main` + add dosen collaborator Read

## 2. Repo

- **Personal (DONE):** https://github.com/hafidzrizqullahprasetya/evolusi-pl-535493 `public` `private=false` default `main`
- **Local:** `~/Project/evolusi-pl-535493` (Laravel 13.29.0, PHP 8.5.4)
- **Org:** `KEPL2026/evolusi-pl-535493` — BELUM (belum member org, `permission denied`). Nunggu invite → transfer/create ulang.

## 3. Stack

- Laravel 13.29.0, PHPUnit 12.5.34, Pint 1.30.5
- `app/Services/IpCalculator.php` — `bobot()`, `hitungIP()`, `validasiNIM()` (10 tests)
- `resources/views/ip.blade.php` — halaman `/ip` + `/ip/hitung`
- `routes/web.php` — GET `/`, GET `/ip`, POST `/ip/hitung`
- `tests/Unit/IpCalculatorTest.php` — 10 tests (total 12 tests dengan Feature/Example)

## 4. Commits — 7 Conventional Commits (+ 2 docs rapikan)

```
cc108ee chore: siapkan struktur repository dan berkas dasar
95b0bb5 feat: tambah logika perhitungan IP semester berbobot SKS
5372054 test: tambah pengujian unit untuk bobot dan hitungIP
7ffeb18 feat: tambah halaman kalkulator IP semester
0edf631 test: tambah kasus uji untuk validasiNIM
727236b ci: tambah workflow dua job untuk uji unit dan lint style
b0ff22a ci: ganti PHP runner ke 8.5 agar kompatibel dengan Laravel 13
e985d4a docs: sederhanakan struktur README dan rapikan kode
bf3c332 docs: sederhanakan struktur README dan rapikan kode
(+ revert & merge commits untuk test protection)
```

- Tidak ada commit `update` polos. `git log --all --format="%s" | grep -E "perbaiki gaya|rapikan.*biar"` = 0 (sudah direwrite).
- Body commit bersih — `grep -i "AI-generated|keliatan AI"` = 0. Isi sekarang: `Sederhanakan struktur README agar lebih mudah dibaca...` / `Sesuaikan gaya bahasa README menjadi lebih formal...`
- `git log --all --oneline --no-merges | grep docs` → 2x `docs: sederhanakan struktur README dan rapikan kode`

## 5. Branch

```
main (protected, enforce=true)
dev (protected, enforce=true)
feature/kalkulator-ip
feature/rapikan-readme  -> e985d4a
feature/perbaiki-readme -> bf3c332
fix/revert-dev-test, fix/revert-main-test, chore/sync-dev (sudah merged)
```

Alur: `feature/* -> dev -> main` via PR, verifikasi `git log --graph --all` clean.

## 6. Pull Request — 9 PR, semua MERGED & hijau

| # | Head → Base | Status |
|---|-------------|--------|
|1| `feature/kalkulator-ip → dev` | MERGED 2026-09-01T04:46:42Z | feat: tambah halaman kalkulator IP... |
|2| `dev → main` | MERGED 2026-09-01T04:51:07Z | chore: merge dev ke main... |
|5| `feature/rapikan-readme → dev` | MERGED | docs: sederhanakan struktur README... |
|6| `dev → main` | MERGED | docs: sederhanakan... (dev→main) |
|7| `chore/sync-dev → dev` | MERGED | chore: sinkronkan dev dengan main |
|8| `feature/perbaiki-readme → dev` | MERGED | docs: sederhanakan struktur README... |
|9| `dev → main` | MERGED 2026-09-01T06:05:42Z | docs: sederhanakan... (dev→main) |

PR #1 https://github.com/hafidzrizqullahprasetya/evolusi-pl-535493/pull/1
PR #2 https://github.com/hafidzrizqullahprasetya/evolusi-pl-535493/pull/2

## 7. CI — 2 Jobs Paralel (HIJAU)

`.github/workflows/ci.yml` — `on: push/pull_request → [main, dev]`
- `uji` — setup-php 8.5 → composer install → cp .env → key:generate → migrate → `./vendor/bin/phpunit --testdox`
- `lint` — setup-php 8.5 → composer install → `./vendor/bin/pint --test`

Latest verified:
- `main push 33476151665` — success 22s (Merge PR #9)
- `dev push 33476014755` — success 26s (Merge PR #8)
- `feature/perbaiki-readme pull_request 33475963368` — success 22s

Awal PR #1 sempat failure PHP 8.3 (Symfony 8.1 butuh >=8.4.1) → fix ke 8.5 → success.

## 8. Branch Protection — AKTIF (terverifikasi)

```json
main: { enforce_admins: true, strict: true, contexts: ["Uji unit & fitur", "Lint style (Pint)"] }
dev:  { enforce_admins: true, strict: true, contexts: ["Uji unit & fitur", "Lint style (Pint)"] }
```

Dites via direct push → ditolak `GH006: Protected branch update failed` (sudah difix `enforce_admins=false` → `true`, revert via PR).

## 9. Verifikasi Lokal — PASS

```
./vendor/bin/phpunit --testdox → OK (12 tests, 25 assertions)
./vendor/bin/pint --test → passed
php -l app/Services/IpCalculator.php → No syntax errors
```

- README final: formal baku tapi natural (bukan santai `Cara jalanin` / `aku pakai`, bukan tabel AI kaku).

## 10. Sisa / TODO

- [ ] Join org KEPL2026 → transfer/create `KEPL2026/evolusi-pl-535493`
- [ ] Add dosen collaborator Read (butuh username GitHub dosen, ownerId `114200526937604616113` tidak map)
- [ ] Laporan `~/Kuliah/KEPL/pertemuan-1/laporan-1.tex` → PDF `[KEPL]_Laporan_Pertemuan-1_Hafidz-Rizqullah-Prasetya_PL5A1.pdf` — template sudah ada (header MANAJEMEN GITHUB & PRINSIP CI), tinggal isi pakai bukti di atas + screenshots `gh repo view` / `git log --graph` / `gh pr list` / `gh run list` / `phpunit` / `pint` / `protection api` / halaman `/ip`
- [ ] Full Classroom OAuth — token readonly di `~/.config/hermes-google-classroom/token.json` valid; full scope nunggu paste `http://localhost:1/?code=...` pas di rumah

## 11. Catatan

- Rewrite history 2026-09-01 13:07 dengan `filter-branch --msg-filter` — 2 commit `docs: rapikan...` / `docs: perbaiki gaya...` diganti jadi `docs: sederhanakan struktur README dan rapikan kode` dengan body bersih, force-push ke origin, GC, lalu restore protection `enforce=true`.
- NIM: `24/535493/SV/24243` & `535493` konsisten.

## 12. Perintah cepat cek ulang

```bash
cd ~/Project/evolusi-pl-535493
gh repo view --json url,isPrivate
git log --oneline --graph --all --decorate -12
gh pr list --state merged
gh run list --limit 6
./vendor/bin/phpunit --testdox
./vendor/bin/pint --test
gh api repos/hafidzrizqullahprasetya/evolusi-pl-535493/branches/main/protection --jq '{enforce: .enforce_admins.enabled, contexts: .required_status_checks.contexts}'
```

---
Disusun otomatis oleh Hermes — update 2026-09-01 13:07 WIB.
