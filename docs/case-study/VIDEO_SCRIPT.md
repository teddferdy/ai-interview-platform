# Video Walkthrough Script — 3–5 menit (Loom/YouTube)

## Pembukaan (0:00–0:20)
Halo, saya [NAMA]. Ini demo perbaikan studi kasus Fullstack Product Engineer di
**AI Interview Platform**. Fokus: memperbaiki *constraint signal* P0 — kebocoran
data kandidat lintas tenant (IDOR) — dan memperkuat keandalan sesi interview AI.

## 1. Bukti kode & kualitas (0:20–1:10)
1. Buka terminal di folder `api`, jalankan:
   - `bundle exec rspec` → **83 examples, 0 failures**
   - `bundle exec rubocop --lint` → **111 files, no offenses**
2. Buka folder `web`, jalankan:
   - `npm test` → **3 files, 10 tests passed** (Vitest)
   - `npm run build` → **tsc + vite build sukses**
3. Buka `.github/workflows/ci.yml` → jelaskan pipeline: DB prepare → RSpec → RuboCop → typecheck/build → Vitest.

## 2. Demo aplikasi — sisi assessor (1:10–2:30)
1. Buka `http://localhost:5174/assessments` (login assessor) → daftar asesmen.
2. Buka form asesmen → susunan skill.
3. Buka tautan undangan kandidat → copy URL.
4. Buka portfolio kandidat (status complete) → lakukan override level skill.
5. Buka Fit/Gap report terhadap vacancy.

## 3. Momen AI verification (2:30–3:30) — WAJIB
1. Ceritakan contoh kode/saran AI yang ternyata salah & dikoreksi:
   - **Speed test:** awalnya pakai host echo pihak ketiga (httpbin/postman) → nyata-nyata `503/504` dan 503 terukur sebagai upload palsu → diganti endpoint backend sendiri → regression test di Vitest.
   - **Model Gemini:** default `gemini-2.0-*` ternyata sudah mati (`404`) → diverifikasi via `ListModels` → pakai `gemini-3.1-flash-live-preview` dsb.
2. Buka terminal, tampilkan WS proxy test / sesi live:
   - Handshake Gemini Live (`setupComplete`) sukses.
   - Muncul `{"type":"transcription","speaker":"ai","text":"Hi there, ..."}` —
     bukti asisten AI benar-benar berbicara di sesi kandidat.
3. Jelaskan: pipeline portfolio → fit/gap hidup end-to-end.

## 4. Seeded Fault Test (3:30–4:30) — WAJIB
1. Jelaskan spec `cross-tenant isolation` di `portfolio_skills_controller_spec.rb`.
2. **Tanam fault:** hapus `for_tenant` dari `set_portfolio_skill`, simpan.
3. Jalankan spec → **1 failure** ("expected 404 but it was 201") — override lintas tenant
   berhasil = data bocor.
4. **Revert fault** → spec hijau kembali → `git status` bersih.
5. Kesimpulan: spec menangkap regresi P0; CI akan memblokir merge bila bocor lagi.

## Penutup (4:30–5:00)
- Ringkas: P0 IDOR diperbaiki lewat concern `TenantScopedBySession` + 83 spec hijau + CI;
  sesi kandidat lebih andal (fallback end tanpa JWT, audio resilience); UX fit/gap & override
  tidak lagi buntu; tidak ada secret yang di-commit.
- Link PR: [URL PR/fork]. Terima kasih.

## Checklist saat merekam
- [ ] Rekam layar 1080p, suara jelas.
- [ ] Pastikan puma + sidekiq + vite berjalan.
- [ ] Tampilkan output terminal berwarna (bukan screenshot statis) di bagian test/fault test.
- [ ] Upload ke Loom/YouTube (unlisted), lalu tempel link ke laporan (PDF) & form submit.
