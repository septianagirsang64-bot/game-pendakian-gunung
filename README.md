# Pendakian Pulau Santai - proyek APK

## Cara termudah (tanpa Android Studio): GitHub Actions
1. Buat repo GitHub, upload seluruh isi folder ini (termasuk folder .github).
2. Buka tab Actions, jalankan "Build APK" (otomatis jalan setiap push).
3. Unduh `pendakian-apk` dari hasil build, ekstrak, lalu pasang `app-debug.apk` di HP.

## Cara lokal (butuh Node 20, JDK 17, Android SDK)
    npm run setup      # sekali saja: pasang dependensi, salin three.js, buat proyek android
    npm run apk        # hasil: android/app/build/outputs/apk/debug/app-debug.apk
Tambahkan `android:screenOrientation="sensorLandscape"` pada <activity> di
android/app/src/main/AndroidManifest.xml agar layar selalu landscape.

## Uji cepat di browser
    npm install && npm run prebuild && npx serve www
