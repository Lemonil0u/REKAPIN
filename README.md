# APLIKASI PRESENSI SMAN 6
## Panduan Instalasi, Menjalankan, Pengembangan, Backup, dan GitHub

Dokumen ini adalah panduan lengkap untuk menyiapkan aplikasi presensi SMAN 6 pada komputer Windows baru, mulai dari instalasi Node.js, Node-RED, PostgreSQL, pgAdmin, Visual Studio Code, sampai menjalankan aplikasi dan mengelolanya melalui GitHub.

---

# 1. Gambaran Sistem

Aplikasi menggunakan arsitektur:

```text
Browser
  ├── Operator Sekolah
  ├── Guru Mata Pelajaran
  └── Wali Kelas
          |
          | HTTP
          v
      Node-RED
   Backend / REST API
          |
          | SQL
          v
     PostgreSQL
     absen_smansix
          |
          v
       Telegram
```

Komponen:

| Komponen | Fungsi |
|---|---|
| Node.js | Runtime yang dibutuhkan Node-RED |
| Node-RED | Backend, API, dan logic aplikasi |
| PostgreSQL | Database |
| pgAdmin | Antarmuka untuk mengelola PostgreSQL |
| Visual Studio Code | Editor source code dan project |
| HTML/CSS/JavaScript | Frontend aplikasi |
| Telegram Bot | Notifikasi presensi |

---

# 2. Struktur Database

Database:

```text
absen_smansix
```

Tabel:

```text
users
guru
kelas
siswa
mata_pelajaran
wali_kelas
guru_mapel
sesi
presensi
```

Hubungan utamanya:

```text
users
  |
  v
guru
  ├── wali_kelas ──> kelas ──> siswa
  |
  └── guru_mapel ──> mata_pelajaran
                 └─> kelas

sesi
  |
  v
presensi
  |
  v
siswa
```

---

# 3. Instalasi Node.js

## 3.1 Download

Buka:

https://nodejs.org/en/download

Pilih versi **LTS** untuk Windows 64-bit.

Download installer `.msi`.

## 3.2 Install

Jalankan installer.

Ikuti:

```text
Next
→ Accept License
→ Next
→ gunakan lokasi default
→ Next
→ Install
→ Finish
```

## 3.3 Cek instalasi

Buka Command Prompt:

```text
Windows + R
```

ketik:

```text
cmd
```

Kemudian:

```bash
node --version
```

dan:

```bash
npm --version
```

Jika keduanya menampilkan versi, Node.js berhasil.

---

# 4. Instalasi Node-RED

Buka Command Prompt.

Jalankan:

```bash
npm install -g node-red
```

Setelah selesai cek:

```bash
node-red --version
```

Jika versi muncul, Node-RED berhasil dipasang.

Dokumentasi Windows:

https://nodered.org/docs/getting-started/windows

---

# 5. Menjalankan Node-RED

Setiap kali ingin menjalankan server:

```bash
node-red
```

Tunggu sampai muncul informasi bahwa server berjalan pada:

```text
http://127.0.0.1:1880/
```

Jangan tutup Command Prompt tersebut selama aplikasi digunakan.

Buka browser:

```text
http://localhost:1880
```

Jika editor Node-RED muncul, Node-RED sudah berjalan.

---

# 6. Instalasi PostgreSQL

Download PostgreSQL:

https://www.postgresql.org/download/windows/

Gunakan installer Windows.

Saat instalasi, pilih:

```text
PostgreSQL Server
pgAdmin
Command Line Tools
```

Gunakan port:

```text
5432
```

Username:

```text
postgres
```

Buat password PostgreSQL dan simpan dengan aman.

Setelah instalasi selesai, buka pgAdmin.

Pastikan PostgreSQL Server dapat terkoneksi.

---

# 7. Membuat Database

Di pgAdmin:

```text
Servers
  ↓
PostgreSQL
  ↓
Databases
  ↓
Klik kanan
  ↓
Create
  ↓
Database
```

Nama database:

```text
absen_smansix
```

Owner:

```text
postgres
```

Klik Save.

---

# 8. Memasukkan Database dari File SQL

Repository menyediakan file:

```text
absensi_smansix.sql
```

Jika menggunakan file `.sql`:

1. Buka database `absen_smansix`.
2. Klik `Tools`.
3. Pilih `Query Tool`.
4. Buka file `absensi_smansix.sql`.
5. Jalankan dengan tombol Execute atau F5.
6. Tunggu sampai selesai.
7. Refresh `Schemas > public > Tables`.

Harus terlihat 9 tabel:

```text
guru
guru_mapel
kelas
mata_pelajaran
presensi
sesi
siswa
users
wali_kelas
```

---

# 9. Jika Menggunakan File Backup PostgreSQL

Jika file yang dimiliki adalah:

```text
absen_smansix.backup
```

buat database `absen_smansix`, kemudian:

```text
Klik kanan database
→ Restore
```

Pilih file `.backup`.

Perbedaan:

```text
.sql
→ Query Tool

.backup
→ Restore
```

---

# 10. Instalasi Visual Studio Code

Download:

https://code.visualstudio.com/

Install VS Code.

Setelah selesai:

```text
File
→ Open Folder
```

Pilih folder repository aplikasi.

VS Code digunakan untuk mengedit:

```text
HTML
CSS
JavaScript
Node-RED flow
README
Git
```

---

# 11. Struktur Repository yang Disarankan

Gunakan struktur:

```text
absen-smansix/
│
├── README.md
│
├── database/
│   └── absensi_smansix.sql
│
├── node-red/
│   └── flows.json
│
├── web/
│   ├── login.html
│   ├── operator.html
│   ├── guru.html
│   ├── wali-kelas.html
│   └── assets/
│       └── logosman6m.png
│
└── docs/
```

File yang sekarang sudah disiapkan:

```text
absensi_smansix.sql
flows.json
guru.html
login.html
logosman6m.png
operator.html
wali-kelas.html
```

dapat ditempatkan ke struktur tersebut.

---

# 12. Install Node-RED PostgreSQL Node

Buka Command Prompt.

Masuk ke folder Node-RED:

```bash
cd C:\Users\<USERNAME>\.node-red
```

Install:

```bash
npm install node-red-contrib-postgresql
```

---

# 13. Install Node-RED Telegram Node

Masih di folder `.node-red`:

```bash
npm install node-red-contrib-telegrambot
```

---

# 14. Install bcrypt

Login aplikasi menggunakan bcrypt.

Jalankan:

```bash
npm install bcrypt
```

Setelah instalasi package selesai, restart Node-RED.

---

# 15. Import Flow Node-RED

Jalankan:

```bash
node-red
```

Buka:

```text
http://localhost:1880
```

Di Node-RED:

```text
Menu ☰
→ Import
→ File
```

Pilih:

```text
flows.json
```

Setelah flow masuk:

```text
Deploy
```

---

# 16. Konfigurasi PostgreSQL pada Node-RED

Buka salah satu node PostgreSQL.

Connection:

```text
Host     : 127.0.0.1
Port     : 5432
Database : absen_smansix
User     : postgres
Password : password PostgreSQL
```

Klik Update/Done.

Kemudian:

```text
Deploy
```

Jika node PostgreSQL menggunakan configuration node yang sama, cukup konfigurasi connection tersebut.

---

# 17. Konfigurasi Telegram

Buka node:

```text
Telegram sender
```

Konfigurasi:

```text
Bot Name : absensi
Token    : TOKEN BOT
Chat ID  : CHAT ID GRUP
```

Token Telegram adalah rahasia.

**Jangan upload token ke GitHub.**

Pada sistem yang sudah dibuat, Telegram digunakan untuk mengirim notifikasi ketika presensi mempunyai siswa dengan status:

```text
Sakit
Izin
Tanpa Keterangan
```

Jika seluruh siswa hadir, tidak perlu mengirim notifikasi ketidakhadiran.

---

# 18. Menyiapkan Folder Website

Buat:

```text
C:\Users\<USERNAME>\.node-red\sman6assets
```

Masukkan:

```text
login.html
operator.html
guru.html
wali-kelas.html
logosman6m.png
```

Struktur:

```text
.node-red/
│
├── sman6assets/
│   ├── login.html
│   ├── operator.html
│   ├── guru.html
│   ├── wali-kelas.html
│   └── logosman6m.png
│
└── settings.js
```

---

# 19. Konfigurasi settings.js

Buka:

```text
C:\Users\<USERNAME>\.node-red\settings.js
```

Pastikan static folder mengarah ke folder aplikasi.

Contoh:

```javascript
httpStatic: [
    {
        path: 'C:/Users/<USERNAME>/.node-red/sman6assets',
        root: '/sman6/'
    }
],
```

Pastikan juga:

```javascript
functionExternalModules: true,
```

Setelah mengubah `settings.js`, restart Node-RED.

---

# 20. Menjalankan Website

Jalankan Node-RED:

```bash
node-red
```

Buka:

```text
http://localhost:1880/sman6/login.html
```

Halaman login aplikasi akan muncul.

---

# 21. Role Operator Sekolah

Operator mengelola:

```text
Dashboard
Data Kelas
Data Siswa
Data Guru
Mata Pelajaran
Wali Kelas
Guru Mapel
```

Operator tidak melakukan presensi harian.

---

# 22. Role Guru Mata Pelajaran

Alurnya:

```text
Login
  ↓
Pilih Kelas
  ↓
Pilih Mata Pelajaran
  ↓
Mulai Presensi
  ↓
Daftar Siswa
  ↓
Hadir / Sakit / Izin / Tanpa Keterangan
  ↓
Simpan Presensi
```

---

# 23. Role Wali Kelas

Wali kelas dapat melihat:

```text
Daftar siswa kelas
Rekap kehadiran
Rekap per mata pelajaran
Filter tanggal
```

---

# 24. Alur Data Presensi

Saat guru menyimpan presensi:

```text
guru.html
   ↓
POST /sman6/api/presensi
   ↓
Node-RED
   ↓
PostgreSQL
   ├── sesi
   └── presensi
```

Jika ada:

```text
sakit
izin
tanpa_keterangan
```

notifikasi Telegram diproses.

---

# 25. Pengujian Setelah Instalasi

## Test 1 - Node-RED

Buka:

```text
http://localhost:1880
```

## Test 2 - Database

Di pgAdmin pastikan:

```text
absen_smansix
```

dapat dibuka dan 9 tabel tersedia.

## Test 3 - Website

Buka:

```text
http://localhost:1880/sman6/login.html
```

## Test 4 - Operator

Login Operator.

Pastikan dashboard dan menu data dapat digunakan.

## Test 5 - Guru

Login Guru.

Pastikan kelas dan mata pelajaran yang ditugaskan tersedia.

## Test 6 - Presensi

Simpan presensi.

Periksa tabel:

```text
sesi
presensi
```

## Test 7 - Telegram

Buat presensi dengan minimal satu:

```text
Sakit
Izin
Tanpa Keterangan
```

Pastikan pesan masuk ke grup Telegram.

## Test 8 - Wali Kelas

Login Wali Kelas.

Pastikan rekap berubah sesuai presensi yang baru disimpan.

---

# 26. Menjalankan dari HP

Jika HP dan komputer server berada pada jaringan yang sama, dari HP jangan menggunakan `localhost`.

Gunakan IP komputer server:

```text
http://IP-KOMPUTER:1880/sman6/login.html
```

Contoh:

```text
http://192.168.x.x:1880/sman6/login.html
```

IP harus disesuaikan dengan komputer server.

---

# 27. Troubleshooting

## Node-RED tidak bisa dijalankan

Cek:

```bash
node --version
npm --version
node-red --version
```

Jika Node-RED belum ada:

```bash
npm install -g node-red
```

## Database tidak tersambung

Periksa:

```text
Host     : 127.0.0.1
Port     : 5432
Database : absen_smansix
User     : postgres
```

Pastikan PostgreSQL Server aktif.

## Node PostgreSQL tidak muncul

Jalankan:

```bash
cd C:\Users\<USERNAME>\.node-red
npm install node-red-contrib-postgresql
```

## Node Telegram tidak muncul

Jalankan:

```bash
cd C:\Users\<USERNAME>\.node-red
npm install node-red-contrib-telegrambot
```

Restart Node-RED.

## Login bermasalah

Pastikan:

```text
PostgreSQL aktif
+
Node-RED aktif
+
flow login aktif
```

## Website tidak muncul

Periksa:

```text
sman6assets
settings.js
Node-RED
```

Setelah mengubah `settings.js`, restart Node-RED.

---

# 28. File Rahasia dan .gitignore

Jangan memasukkan credential ke GitHub.

Contoh `.gitignore`:

```gitignore
flows_cred.json
.env
node_modules/
*.log
Thumbs.db
.DS_Store
```

Jangan upload:

```text
Telegram Bot Token
Password PostgreSQL
Password akun
API key
flows_cred.json
```

`flows.json` boleh disimpan di repository jika tidak berisi credential sensitif.

---

# 29. GitHub

Buka terminal VS Code pada folder repository.

```bash
git init
```

Tambahkan file:

```bash
git add .
```

Commit:

```bash
git commit -m "Initial version aplikasi presensi SMAN 6"
```

Hubungkan repository GitHub:

```bash
git remote add origin https://github.com/USERNAME/REPOSITORY.git
```

Kemudian:

```bash
git branch -M main
git push -u origin main
```

Untuk mengambil perubahan terbaru:

```bash
git pull
```

Untuk mengirim perubahan:

```bash
git add .
git commit -m "Update aplikasi presensi"
git push
```

---

# 30. Workflow Pengembangan

Workflow utama:

```text
VS Code
   ↓
Edit HTML / CSS / JavaScript
   ↓
Test di Node-RED
   ↓
Test PostgreSQL
   ↓
Test aplikasi
   ↓
git add .
   ↓
git commit
   ↓
git push
   ↓
GitHub
```

---

# 31. Backup Database

Sebelum perubahan database besar:

```text
pgAdmin
   ↓
Klik kanan database
   ↓
Backup...
   ↓
Format: Custom
   ↓
absen_smansix.backup
```

Backup ini dapat digunakan untuk restore pada komputer lain.

Jika menggunakan:

```text
absensi_smansix.sql
```

gunakan Query Tool.

Jika menggunakan:

```text
absen_smansix.backup
```

gunakan Restore.

---

# 32. SOP Menjalankan Sistem Setelah Semua Terpasang

Untuk penggunaan sehari-hari:

```text
1. Nyalakan komputer server.

2. Pastikan PostgreSQL Server aktif.

3. Buka Command Prompt.

4. Jalankan:

   node-red

5. Tunggu sampai Node-RED berjalan.

6. Buka browser.

7. Buka:

   http://localhost:1880/sman6/login.html

8. Login sesuai role.

9. Aplikasi siap digunakan.
```

Jangan menutup Command Prompt yang menjalankan Node-RED selama aplikasi digunakan.

---

# 33. Checklist Komputer Baru

```text
[ ] Node.js LTS terinstall
[ ] node --version berhasil
[ ] npm --version berhasil

[ ] Node-RED terinstall
[ ] node-red --version berhasil
[ ] Node-RED dapat dijalankan

[ ] PostgreSQL terinstall
[ ] pgAdmin terinstall
[ ] PostgreSQL Server berjalan

[ ] Database absen_smansix dibuat
[ ] absensi_smansix.sql diimport
[ ] 9 tabel muncul

[ ] VS Code terinstall
[ ] Repository GitHub di-clone/dibuka

[ ] node-red-contrib-postgresql terinstall
[ ] node-red-contrib-telegrambot terinstall
[ ] bcrypt terinstall

[ ] flows.json diimport
[ ] PostgreSQL dikonfigurasi
[ ] Telegram dikonfigurasi

[ ] sman6assets dibuat
[ ] login.html tersedia
[ ] operator.html tersedia
[ ] guru.html tersedia
[ ] wali-kelas.html tersedia
[ ] logosman6m.png tersedia

[ ] settings.js dikonfigurasi
[ ] Node-RED restart

[ ] Login berhasil
[ ] Operator berhasil
[ ] Guru berhasil
[ ] Wali Kelas berhasil
[ ] Presensi berhasil
[ ] Database terisi
[ ] Telegram berhasil
```

---

# 34. Referensi Resmi

Node.js:
https://nodejs.org/en/download

Node-RED Windows:
https://nodered.org/docs/getting-started/windows

PostgreSQL Windows:
https://www.postgresql.org/download/windows/

Visual Studio Code:
https://code.visualstudio.com/

Node-RED PostgreSQL:
https://flows.nodered.org/node/node-red-contrib-postgresql

Node-RED Telegram:
https://flows.nodered.org/node/node-red-contrib-telegrambot
