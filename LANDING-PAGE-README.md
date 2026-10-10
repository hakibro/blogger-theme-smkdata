# Mode Landing Page (Full Page) — Blogger Theme

Tema Blogger `theme-8996540688293237985.xml` mendukung **mode landing page**:
sebuah Postingan atau Halaman di-render sebagai **full page** — navbar, header
situs, sidebar, footer, judul/breadcrumb/byline postingan semuanya disembunyikan,
dan container dibuat full-width tanpa padding/max-width.

## 3 Cara Mengaktifkan (pilih salah satu)

### 1) Label `landing` (paling praktis)
Beri **Label** `landing` (atau `fullpage`) pada Postingan/Halaman.
Label cocok **case-insensitive**.

```
Label: landing
```
Varian yang dikenali: `landing`, `Landing`, `LANDING`, `fullpage`, `full-page`,
`full page`, `landing-page`, `landing page`.

### 2) Penanda di judul `[landing]` (untuk halaman tanpa fitur label)
Awali judul postingan/halaman dengan `[landing]` atau `[fullpage]`:

```
[landing] SPMB SMK Darut Taqwa 2026/2027
```

Penanda ini **tidak ikut tampil** karena judul disembunyikan pada mode landing.

### 3) URL tertentu (server-side, tanpa JS, tanpa kedip)
Tambahkan URL halaman ke daftar di dalam XML:

```xml
<b:with value='data:view.url in {"https://smkdata.sch.id/2026/10/spmb-daftar-lebih-awal.html"}' var='isLandingUrl'/>
```

URL harus **persis sama** dengan canonical URL halaman (termasuk `https://` dan `.html`).
Pisahkan beberapa URL dengan koma: `{"url1","url2"}`.

> Untuk screenshot Anda (`/2026/10/spmb-daftar-lebih-awal.html`), URL ini sudah
> dimasukkan ke daftar. Cukup pastikan URL-nya persis.

## Apa yang Disembunyikan / Diubah

| Area | Perilaku |
|------|----------|
| Navbar/menu (`.topbar`, `#menunav`), header/logo situs (`.headertypes`, `.header`) | disembunyikan |
| Sidebar (`#sidebar-wrapper`) | disembunyikan; konten full width |
| Footer (`#footer-top`, `#footer1`, `.footer`, `#footer-credits`) | disembunyikan |
| Slider & featured box beranda (`#Slider`, `.odd-box`, `.featured-box-area`) | disembunyikan |
| **Breadcrumb + judul (`entry-title`) + byline (`post-info`) + label + tanggal + author** | disembunyikan |
| Share, komentar, related post, pager | disembunyikan |
| `#outer-wrapper`, `.main-content-wrapper`, `#content-wrapper`, `#mywrapper`, `#post-wrapper`, `.post`, `.post-body`, `.post-entry` | max-width/margin/padding/float dihapus → full width |
| `padding-top` body (dari skrip sticky topbar) | dinolkan |
| Pembungkus `<p>` pada body (khusus halaman statis berlabel landing) | dihilangkan |

## Utility

- `body.is-landing` — kelas pada `<body>` saat mode aktif. Tambahkan CSS sendiri:
  ```css
  body.is-landing .hero-saya { padding: 120px 24px; }
  ```
- `.landing-bleed` — elemen melebar penuh ke tepi layar (full-bleed):
  ```html
  <section class="landing-bleed bg-slate-900 py-24">...</section>
  ```

## Cara Kerja (Teknis)

1. `<head>` memuat `<style id='landing-mode-css'>`:
   - Anti-FOUC: `html.landing-pending` menyembunyikan chrome situs selama deteksi.
   - Aturan `body.is-landing` (sembunyikan chrome + full width).
2. Deteksi **url** (`data:view.url in {...}`) dipasang **server-side** langsung ke
   `<body expr:class='... is-landing'>` → tanpa kedip.
3. Deteksi **label** & **penanda judul** dilakukan via skrip di widget Blog
   (karena `data:post.labels` hanya bisa dibaca di dalam widget Blog):
   - label `landing`/`fullpage`, atau
   - `document.title` diawali `[landing]`/`[fullpage]`, atau
   - `location.pathname` cocok dengan daftar URL.
4. Bila cocok → `<body>` diberi kelas `is-landing`; anti-FOUC dilepas.
5. Pengaman: bila deteksi gagal, `landing-pending` dilepas otomatis setelah 1,5 detik
   sehingga chrome situs tampil normal.

## Mengubah Kata Kunci

- Label: ubah pada skrip deteksi di widget Blog (baris `lbl === 'landing' ...`).
- Penanda judul: ubah `'[landing]'` / `'[fullpage]'` pada skrip yang sama.
- URL: ubah pada `b:with value='data:view.url in {...}'` dan pada
  `landingPaths` di skrip.

## Berkas Terkait

- `theme-8996540688293237985.xml` — tema (sudah dimodifikasi).
- `theme-8996540688293237985.backup.xml` — cadangan tema asli.
- `landing-preview.html` — pratinjau visual + tombol toggle `is-landing`.
