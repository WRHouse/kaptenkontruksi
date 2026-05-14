# Kapten Kontruksi — Petunjuk Deploy ke Netlify

Folder ini **siap deploy** ke Netlify. Tinggal drag & drop.

## 📦 Isi Folder

```
deploy/
├── index.html              ← Website utama (https://domainmu/)
├── kk-data-baked.js        ← Data konten yg dilihat visitor (REPLACE saat update)
├── kaptenmalang/
│   ├── index.html          ← Backoffice admin (https://domainmu/kaptenmalang)
│   └── kk-data-baked.js    ← Copy data (untuk preview di admin)
├── netlify.toml            ← Konfigurasi Netlify (security headers + redirects)
├── _redirects              ← Pretty URL untuk /kaptenmalang
├── robots.txt              ← Block admin dari search engine
└── sitemap.xml             ← Sitemap untuk SEO
```

## 🚀 Step-by-step Deploy

### 1. Download folder ini
Klik download di Claude untuk dapat zip folder `deploy/` → extract di komputermu.

### 2. Buka Netlify Drop
👉 https://app.netlify.com/drop

(Buat akun gratis dulu kalau belum punya — bisa pakai email/Google/GitHub.)

### 3. Drag & drop folder
Drag folder `deploy/` (BUKAN file zip-nya — extract dulu) ke area drop di halaman Netlify.

⏳ Tunggu ~30 detik — Netlify upload semua file & baca `netlify.toml` otomatis.

### 4. Dapat URL otomatis
Setelah selesai, kamu dapat URL random seperti:
`https://radiant-puffin-abc123.netlify.app`

✅ **Test:**
- Buka URL → website utama muncul (pakai data default)
- Tambah `/kaptenmalang` di belakang → halaman login admin
- Login: `admin` / `kapten2025`

---

## 🔑 ALUR UPDATE KONTEN — PENTING!

Karena ini static site (no backend), update konten butuh 2 langkah:

### Langkah 1 — Edit di Admin
Buka `URL-mu/kaptenmalang` → edit apapun (tambah proyek, ganti foto, dll). Otomatis tersimpan di browser kamu.

⚠️ **Tapi visitor BELUM lihat update kamu** — data masih ada di browser admin doang.

### Langkah 2 — Publish ke Visitor
1. Di admin → tab **Pengaturan**
2. Klik **📦 Generate Deploy Bundle** → download `kk-data-baked.js`
3. Buka Netlify dashboard → Sites → site kamu → tab **Deploys**
4. Drag file `kk-data-baked.js` ke area drop (Netlify upload sebagai file individual)
5. **Selesai** — visitor refresh website langsung lihat update

> 💡 Alternatif drag-drop: bisa juga upload via Netlify CLI atau replace file di GitHub repo (kalau pakai cara auto-deploy).

---

## 🔐 PENTING — Ganti Password Admin

**Default login**: `admin / kapten2025`

Segera setelah live:
1. Buka `URL-mu/kaptenmalang`
2. Login dengan default
3. Tab **Pengaturan** → Update Login dengan password kuat
4. Logout & test login dengan password baru

---

## 💾 Backup Data

Data admin tersimpan di browser kamu (IndexedDB, ~ratusan MB). Backup berkala:

- **Pengaturan → Ekspor Data** → simpan file JSON
- Pindah ke browser/komputer lain → **Impor Data**

---

## 🛠️ Update Total Website (Bukan Cuma Konten)

Kalau yang berubah BUKAN cuma konten (misal: ada fitur baru, layout berubah, dll), perlu re-deploy seluruh folder:

### Cara 1 — Drag & drop ulang
1. Download folder `deploy/` baru dari Claude
2. Buka Netlify → site → **Deploys** → drag folder baru ke area drop

### Cara 2 — Netlify CLI
```bash
npm install -g netlify-cli
netlify deploy --prod --dir=deploy
```

### Cara 3 — Auto-deploy dari GitHub
Push folder ke GitHub repo → Netlify auto-build setiap push.

---

## 🔒 Fitur Keamanan di `netlify.toml`

- `X-Frame-Options: SAMEORIGIN` — anti clickjacking
- `X-Content-Type-Options: nosniff` — anti MIME sniff
- `Referrer-Policy: strict-origin-when-cross-origin` — privacy
- `/kaptenmalang/*` di-block dari search engine (X-Robots-Tag + robots.txt)
- `/admin` di-redirect ke `/kaptenmalang/` (kalau ada yang nyasar)

---

## ❓ FAQ

**Q: Kenapa harus upload `kk-data-baked.js` manual? Nggak bisa otomatis?**
A: Karena ini static site (gratis, ringan, cepat). Untuk auto-publish butuh backend (Firebase/Supabase) — bisa ditambah kalau perlu.

**Q: Bisa upload file `kk-data-baked.js` saja tanpa re-deploy semua?**
A: Bisa. Di Netlify dashboard → Deploys → "Browse files" pilih file individual. Atau pakai Netlify CLI: `netlify deploy --prod --dir=. --include=kk-data-baked.js`.

**Q: File `kk-data-baked.js` besar nggak?**
A: Tergantung jumlah foto. Realistis 5–50 MB. Netlify free tier handle dengan mudah.

---

Selamat live! 🎉
