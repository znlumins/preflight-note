# Design Guide — Spartan Voks Friendly

## 1. Tujuan Design

Design Spartan Voks Friendly harus terasa sederhana, membantu, dan tidak mengintimidasi pengguna baru. Prioritasnya adalah:

1. Pengguna selalu tahu sedang berada di halaman apa.
2. Aksi utama mudah ditemukan.
3. Informasi penting dapat dipindai dengan cepat.
4. Tampilan tetap nyaman pada mobile.
5. Komponen cukup sederhana untuk diimplementasikan programmer junior.

## 2. Prinsip UX

- **Mulai dari satu aksi utama:** setiap halaman utama memiliki satu CTA paling menonjol.
- **Progressive disclosure:** tampilkan ringkasan dahulu, detail setelah pengguna memilih.
- **Bahasa manusia:** hindari pesan error teknis seperti stack trace.
- **Konsisten:** gunakan komponen dan istilah yang sama di seluruh halaman.
- **Feedback cepat:** setiap aksi yang membutuhkan waktu harus memiliki status.
- **Mobile-first:** mulai dari satu kolom, lalu perluas untuk layar lebih besar.

## 3. Information Architecture

```text
Beranda /
└── Daftar Item /items
    └── Detail Item /items/:id

Halaman pendukung:
- Not Found
- Error State
```

## 4. User Flow Utama

### Flow pengguna baru

1. Pengguna membuka beranda.
2. Pengguna membaca judul dan ringkasan.
3. Pengguna menekan “Lihat daftar”.
4. Pengguna melihat kartu item.
5. Pengguna membuka salah satu item.
6. Pengguna kembali ke daftar.

### Flow pencarian

1. Pengguna membuka daftar.
2. Pengguna mengisi input pencarian.
3. Daftar menampilkan hasil yang cocok.
4. Jika tidak ada hasil, tampilkan pesan dan tombol “Reset pencarian”.

### Flow error

1. Aplikasi gagal mengambil atau memproses data.
2. Tampilkan pesan singkat dan penyebab umum bila diketahui.
3. Tampilkan tombol “Coba lagi”.
4. Jangan menghapus konteks pengguna jika memungkinkan.

## 5. Daftar Screen

### Beranda

- Header.
- Hero: eyebrow opsional, judul, deskripsi, CTA.
- Preview manfaat atau 3 poin keunggulan.
- Footer sederhana.

### Daftar item

- Judul halaman dan deskripsi.
- Search input.
- Filter kategori opsional P1.
- Grid/list `ItemCard`.
- Loading, empty, no-result, dan error state.

### Detail item

- Breadcrumb atau tombol kembali.
- Judul dan metadata.
- Deskripsi lengkap.
- CTA utama jika ada.
- Konten terkait opsional P2.

### Not found

- Judul “Halaman tidak ditemukan”.
- Penjelasan singkat.
- Tombol kembali ke beranda.

## 6. Komponen

### Header

- Logo/nama produk di kiri.
- Link “Beranda” dan “Daftar”.
- Pada mobile, gunakan menu sederhana atau susun link dalam baris yang dapat wrap.
- Jangan membuat mega menu pada MVP.

### Button

Variasi minimum:

- `primary` untuk aksi utama.
- `secondary` untuk aksi pendamping.
- `ghost` untuk aksi ringan seperti kembali.

Aturan:

- Tinggi minimum 44px untuk target sentuh.
- Gunakan teks aksi, bukan hanya ikon.
- Tambahkan `aria-busy="true"` atau teks status ketika sedang memproses.

### ItemCard

- Judul maksimal 2 baris.
- Ringkasan maksimal 3 baris.
- Metadata menggunakan teks kecil dengan kontras cukup.
- Seluruh kartu boleh menjadi link jika pola tersebut konsisten.
- Hindari terlalu banyak tombol di dalam kartu.

### SearchInput

- Label terlihat: “Cari item”.
- Placeholder: “Contoh: onboarding”.
- Tombol reset hanya tampil jika ada query.
- Jangan mengandalkan placeholder sebagai satu-satunya label.

### State components

- `LoadingState`: indikator visual + teks.
- `EmptyState`: judul, penjelasan, CTA bila ada.
- `ErrorState`: judul, penjelasan, tombol retry.
- `SuccessMessage`: feedback singkat dan tidak menghalangi konten.

## 7. Layout Responsive Mobile-first

### Mobile: 320–767px

- Padding horizontal: 16px.
- Satu kolom.
- Header boleh wrap.
- Tombol utama full-width jika konteksnya form atau CTA utama.
- Hindari tabel lebar dan horizontal scrolling.

### Tablet: 768–1023px

- Container maksimal sekitar 720px.
- Grid item 2 kolom jika kartu tetap terbaca.
- Hero dapat memakai dua area jika tidak mengurangi keterbacaan.

### Desktop: 1024px ke atas

- Container maksimal 1120px.
- Grid item 3 kolom bila konten mencukupi.
- Spacing halaman lebih longgar.
- Jangan memperbesar font secara berlebihan.

## 8. Design Tokens

Nilai berikut adalah baseline. Gunakan token/variabel agar mudah diubah.

### Warna

```css
:root {
  --color-brand-600: #2563eb;
  --color-brand-700: #1d4ed8;
  --color-text-900: #172033;
  --color-text-600: #5b6475;
  --color-surface: #ffffff;
  --color-surface-muted: #f6f8fb;
  --color-border: #dce2ea;
  --color-success: #15803d;
  --color-warning: #b45309;
  --color-danger: #b42318;
  --color-focus: #f59e0b;
}
```

### Typography

- Font sans-serif sistem atau font project yang sudah tersedia.
- Body: 16px, line-height 1.5.
- Small text: 14px, line-height 1.4.
- H1 mobile: 32px, desktop: 44px.
- H2: 24–32px.
- Gunakan maksimal 3 level heading pada satu halaman.

### Spacing

Gunakan skala 4px: `4, 8, 12, 16, 24, 32, 48, 64px`.

### Radius dan shadow

- Small control: `8px`.
- Card: `12px`.
- Large panel: `16px`.
- Shadow card: `0 4px 16px rgba(23, 32, 51, 0.08)`.
- Hindari shadow berat pada semua elemen.

### Breakpoint

- `sm: 640px`.
- `md: 768px`.
- `lg: 1024px`.
- `xl: 1280px`.

## 9. Aturan Interaksi

- Hover hanya sebagai tambahan; fungsi utama harus jelas tanpa hover.
- Focus keyboard harus terlihat dengan outline warna focus.
- Tombol disabled tidak boleh terlihat seperti link aktif.
- Setelah submit, pertahankan nilai input jika terjadi error.
- Tampilkan loading pada aksi yang memerlukan waktu.
- Jangan menggunakan modal kecuali aksi benar-benar membutuhkan konfirmasi.
- Animasi maksimal 150–200ms dan tidak boleh menjadi syarat memahami UI.
- Hormati `prefers-reduced-motion`.

## 10. Accessibility Minimum

- Gunakan elemen semantic: `header`, `nav`, `main`, `section`, `footer`, `button`, dan `a` sesuai fungsi.
- Semua input memiliki label.
- Semua gambar informatif memiliki alt text; gambar dekoratif menggunakan alt kosong.
- Urutan heading tidak melompat tanpa alasan.
- Semua aksi utama bisa dilakukan dengan keyboard.
- Focus state terlihat jelas.
- Warna teks dan background harus memiliki kontras yang layak.
- Jangan menyampaikan status hanya dengan warna.
- Error form ditempatkan dekat field terkait dan dapat dibaca screen reader.
- Sediakan skip link jika halaman memiliki header panjang.

## 11. Copy dan Microcopy

Gunakan Bahasa Indonesia yang singkat dan langsung.

| Konteks | Copy |
|---|---|
| CTA utama | Lihat daftar |
| Membuka detail | Lihat detail |
| Loading | Memuat data… |
| Empty | Belum ada item untuk ditampilkan. |
| No result | Tidak ada hasil yang cocok. |
| Reset | Reset pencarian |
| Error | Data belum dapat dimuat. |
| Retry | Coba lagi |
| Not found | Halaman tidak ditemukan |
| Kembali | Kembali ke daftar |
| Success | Perubahan berhasil disimpan. |

Hindari copy seperti “Oops!!!”, “Something went wrong”, atau istilah teknis yang tidak membantu pengguna.

## 12. QA Visual dan Responsive

### Checklist layout

- [ ] Tidak ada horizontal overflow pada lebar 320px.
- [ ] CTA utama terlihat jelas pada mobile.
- [ ] Kartu memiliki tinggi dan spacing yang wajar.
- [ ] Teks panjang tidak merusak layout.
- [ ] Grid berubah dari satu kolom ke beberapa kolom dengan rapi.
- [ ] Header tetap dapat digunakan setelah resize.

### Checklist interaksi

- [ ] Semua link utama dapat diklik.
- [ ] Tombol memiliki state hover, focus, disabled, dan loading.
- [ ] Input dapat digunakan dengan keyboard.
- [ ] Tombol retry benar-benar mengulang proses.
- [ ] Tombol kembali tidak kehilangan konteks yang diharapkan.

### Checklist accessibility

- [ ] Tab order masuk akal.
- [ ] Focus terlihat.
- [ ] Label input terbaca.
- [ ] Error tidak hanya ditandai warna.
- [ ] Reduced motion dihormati.

### Browser minimum

Uji pada browser modern utama yang tersedia di lingkungan project. Tidak perlu membuat polyfill atau dukungan browser lama sebelum ada kebutuhan nyata.

## 13. Instruksi Implementasi untuk Junior atau Model Murah

1. Baca `PRD.md` dan kerjakan P0 saja.
2. Cari komponen atau style yang sudah ada sebelum membuat baru.
3. Buat satu komponen kecil per tanggung jawab.
4. Gunakan mock data lokal terlebih dahulu.
5. Implementasikan satu screen sampai selesai sebelum berpindah ke screen berikutnya.
6. Setelah setiap tahap, jalankan lint/build/test yang tersedia.
7. Jangan mengubah dependency tanpa alasan.
8. Jangan menambahkan fitur P1/P2 ketika P0 belum stabil.
9. Jika menemukan kebutuhan yang tidak jelas, gunakan asumsi terkecil dan catat di PR.
10. Pastikan nama, copy, dan state konsisten dengan dokumen ini.

## 14. Urutan Pengerjaan yang Disarankan

1. Token warna, typography, spacing, dan container.
2. Button, link, input, dan card.
3. Header dan layout global.
4. Beranda.
5. Daftar dan mock data.
6. Detail dan not found.
7. Loading, empty, error, dan success state.
8. Search sederhana.
9. Responsive QA dan accessibility.
10. Dokumentasi menjalankan project.
