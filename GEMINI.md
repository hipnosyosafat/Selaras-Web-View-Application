# Panduan Pengembangan & Desain Sistem — SELARAS WebApp

Dokumen aturan ini mendefinisikan standar desain, interaksi, dan kaidah teknis purwarupa aplikasi **SELARAS (Habit & Self-Development)** agar tetap konsisten pada seluruh sesi pengembangan.

---

## 1. Identitas Visual & Filosofi Mindfulness

* **Warna Utama (*Primary*)**:
  * Gunakan kode warna hijau **`#2FB369`** (bukan hijau gelap atau default Tailwind) untuk seluruh tombol aksi utama, border tombol aktif, dan sorotan antarmuka.
  * Warna turunan gelap (*hover/active*): `#249B58`.
  * Warna latar aksen transparan: `rgba(234, 251, 243, 0.75)` atau `bg-[#EAFBF3]/70`.
* **Latar Belakang (*Zen Wallpaper*)**:
  * Gunakan gambar `background.png` (720×1600 organic zen waves) terpasang `cover` & `center` di container utama (`<main class="bg-selaras-gradient">`) dengan fallback warna `#9DE2CD`.
  * Seluruh layar di dalamnya (`screen-dashboard`, `screen-add-habit`, `screen-auth`, `screen-onboarding`) berlatar transparan (`bg-transparent`) agar kartu-kartu *frosted glass* (`backdrop-blur`) melayang secara anggun di atas gelombang wallpaper tanpa *flicker*.
* **Tone of Voice & Mikro-Kopian (*Mindful Communication*)**:
  * Hindari bahasa menghakimi atau *guilt-trip*.
  * Hari di luar jadwal aktif disebut **"Hari Istirahat"** (*rest day*).
  * Penghapusan kebiasaan menggunakan istilah **"Lepaskan Kebiasaan"** dengan pesan *"Kebiasaan telah dilepaskan 🍃"*.

---

## 2. Status Komponen Ikon & Checklist

### A. Selector Ikon pada Halaman Tambah Kebiasaan
Urutan 5 piktogram dari kiri ke kanan:
1. `Daun icon.png` (`leaf`)
2. `buku icon.png` (`book`)
3. `droplet icon.png` (`water`)
4. `love icon.png` (`heart`)
5. `electric icon.png` (`bolt`)

* **Kondisi Belum Dipilih (*Unselected*)**:
  * Border: `2px solid #2FB369`
  * Latar: `rgba(234, 251, 243, 0.75)`
  * Ikon: Warna hijau asli `#2FB369`
* **Kondisi Dipilih (*Selected*)**:
  * Latar: Solid `#2FB369`
  * Border: `2.5px solid #ffffff`
  * Ikon: Putih murni `#ffffff` menggunakan filter CSS `filter: brightness(0) invert(1) !important`
  * Elevasi: `box-shadow: 0 8px 20px -3px rgba(47, 179, 105, 0.45)` dan `scale-105`

### B. Kartu Kebiasaan di Dashboard (Sinkron dengan Checklist)
* **Belum di-Checklist (`!isCompleted`)**:
  * Badge Ikon: Outline hijau `border-2 border-[#2FB369] bg-[#EAFBF3]/70` dengan ikon hijau `#2FB369`.
  * Tombol Checklist: Lingkaran outline `border-2 border-[#2FB369]/70 bg-transparent`.
* **Setelah di-Checklist (`isCompleted`)**:
  * Badge Ikon: Solid hijau `border-2 border-white bg-[#2FB369] shadow-btn` dengan ikon putih murni `.icon-filter-white`.
  * Tombol Checklist: Solid hijau `border-2 border-[#2FB369] bg-[#2FB369] text-white shadow-btn` dengan ikon centang putih.
  * Judul: Teks coret halus `line-through text-selaras-text/50`.

---

## 3. Standar Area Sentuh (*Touch Target*) & Responsivitas Mobile

* **Standar Ukuran Sentuh (Material Design 3 & WCAG)**:
  * Tombol pilihan hari (*Hari Aktif*): Wajib berukuran minimal **tinggi 48px (`h-12`)** dengan sudut `rounded-[14px]` dan `gap-1 sm:gap-1.5`.
  * Tombol pilihan ikon (*Icon Selector*): Wajib berukuran minimal **`w-[54px] h-[54px]`** dengan sudut `rounded-2xl` dan ukuran gambar di dalamnya `w-7 h-7` (28px).
* **Kesiapan Android WebView**:
  * Seluruh input teks (`input`, `textarea`) wajib memiliki `font-size: 16px !important` untuk mencegah *auto-zoom* liar.
  * Mendukung *notch* dan *gesture bar* melalui Safe-Area Insets (`env(safe-area-inset-top)` & `env(safe-area-inset-bottom)`).
  * Nonaktifkan sorotan sentuh biru via `-webkit-tap-highlight-color: transparent` dan seleksi teks tak sengaja via `user-select: none`.

---

## 4. Alur Bisnis & Error Prevention (Tanpa Alert Browser)

* **Larangan Alert Native Browser**: Dilarang menggunakan `alert()` atau `confirm()` bawaan browser.
* **Inline Error Prevention**:
  * Input/elemen yang belum dipilih disorot dengan border merah lembut `border-red-400`.
  * Menampilkan pesan bantuan di bawah elemen terkait.
  * Melakukan *smooth scroll* otomatis ke input yang terlewat pertama kali.
  * Menghapus pesan error seketika (*live dismiss*) saat pengguna mulai berinteraksi dengan elemen tersebut.
* **Aturan Hari Aktif (BR-02)**:
  * Kebiasaan hanya dapat dicentang pada hari yang sesuai dengan jadwal `activeDays`. Hari di luar jadwal otomatis berstatus "Hari Istirahat" dan tombol dinonaktifkan tanpa mempengaruhi persentase *progress ring*.

