# PRD — Spartan Voks Friendly

## 1. Ringkasan

**Spartan Voks Friendly** adalah versi reimplementation/cloning dari project `spartan-voks-mockup` dengan pengalaman pengguna yang lebih sederhana, jelas, dan nyaman di desktop maupun mobile.

Dokumen ini dibuat agar dapat dikerjakan oleh programmer junior atau model coding berbiaya rendah. Implementasi harus memprioritaskan alur utama, komponen sederhana, dan mock data sebelum integrasi backend nyata.

## 2. Asumsi

- Repository saat ini bernama `spartan-voks-mockup`.
- Informasi produk dan struktur fitur yang lengkap belum tersedia di repository saat dokumen ini dibuat.
- Karena itu, dokumen ini mendefinisikan baseline MVP yang aman: halaman beranda, daftar konten, detail konten, pencarian/filter sederhana, dan feedback UI.
- Detail domain, API, autentikasi, serta sumber data asli harus dikonfirmasi sebelum implementasi lanjutan.
- Nama repository baru yang diusulkan: `spartan-voks-friendly`.

## 3. Tujuan

### Tujuan pengguna

- Memahami fungsi aplikasi tanpa perlu membaca instruksi panjang.
- Menemukan konten atau item yang dibutuhkan dengan cepat.
- Menggunakan aplikasi dengan nyaman dari layar mobile.
- Mendapatkan feedback yang jelas setelah melakukan aksi.

### Tujuan produk

- Menghasilkan MVP yang mudah dipahami dan mudah dirawat.
- Mengurangi kebingungan melalui navigasi dan copy yang konsisten.
- Menyediakan fondasi yang siap dikembangkan ketika kebutuhan domain sudah final.

## 4. Masalah yang Diselesaikan

- Pengguna tidak selalu tahu harus mulai dari mana.
- Informasi penting dapat sulit ditemukan jika layout tidak terstruktur.
- State loading, kosong, error, dan berhasil sering tidak dijelaskan dengan baik.
- Tampilan desktop yang tidak mobile-first menyulitkan pengguna ponsel.

## 5. Target Pengguna dan Use Case

### Target pengguna

1. **Pengguna baru** — membutuhkan orientasi singkat dan tombol aksi utama yang jelas.
2. **Pengguna rutin** — ingin langsung mencari dan membuka item.
3. **Pengguna mobile** — menggunakan aplikasi dalam kondisi layar kecil dan koneksi yang mungkin tidak stabil.

### Use case utama

- Membuka aplikasi dan memahami tujuan halaman.
- Melihat daftar item.
- Mencari item berdasarkan kata kunci.
- Membuka detail item.
- Kembali ke daftar tanpa kehilangan konteks pencarian.
- Mencoba kembali ketika terjadi error.

## 6. Scope MVP

### P0 — wajib ada

- Halaman beranda dengan value proposition singkat.
- Tombol aksi utama menuju daftar item.
- Halaman daftar item dengan mock data.
- Kartu item yang konsisten.
- Halaman detail item.
- Navigasi desktop dan mobile.
- Loading, empty, error, success, dan disabled state.
- Responsive layout mobile-first.
- Semantic HTML dan keyboard navigation dasar.

### P1 — dikerjakan setelah P0 stabil

- Pencarian berdasarkan judul atau nama item.
- Filter sederhana berbasis kategori.
- Empty state khusus ketika hasil pencarian tidak ditemukan.
- Toast atau inline feedback setelah aksi pengguna.
- Persistensi query pencarian saat kembali dari halaman detail.

### P2 — opsional

- Autentikasi.
- Backend/API nyata.
- Favorit atau bookmark.
- Pagination atau infinite scroll.
- Analytics.
- Tema gelap.

## 7. User Stories dan Acceptance Criteria

### US-01 — Memahami beranda

**Sebagai** pengguna baru, **saya ingin** melihat penjelasan singkat dan satu aksi utama, **agar** saya tahu langkah berikutnya.

Acceptance criteria:

- Judul utama menjelaskan manfaat aplikasi.
- Terdapat deskripsi maksimal 2–3 kalimat.
- Terdapat satu tombol utama yang terlihat tanpa scrolling pada mobile umum.
- Tombol utama mengarah ke halaman daftar.

### US-02 — Melihat daftar item

**Sebagai** pengguna, **saya ingin** melihat daftar item dalam format kartu, **agar** saya dapat memilih item yang relevan.

Acceptance criteria:

- Data ditampilkan dari mock data lokal.
- Setiap kartu memiliki judul, ringkasan, metadata singkat, dan tombol/link detail.
- Kartu dapat dipakai dengan keyboard.
- Pada mobile, kartu tersusun satu kolom.

### US-03 — Melihat detail item

**Sebagai** pengguna, **saya ingin** membuka detail item, **agar** saya dapat memahami informasinya sebelum melakukan aksi.

Acceptance criteria:

- Detail menampilkan judul, deskripsi, metadata, dan aksi utama bila tersedia.
- Ada tombol/link kembali ke daftar.
- Jika ID item tidak ditemukan, tampilkan halaman error/not found yang jelas.

### US-04 — Mencari item

**Sebagai** pengguna, **saya ingin** mencari item berdasarkan kata kunci, **agar** saya tidak perlu membaca seluruh daftar.

Acceptance criteria:

- Input memiliki label yang jelas.
- Hasil berubah sesuai kata kunci setelah submit atau perubahan input yang sederhana.
- Pesan empty state tampil jika tidak ada hasil.
- Tombol reset menghapus pencarian.

### US-05 — Mendapat feedback

**Sebagai** pengguna, **saya ingin** mengetahui status aksi saya, **agar** saya tidak mengira aplikasi macet.

Acceptance criteria:

- Loading memiliki indikator dan teks singkat.
- Error menjelaskan masalah dengan bahasa manusia.
- Error menyediakan tombol “Coba lagi” jika relevan.
- Tombol yang sedang diproses menjadi disabled.

## 8. Halaman dan Fitur Inti

1. `/` — Beranda.
2. `/items` — Daftar item, pencarian, dan filter sederhana.
3. `/items/:id` — Detail item.
4. Not found — Halaman untuk route atau item yang tidak ditemukan.
5. Komponen bersama — Header, tombol, card, input, badge, empty state, error state, loading state.

## 9. State yang Wajib Dirancang

- **Loading:** skeleton sederhana atau spinner dengan teks “Memuat data…”.
- **Empty:** “Belum ada item untuk ditampilkan.”
- **No search result:** “Tidak ada hasil yang cocok. Coba kata kunci lain.”
- **Error:** “Data belum dapat dimuat. Silakan coba lagi.”
- **Success:** feedback singkat setelah aksi berhasil.
- **Disabled:** tombol tidak dapat ditekan saat proses berlangsung.
- **Mobile:** navigasi dan kartu tetap terbaca pada lebar sekitar 320px.

## 10. Data dan Mock Backend

Gunakan data lokal terlebih dahulu. Contoh bentuk data:

```ts
type Item = {
  id: string;
  title: string;
  summary: string;
  description: string;
  category?: string;
  updatedAt?: string;
};
```

Aturan:

- Sediakan minimal 6 mock item.
- Pisahkan data mock dari komponen UI.
- Buat fungsi repository/service sederhana agar penggantian ke API nyata mudah dilakukan.
- Jangan membuat autentikasi atau database sebelum kebutuhan dikonfirmasi.

## 11. Non-goals

- Tidak membuat backend produksi pada tahap MVP.
- Tidak menyalin aset atau identitas merek pihak lain tanpa izin.
- Tidak menambahkan fitur sosial, pembayaran, chat, atau dashboard admin.
- Tidak membuat sistem design yang kompleks.
- Tidak melakukan optimasi lanjutan sebelum alur P0 berfungsi.

## 12. Definition of Done

- Semua halaman P0 dapat dibuka dari navigasi.
- Tidak ada link utama yang rusak.
- Mock data tampil konsisten.
- Semua state wajib dapat diuji secara manual.
- Layout diuji pada mobile dan desktop.
- Fokus keyboard terlihat.
- Tidak ada error console yang diketahui pada alur normal.
- Copy UI menggunakan Bahasa Indonesia yang jelas.
- README atau instruksi menjalankan project diperbarui jika diperlukan.

## 13. Rencana Implementasi Bertahap

### Tahap 1 — Persiapan

1. Cek struktur project dan perintah menjalankan aplikasi.
2. Catat framework dan komponen yang sudah tersedia.
3. Tambahkan data mock dan tipe `Item`.

### Tahap 2 — Shell aplikasi

1. Buat layout utama.
2. Buat header dan navigasi.
3. Tambahkan routing untuk beranda, daftar, dan detail.

### Tahap 3 — Beranda dan daftar

1. Implementasikan hero sederhana.
2. Implementasikan `ItemCard`.
3. Tampilkan daftar dari mock data.
4. Tambahkan loading dan empty state.

### Tahap 4 — Detail dan pencarian

1. Implementasikan halaman detail.
2. Tambahkan pencarian lokal.
3. Tambahkan not found dan no-result state.

### Tahap 5 — Responsive dan aksesibilitas

1. Uji lebar 320px, 768px, dan desktop.
2. Perbaiki focus state dan label form.
3. Pastikan tombol dapat digunakan dengan keyboard.

### Tahap 6 — QA dan dokumentasi

1. Jalankan lint, test, dan build yang tersedia.
2. Cek semua state secara manual.
3. Perbarui README.
4. Buat PR kecil per tahap bila workflow repository mendukung.

## 14. Risiko dan Pertanyaan Terbuka

- Apa domain dan tujuan asli project Voks?
- Fitur mockup mana yang wajib dipertahankan?
- Apakah ada API atau sumber data resmi?
- Apakah pengguna membutuhkan login?
- Apakah nama `Spartan Voks Friendly` sudah disetujui?
- Apakah repository boleh diganti nama menjadi `spartan-voks-friendly`?
- Aset, logo, dan konten mana yang boleh digunakan?

Jika jawaban belum tersedia, lanjutkan hanya dengan scope P0 dan tandai keputusan sebagai asumsi.
