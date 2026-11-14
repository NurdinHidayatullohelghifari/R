# R
R CONTROL
#  Remote Control — MVP

## Apa yang sudah berfungsi
- Server signaling Node.js + Socket.IO.
- Kode sesi acak satu kali dan pasangan host/viewer.
- Pemilik perangkat target harus menyetujui permintaan sebelum berbagi layar.
- Screen capture melalui `getDisplayMedia()` dan streaming WebRTC.
- Tombol demo mengirim event input melalui kanal data WebRTC.

## Batasan penting
Versi ini **belum melakukan kendali penuh sistem operasi**. Tombol demo hanya mengirim pesan input ke halaman browser pendamping; browser tidak dapat menyuntikkan mouse/keyboard ke aplikasi desktop lain. Untuk kontrol Windows penuh diperlukan agent native yang ditandatangani, autentikasi kuat, izin lokal, indikator sesi aktif, dan tombol stop. Android membutuhkan aplikasi native dengan izin MediaProjection dan Accessibility Service yang diaktifkan manual oleh pemilik perangkat. Jangan mengaktifkan akses tersembunyi atau tanpa persetujuan.

## Menjalankan di Windows 10/11
1. Instal Node.js LTS dari https://nodejs.org/
2. Ekstrak ZIP ini.
3. Buka folder `elghifari-remote-control`.
4. Buka CMD/PowerShell di folder tersebut.
5. Jalankan:
   ```bat
   npm install
   npm start
   ```
6. Buka `http://localhost:3000` pada browser.

## Mengakses lewat internet
Untuk penggunaan internet, deploy server ke VPS/hosting Node.js dan pasang domain dengan HTTPS. `getDisplayMedia()` memerlukan secure context (HTTPS; localhost dikecualikan). WebSocket harus menggunakan WSS. Atur firewall hanya untuk port HTTPS. Gunakan TURN server (misalnya coturn) untuk jaringan yang tidak dapat terhubung langsung; konfigurasi kredensial TURN sementara di `rtcConfig`.

## Uji dua perangkat
1. Buka URL HTTPS di perangkat target, klik **Buat sesi & kode**.
2. Buka URL yang sama di perangkat pengendali, masukkan kode, klik **Hubungkan**.
3. Pada target, periksa permintaan lalu klik **Setujui & bagikan layar** dan pilih layar/jendela.
4. Video akan muncul di perangkat pengendali. Klik **Hentikan berbagi layar** pada target untuk memutuskan.

## Catatan produksi
MVP ini belum cocok diekspos ke publik tanpa penguatan keamanan. Sebelum produksi: batasi CORS/origin, rate-limit pembuatan kode, simpan sesi dengan TTL, tambahkan autentikasi, validasi payload, HTTPS/WSS, TURN berautentikasi, logging minimal, dan uji keamanan.

