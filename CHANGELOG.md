# Changelog — GConsultant

Semua perubahan penting dicatat di sini (format: Keep a Changelog, SemVer).

## [3.2.0] — 2026-10-24 — Full Catalog Integration

### Added
- **Integrasi penuh katalog PusatPerizinan (500 layanan)**: seluruh layanan dari gunaracorp/pusatperizinan (`layanan-500.json`) kini hidup di GConsultant sebagai halaman detail `/layanan/[slug]` — total 546 layanan aktif (46 unggulan + 500 katalog), sitemap 66 → 566 URL.
- **Harga transparan & diskon aktif**: setiap halaman katalog menampilkan harga resmi Rupiah, harga normal tercoret, badge diskon persen, dan estimasi hemat — aset diferensiasi yang selama ini hanya ada di pusatperizinan.com.
- **Taksonomi 4 kategori + 38 sub-kategori bilingual** (`katalog-meta.ts`): lingkup layanan, alur pengurusan, dokumen yang disiapkan, dan FAQ keluarga layanan — konsisten untuk seluruh anggota sub.
- **Seksi "Katalog Lengkap"** di hub `/layanan`: pencarian instan 546 layanan (nama/kode/slug), filter 4 kategori + chip sub-kategori, paginasi adaptif 24/halaman, kartu harga berdiskon.
- **Indeks klien ringan** (`katalog-lite.ts`): pencarian 500 item tanpa memuat deskripsi — bundle browser tetap ramping.
- **JSON-LD lengkap** untuk halaman katalog: Service + Offer (harga IDR diskon), BreadcrumbList, FAQPage, HowTo.
- **/api/status** kini melaporkan `catalogServices` (500) dan `servicesTotal` (546).

### Changed
- Metadata hub `/layanan`: "46 Jenis" → "546 Layanan"; klaim hero "500+ perizinan" kini terbukti oleh katalog nyata.
- Sitemap menambah 500 URL katalog (prioritas 0.8).

### Verified (Bukti)
- `validateAllSlugs()`: 0 error — 546 slug unik tanpa bentrokan lintas tier, 38 sub-meta & 4 kategori lengkap.
- Lint 0 error; 62 halaman katalog acak + sampel keempat kategori (BU/KP/LH/SK) semua HTTP 200; sitemap 566 URL.
- Agent-browser E2E: kartu harga/diskon/hemat ter-render; pencarian "halal" → 15 hasil; switch EN mengubah label keluarga layanan; mobile 375px tanpa scroll horizontal; dev.log tanpa error.

## [3.1.1] — 2026-10-24 — Preview Iframe Hardening

### Fixed
- **Preview sandbox "refused to connect" diberantas**: `X-Frame-Options: SAMEORIGIN` pada semua respons memblokir penyematan situs oleh UI chat via `<iframe>` lintas-origin — browser menampilkan error "refused to connect" padahal server merespons 200. Diganti kebijakan frame berlapis: header `Content-Security-Policy: frame-ancestors *` dikirim, `X-Frame-Options` tidak lagi dikirim pada mode preview.
- **Saklar produksi**: set env `FRAME_POLICY=sameorigin` saat deploy produksi sungguhan → CSP `frame-ancestors 'self'` + `X-Frame-Options: SAMEORIGIN` dikirim kembali (proteksi klikjacking terkunci), tanpa menyentuh kode.

### Changed
- Versi 3.1.0 → 3.1.1 (SemVer patch: perubahan header keamanan, tanpa perubahan UI/route).

### Verified (Bukti)
- `next.config.ts` dimuat ulang tanpa error sintaks (import uji via bun); kebijakan frame terverifikasi pada level konfigurasi; header lain (nosniff, Referrer-Policy, Permissions-Policy, HSTS) tidak berubah.

## [3.1.0] — 2026-10-24 — Lead UX Hardening

### Fixed
- **Form kontak `/kontak` — silent-fail diberantas**: validasi native HTML diganti validasi custom inline (`noValidate`) — pesan error di bawah setiap field (`role="alert"`, `aria-invalid`, `aria-describedby`), fokus otomatis ke field pertama yang salah, border merah real-time yang hilang saat dikoreksi.
- **Layanan pada form kini opsional** — friksi turun, konversi naik; pesan WhatsApp otomatis memakai "Konsultasi umum / General consultation" bila layanan belum dipilih. Label diberi tanda "(Opsional — boleh dipilih nanti)".
- **Konfirmasi pengiriman terlihat**: baris status `aria-live="polite"` di bawah tombol — "WhatsApp terbuka di tab baru — kami menanti pesan Anda!" (sebelumnya hanya teks tombol berubah 3,5 detik; kini 6 detik + status permanen).
- **Autocomplete & a11y**: `autoComplete="name"`/`"tel"` pada input, placeholder nomor telepon dinormalisasi.

### Changed
- Versi 3.0.0 → 3.1.0 (SemVer minor: perbaikan UX tanpa perubahan API/route).

### Verified (Bukti)
- Crawl penuh 66/66 URL sitemap → HTTP 200; lint 0 error; security headers 6/6; E2E agent-browser: alur submit form gagal-validasi menampilkan pesan inline, alur valid membuka wa.me dengan pesan terisi lengkap.

## [3.0.0] — 2026-10-23 — GCONSULTANT REBRAND — Polymathic Edition

### Changed (Rebrand Total — sesuai GConsultant Brand System v1.0)
- **Identitas baru "The Geometric G"**: logo perisai "Golden Seal" diganti mark G geometris kontinu (cincin konsentris + palang menuju inti) — tanpa perisai/mahkota/segel sesuai guideline. Inline SVG (brand-seal.tsx), logo.svg, logo-horizontal.svg, logo-stacked.svg (baru), logo-mono.svg.
- **Palet resmi token brand**: Sovereign Green #073B2E (primary), Deep Forest #0F241D (dark), Antique Gold #C6A15B (accent), Illuminated Gold #E5C97A, Legacy Cream #F6F1E4 (light), Ink #111815, Charcoal, Clarity Green — menggantikan palet Emerald Royal.
- **Tipografi resmi**: Cormorant Garamond (display serif, 500/600/700) + Inter (UI) menggantikan Playfair Display + Plus Jakarta Sans.
- **Hero beranda dibangun ulang sesuai preview resmi**: kicker POLYMATHIC STRATEGY & SYSTEMS ARCHITECTURE, headline serif "Kompleksitas memiliki struktur. Solusi memiliki jalur." / "Complexity has a structure. Solutions have a path.", CTA emas "Jelajahi Pendekatan Kami" → /metodologi, plus strip pendekatan DIAGNOSE / ARCHITECT / EXECUTE + band statistik (500+/38/15+/98%).
- **Brand name**: Gunara Consultant → GConsultant (wordmark GCONSULTANT); signature THE POLYMATHIC PROBLEM SOLVER; promise "Menyatukan ilmu. Menata kompleksitas. Mewujudkan solusi."
- **Domain resmi**: gunaraconsultant.com → gunara.web.id (metadataBase, canonical, hreflang, OG, sitemap 66 URL, robots, email halo@gunara.web.id, JSON-LD).
- favicon.ico di-rebuild byte-per-byte dari icon.svg baru (4 entri PNG 16/24/32/48, assembler murni); apple-icon.png, icon-192.png, icon-512.png diregenerasi.
- og-image.jpg 1200x630 komposisi baru (Deep Forest + Geometric G + wordmark + tagline + promise).
- hero kini CSS-only (hero-bg.png dihapus) — LCP lebih cepat, tanpa aset gambar hero.

### Fixed (Bug)
- **SVG gradient objectBoundingBox pada garis horizontal** (bbox tinggi-0) membuat palang G tidak dirender — palang kini memakai gradient userSpaceOnUse (brand-seal, icon.svg) / warna solid (OG image).

### Verified (Bukti)
- lint 0 error; favicon.ico 4 entri PNG-valid (round-trip parser); seluruh route 200; SSR memuat GCONSULTANT, THE POLYMATHIC PROBLEM SOLVER, gunara.web.id, DIAGNOSE/ARCHITECT/EXECUTE; nol referensi "Gunara Consultant"/domain lama di src.

## [2.0.0] — 2026-10-23 — GOD MODE TIER MAX — UMEA Edition

### Added (Baru)
- **Flagship UMEA™ Master Matrix**: halaman `/metodologi` — 3 Hukum Besar, 6 tingkat organisasi (UMEA-03 s.d. UMEA-21), 21 fungsi kritis (function-first), 5 tingkat dokumen (D-10 s.d. D-165), 8 band UMEA Maturity Index™ — semua bilingual ID/EN dengan padanan "bahasa profesional" dan "bahasa sehari-hari".
- **Mesin asesmen interaktif** `/asesmen`: wizard 12 pertanyaan berbobot → skor UMI 0–100 → band kematangan → tingkat UMEA → 3 titik terlemah + kekuatan → paket dokumen rekomendasi → CTA WhatsApp dengan pesan hasil terisi otomatis (event konversi `begin_checkout`, `view_item`, `generate_lead`).
- `src/data/umea.ts` — single source of truth metodologi (Z2: konten terpisah tampilan) + mesin skor + validator integritas data.
- **Halaman legal (S11)**: `/kebijakan-privasi` (UU PDP No. 27/2022, GDPR-ready) + `/syarat-ketentuan` (IP, kanal pembayaran resmi BCA, hukum Indonesia) — bilingual, JSON-LD WebPage + breadcrumb.
- **Halaman 404 kustom** `not-found.tsx` — Golden Seal + CTA beranda/layanan, noindex.
- **S1 Sistem versi**: `src/lib/version.ts` (sumber tunggal), badge versi di footer, `softwareVersion` di JSON-LD Organization, sinkron package.json + CHANGELOG.
- **S3 README kelas dunia** — bilingual ID/EN, badge, struktur internasional, PETA MODUL ABADI.
- **S12 Resiliensi**: endpoint `/api/status` (versi + kesehatan modul), `docs/RUNBOOK.md`.
- **S7 Analitik**: wiring GA4 + Microsoft Clarity via env (`NEXT_PUBLIC_GA_ID`, `NEXT_PUBLIC_CLARITY_ID`) — no-op tanpa ID, menghormati Do-Not-Track.
- **S10 Aksesibilitas**: skip-link "Lompat ke konten utama" global + `main#main-content` di semua halaman baru.
- **PIAGAM 2125** di `/tentang-kami` — narasi brand abadi (FASE 3).
- Sitemap 62 → 66 URL (metodologi, asesmen, kebijakan-privasi, syarat-ketentuan) + 75 kunci i18n baru simetris.

### Security (Keamanan)
- **S6**: security headers global (X-Frame-Options SAMEORIGIN, nosniff, Referrer-Policy, Permissions-Policy, HSTS) + rate-limit 30 req/menit/IP pada `/api` + cache rules aset immutable.
