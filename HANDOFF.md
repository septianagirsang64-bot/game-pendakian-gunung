# SERAH TERIMA: Pendakian Pulau Santai
Dokumen ini untuk melanjutkan proyek dengan akun/chat Claude lain. Unggah file ini bersama `www/index.html` (atau seluruh zip), lalu tempel "Prompt lanjutan" di bagian bawah.

## Ringkasan
Game 3D mendaki gunung, berbasis satu file HTML (`www/index.html`, Three.js r128, tanpa internet). Dibungkus jadi APK dengan Capacitor, dibuild lewat GitHub Actions (`.github/workflows/apk.yml`). Target: HP rendah-menengah, mode landscape. Bahasa antarmuka: Indonesia.

## Struktur proyek
- `www/index.html`: SELURUH kode game (HTML, CSS, JS). Satu-satunya file yang berubah saat mengembangkan game.
- `www/lib/three.min.js`: disalin otomatis dari npm (`three@0.128.0`) saat build. Jangan diedit.
- `package.json`, `capacitor.config.json`: konfigurasi build APK (appId `com.pulausantai.pendakian`).
- `.github/workflows/apk.yml`: build APK debug otomatis di GitHub (Actions), artifact `pendakian-apk`.

## Peta kode di `www/index.html` (cari dengan nama ini)
- `height(x,z)`: bentuk dunia. `R=300` pulau, gunung di (150,-190) tinggi 100 sigma 75, desa di (0,10), danau (-45,35). Juga rata-kan area jurang (`RV`) dan buat parit `ravAt()`.
- `RV={x:89.5,z:-104,h:42}`: lokasi jurang dan jembatan. `ICE[]` + `iceAt()`: zona es licin (juga mewarnai tanah).
- Tanah: `TS=720, SEG=150`; warna vertex (batu, salju, es, jurang) di loop pewarnaan.
- `groundAt(x,z,py)` + `platforms[]` ({x,z,hw,hd,top}, kotak sejajar sumbu): permukaan yang bisa diinjak, dipakai jembatan.
- `buildAvatar(o)` + tekstur wajah (`faceMat`): karakter bergaya anime (pemain `style:'cap'` = rambut biru jambul, ransel, syal), NPC `style:'hair'`.
- `summit`: titik puncak (dicari otomatis). Bendera di puncak.
- Blok `/* sistem pendakian */` (sebelum `stepPlayer`):
  - `CAMPS` (Pos 1-3 + Puncak, tenda dan bendera), `CP` (titik simpan, disimpan di localStorage `pps_reach`, `pps_cp`).
  - `obstacles(dt,hi)`: batu menggelinding, angin, dingin.
  - `climb(dt)`: stamina, mdpl (tinggi x 28), kabut menurut ketinggian, drain memanjat, jatuh ke jurang, simpan progres.
- `stepPlayer(dt)`: gerak pemain; stamina membatasi lari, es mengurangi traksi (`k`).
- HUD: chip `hSt` (stamina), `hAlt` (mdpl); `#cold` efek dingin; tombol "Ke pos" = pulang ke titik simpan.
- Performa HP rendah: antialias mati, bayangan 512 PCF, pixelRatio maks 1.25, minimap tiap 3 frame.

## Sudah ada
Gaya referensi (gambar Roblox mountain): lereng hijau (batu hanya di atas ketinggian 58), jalur tanah lurus desa-puncak (warna vertex), hutan pinus 420 pohon, 14 puncak bersalju jauh di cakrawala, 8 pendaki NPC berransel di tiap pos (`HK[]`, `o.hair` untuk warna rambut). Belum: jalur berkelok, tekstur kayu/kotak plaid, cahaya hangat/sinematik.
Obby: 4 arena lava (`AR[]`, `arenaTick`) di jalur menuju gunung (d=215,195,175,155 dari pusat gunung): batu loncat, palang berputar, platform bergerak, platform menghilang. Tiap arena punya bendera checkpoint hijau. `respawn(msg)` = kembali ke `CP`.
Visual: tekstur detail tanah, salju turun di ketinggian (`snowTick`), awan melayang (`cloudsG`), 170 pinus dan 220 batu besar instanced di lereng (blok `/* visual */`).
Stamina, 3 pos + puncak (titik simpan), altimeter, kabut/dingin menurut ketinggian, batu menggelinding, angin, udara dingin, es licin, jurang + jembatan tali, lereng curam menguras stamina, karakter anime, dunia besar, offline, simpan progres.

## Belum / ide lanjutan (urut prioritas)
1. Gameplay: parkour batu loncat, dinding panjat dengan pegangan, tali, jembatan goyang, jurang kedua, badai salju.
2. Tampilan: tekstur, pohon/batu lebih detail, efek salju (partikel), cahaya, air terjun.
3. Progres: level/banyak gunung, koin, toko baju dan perlengkapan, misi warga, lencana, papan waktu.
4. Polesan: layar judul, pengaturan kualitas grafis (rendah/sedang/tinggi), ikon dan layar pembuka APK, suara angin dan langkah salju.

## Catatan teknis
- Kode ditulis gaya ES5 (`var`, `function`); pertahankan agar aman di WebView lama.
- Tidak boleh pakai localStorage selain yang sudah ada dengan try/catch.
- Setelah edit: cek sintaks (`node --check` pada isi `<script>`), lalu commit ke GitHub, build APK otomatis jalan.
- Angka keseimbangan (stamina, kekerapan batu, angin, dingin) masih tebakan, belum diuji di HP; sesuaikan setelah dicoba.
- Cara upload: isi folder ke repo GitHub; folder `.github` dibuat manual via Add file > Create new file bila tidak ikut ter-upload.

## Prompt lanjutan (tempel di chat baru)
"Saya melanjutkan proyek game pendakian ini. Baca HANDOFF.md dan www/index.html terlampir. Lanjutkan dari daftar 'Belum / ide lanjutan' nomor [ISI NOMOR]. Ubah hanya www/index.html, jaga agar ringan untuk HP rendah, dan beri file hasil akhir berupa zip proyek lengkap."
