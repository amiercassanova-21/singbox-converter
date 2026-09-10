# CASSANOVA — Sing-box Multi Akun

Panel web untuk mengubah banyak link akun (VLESS, VMess, Trojan) menjadi satu file config sing-box siap pakai di Android.

Live: https://singbox.cassanova.my.id

---

## Yang dihasilkan

Struktur JSON mengikuti config yang sudah terbukti jalan:

- Grup `🌐 Pilih Mode` → `⚡ Auto Tercepat` → grup per protokol → `🎯 Primary/Backup/Tertiary` → `👆 Pilih Akun Manual`
- Inbound TUN saja: `sing-box`, `172.18.0.1/30`, mtu 1280, stack mixed
- DNS: `dns-local` udp 8.8.8.8 + `dns-remote` tls 1.1.1.1:853
- Clash API di `127.0.0.40:9090`, dashboard metacubexd
- Rule WhatsApp diletakkan di atas rule QUIC

Pilihan versi: reF1nd 1.13/1.14 dan SFA resmi SagerNet 1.12/1.13/1.14.
Rilis resmi core: https://github.com/SagerNet/sing-box/releases

---

## Cara deploy ke GitHub Pages

### 1. Siapkan file

File panel **harus** bernama `index.html`. Tanpa nama itu, halaman tidak terbuka otomatis.

### 2. Buat repo

1. Buka github.com, login
2. Tombol **+** kanan atas → **New repository**
3. Repository name: `singbox-converter`
4. Pilih **Public** (Pages gratis hanya untuk repo publik)
5. Centang **Add a README file**
6. **Create repository**

### 3. Unggah file

1. **Add file** → **Upload files**
2. **choose your files** → pilih `index.html`
3. **Commit changes**

> Periksa isinya setelah unggah. Buka
> `https://raw.githubusercontent.com/NAMA-AKUN/singbox-converter/main/index.html`
> Harus diawali `<!DOCTYPE html>`. Kalau isinya lain, berarti salah pilih file.

### 4. Nyalakan Pages

1. Tab **Settings** (kalau tidak kelihatan, tekan titik tiga di deretan tab)
2. Menu kiri → **Pages**
3. Source: **Deploy from a branch**
4. Branch: **main**, folder: **/ (root)** → **Save**
5. Tunggu 1–3 menit, muat ulang. Alamat muncul:
   `https://NAMA-AKUN.github.io/singbox-converter/`

### 5. Pasang domain sendiri

1. Di halaman Pages, isi **Custom domain**: `singbox.cassanova.my.id` → **Save**
2. Cloudflare → domain `cassanova.my.id` → **DNS** → **Add record**
3. Type **CNAME**, Name `singbox`, Target `NAMA-AKUN.github.io`
4. Proxy status: **DNS only** (awan abu-abu) → **Save**
5. Balik ke GitHub Pages, tunggu sertifikat terbit, lalu centang **Enforce HTTPS**

---

## Catatan penting

**Awan harus abu-abu dulu.** Kalau proxy Cloudflare menyala sebelum sertifikat GitHub terbit, sertifikat gagal dan Enforce HTTPS tidak bisa dicentang. Setelah HTTPS aktif, proxy boleh dinyalakan asal SSL/TLS Cloudflare dalam mode **Full** atau **Full (strict)** — jangan Flexible.

**Kalau halaman menampilkan isi lain** (misalnya config Clash), urutan pemeriksaan:

1. Cek isi `index.html` lewat alamat raw di atas — paling sering ini penyebabnya
2. Cek Proxy status record `singbox` harus DNS only
3. Cloudflare → **Workers Routes** pada zona, cari pola yang menangkap subdomain ini
4. Cek Worker yang memakai **Custom Domain** dengan nama subdomain ini (tidak muncul di Workers Routes)

**Sertifikat mandek.** Tekan **Remove** di Custom domain, tunggu semenit, isi ulang, **Save**. Itu memicu permintaan sertifikat baru.

---

## Cara memperbarui panel

1. Buka `index.html` di repo
2. Ikon pensil (Edit)
3. Hapus semua isi, tempel isi baru
4. **Commit changes**

Halaman ikut berubah dalam satu dua menit. Buka di tab penyamaran kalau masih tampil versi lama.

---

by: Amier Cassanova
