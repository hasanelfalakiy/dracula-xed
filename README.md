# Dracula — Tema Xed-Editor

Tema [Xed-Editor](https://github.com/Xed-Editor/Xed-Editor) bergaya **Dracula**: latar gelap `#282a36` dengan aksen ungu, pink, dan cyan. Mencakup antarmuka aplikasi, editor, dan terminal.

> [!TIP]
> Dokumentasi resmi pembuatan tema: [Xed-Docs → Themes](https://xed-editor.github.io/Xed-Docs/docs/themes/#creating-custom-themes)

## Fitur

- Struktur `theme.json` sama persis dengan template Xed-Editor (key dan urutannya tidak diubah); hanya nilai warna yang diganti.
- Warna diambil dari `dracula.json`. Key yang tidak ada di sumber diisi dengan warna turunan palet Dracula, dengan **ungu terang `#bd93f9`** sebagai aksen dominan.
- **Latar seragam `#282a36`**: `background`, `surface`, `whole_background`, `line_number_background`, dan `line_divider` memakai warna yang sama sehingga tidak ada pita warna berbeda antara antarmuka dan editor.
- 85 aturan `tokenColors` (scope TextMate) lengkap dengan gaya huruf (italic, bold, underline).
- Palet terminal Dracula lengkap (`cursor`, `color0`–`color21`).
- Tervalidasi terhadap [skema resmi `theme.json`](https://github.com/Xed-Editor/Xed-Schemas).

## Palet warna

### Palet Dracula

| Peran | Warna |
|---|---|
| Latar | `#282a36` |
| Teks | `#F8F8F2` |
| Komentar, garis tepi | `#6272A4` |
| Seleksi | `#44475A` |
| Popup, panel gelap | `#21222C` |
| Keyword, storage, tag | `#FF79C6` |
| Fungsi, class, tipe | `#50FA7B` |
| Konstanta, angka | `#BD93F9` |
| String | `#F1FA8C` |
| Parameter, atribut | `#FFB86C` |
| Support, operator tipe | `#8BE9FD` |
| Error | `#FF5555` |

### Warna turunan (tidak ada di sumber)

| Key | Warna | Alasan |
|---|---|---|
| `primaryContainer` | `#6d4cae` | Ungu gelap dari `#bd93f9`; kontras 6:1 dengan `onPrimaryContainer` |
| `error` / `onError` | `#ff5555` / `#282a36` | Merah Dracula, teks memakai warna latar |
| `errorContainer` / `onErrorContainer` | `#6b2230` / `#ffb8b8` | Turunan merah Dracula |
| `outline` / `outlineVariant` | `#6272a4` / `#44475a` | Warna komentar dan seleksi Dracula |
| `cursor` (terminal) | `#f8f8f2` | Kursor putih khas Dracula |
| `selection_handle`, `completion_wnd_text_matched`, `diagnostic_tooltip_action`, `hover_text_highlighted`, `signature_text_highlighted_parameter` | `#bd93f9` | Aksen ungu |
| `highlighted_delimiters_background` | `#50bd93f9` | Ungu transparan untuk pasangan kurung |
| `matched_text_background` | `#55ffb86c` | Oranye Dracula untuk hasil pencarian |
| `current_line` | `#6644475a` | Seleksi Dracula dengan alpha, agar baris aktif terlihat tetapi seleksi tetap menonjol |
| `scroll_bar_thumb` / `_pressed` | `#996272a4` / `#cc6272a4` | Warna komentar dengan alpha |
| `text_inlay_hint_*` | `#343746` / `#9aa5ce` | Latar dan teks inlay hint yang redup |

## Struktur berkas

```
.
├── manifest.json   # Metadata paket (id, nama, versi, penulis, dll.)
├── theme.json      # Definisi warna: light & dark
├── README.md       # Dokumentasi ini
├── build.js        # Skrip build tanpa dependensi → output/<id>.xed
└── output/         # Hasil build (.xed)
```

### `theme.json`

Setiap mode (`light`, `dark`) memiliki empat bagian:

| Bagian | Fungsi |
|---|---|
| `baseColors` | Warna antarmuka aplikasi (`primary`, `surface`, `outline`, …) |
| `terminalColors` | Warna terminal: `cursor`, `color0`–`color21` (mode `dark` hanya `color0`–`color15`, mengikuti template) |
| `editorColors` | Warna komponen editor: latar, nomor baris, seleksi, popup, scrollbar, diagnostik |
| `tokenColors` | Aturan highlight sintaks berbasis scope TextMate |

### `manifest.json`

| Field | Nilai | Keterangan |
|---|---|---|
| `id` | `com.andi.dracula` | Wajib format *reverse-domain* huruf kecil (`a.b.c`) |
| `name` | `Dracula` | Nama tema di Store |
| `minAppVersion` | `88` | Versi minimum Xed-Editor |
| `inheritBase` | `false` | Seluruh `tokenColors` didefinisikan sendiri, tidak mewarisi tema dasar |

## Build & instalasi

```bash
node build.js
```

Menghasilkan `output/com.andi.dracula.xed` (ZIP berisi `theme.json`, `manifest.json`, dan `README.md`).

**Instal lokal:** Xed-Editor → **Settings → Store → Install from storage** → pilih berkas `.xed`.

**Termux / Android:**

```bash
pkg install nodejs
node build.js
```

## Catatan pemetaan dari skema sumber

1. **Key mengikuti template.** Key yang ada di sumber tetapi tidak ada di template (`surfaceDim`, `surfaceBright`, `surfaceContainer*`, `foreground`/`background` terminal, `text_highlight_*`, `current_row_border`, dan `color16`–`color21` pada mode `dark`) tidak dimasukkan.
2. **Mode `light`** mengikuti sumber apa adanya (latar tetap `#282a36`), karena sumber memakai warna editor dan token yang sama untuk kedua mode.
3. **`strikethrough` dan `underline`** di sumber bernilai angka `0` (tidak valid menurut skema); diganti warna teks `#f8f8f2` agar garis bawah komposisi keyboard (IME) tetap terlihat.
4. **`fontStyle`** bernilai `normal` atau `regular` di sumber diubah menjadi `none`, karena itulah nilai yang valid menurut skema Xed.
5. **Format warna:** `baseColors` dan `editorColors` memakai `#RRGGBB` atau `#AARRGGBB` (alpha di depan); `tokenColors` memakai `#RRGGBB` atau `#RRGGBBAA`.
6. **Kontras `onPrimary`:** sumber menetapkan `onPrimary` `#f8f8f2` di atas `primary` pink `#ff77cc` (rasio sekitar 2,3:1). Nilai sumber dipertahankan; untuk teks tombol yang lebih tegas, ubah `onPrimary` menjadi `#282a36`.

## Kustomisasi

- Ubah warna di `theme.json`, lalu jalankan `node build.js`.
- Sebelum publikasi, perbarui di `manifest.json`: `author`, `repository`, dan `version`.
- Pertahankan baris `$schema` agar editor dapat memvalidasi berkas.

## Publikasi

Unggah berkas `.xed` hasil build ke [xed-editor.app](https://xed-editor.app).

## Lisensi

MIT. Palet warna © [Dracula Theme](https://draculatheme.com) (MIT).
