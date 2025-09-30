# 🛒 Platform Top-Up Digital - RGShenn Store

Platform top-up digital untuk game vouchers, pulsa, e-money, dan layanan digital lainnya. Dibangun dengan PHP native dan MySQL.

---

## 📋 Daftar Isi
- [Tentang Project](#tentang-project)
- [Framework & Teknologi](#framework--teknologi)
- [Fitur Utama](#fitur-utama)
- [Struktur Database](#struktur-database)
- [Instalasi di Replit](#instalasi-di-replit)
- [Instalasi di cPanel Hosting](#instalasi-di-cpanel-hosting)
- [Konfigurasi](#konfigurasi)
- [Penggunaan](#penggunaan)
- [Troubleshooting](#troubleshooting)

---

## 📖 Tentang Project

Platform top-up digital yang menyediakan layanan pembelian:
- 🎮 **Game Vouchers** (Mobile Legend, Free Fire, PUBG Mobile, COD Mobile, dll)
- 📱 **Pulsa & Paket Data** 
- 💰 **E-Money** (OVO, DANA, GoPay, dll)
- 📺 **Voucher TV** & Entertainment
- 🔄 **Transfer Pulsa**
- 💳 **Pascabayar** (Listrik, PDAM, dll)

---

## 🛠 Framework & Teknologi

### Backend
- **PHP 7.4+** / PHP 8.x
- **MySQL / MariaDB** - Database utama
- **MySQLi** - Database driver
- **Custom MVC Pattern** - Arsitektur aplikasi

### Frontend
- **HTML5, CSS3, JavaScript**
- **Bootstrap** - UI Framework
- **jQuery** - JavaScript library
- **SweetAlert** - Notifikasi UI
- **Progressive Web App (PWA)** - Support mobile app

### Integrasi Pembayaran & API
- **DigiFlazz** - Provider topup utama
- **BosPanel** - Provider sosial media
- **BCA API** - Cek mutasi bank BCA
- **BNI API** - Cek mutasi bank BNI
- **OVO API** - Integrasi e-wallet OVO
- **WhatsApp API** - Notifikasi pelanggan
- **Firebase Cloud Messaging (FCM)** - Push notification
- **OneSignal** - Alternative push notification

### Fitur Tambahan
- **CSRF Protection** - Keamanan form
- **Session Management** - Manajemen sesi pengguna
- **Cookie-based Authentication** - Login otomatis
- **Email Notification** - PHPMailer
- **SIM Card Detection** - Deteksi provider pulsa otomatis
- **URL Rewriting** - SEO-friendly URLs (.htaccess)

---

## ✨ Fitur Utama

### User Panel
- ✅ Registrasi & Login pengguna
- ✅ Dashboard pengguna
- ✅ Pembelian voucher game, pulsa, dll
- ✅ Riwayat transaksi
- ✅ Top-up saldo deposit
- ✅ Manajemen profil
- ✅ Sistem poin reward
- ✅ Notifikasi real-time

### Admin Panel
- ✅ Dashboard admin
- ✅ Manajemen pengguna
- ✅ Manajemen produk/layanan
- ✅ Manajemen pesanan
- ✅ Laporan transaksi & keuangan
- ✅ Manajemen deposit
- ✅ Pengaturan website
- ✅ Manajemen bank & payment gateway
- ✅ News & Promo management

### Premium Features
- ✅ Export laporan ke Excel (PHPExcel)
- ✅ API Integration monitoring
- ✅ Multi-provider support
- ✅ Auto-process orders

---

## 🗄 Struktur Database

Database utama menggunakan **MySQL/MariaDB** dengan tabel-tabel berikut:

### Tabel Utama:
- `users` - Data pengguna
- `users_cookie` - Session & cookie management
- `category` - Kategori produk (games, pulsa, dll)
- `srv` - Service/produk yang dijual
- `orders` - Data pesanan
- `deposit` - Data top-up saldo
- `provider` - Konfigurasi provider API
- `webcfg` - Konfigurasi website
- `webmt` - Maintenance mode
- `account-bank` - Data rekening bank
- `mailer` - Konfigurasi email

### SQL Files:
Terdapat contoh struktur tabel di folder `cron/`:
- `category(MANUAL).sql` - Struktur tabel kategori
- `srv(MANUAL).sql` - Struktur tabel service/produk

---

## 🚀 Instalasi di Replit

### Langkah 1: Clone/Import Project
```bash
# Project sudah ada di Replit
# Pastikan semua file sudah ter-extract dari V8-FIXED-NODB.zip
```

### Langkah 2: Setup PHP
```bash
# PHP sudah otomatis terinstall di Replit
php --version
```

### Langkah 3: Setup Database (Development)
Untuk development di Replit, Anda perlu setup database:

**Opsi A: Gunakan MySQL Online (Recommended untuk testing)**
1. Daftar di layanan MySQL gratis seperti:
   - db4free.net
   - freemysqlhosting.net
   - remotemysql.com

2. Update konfigurasi di `RGShenn.php`:
```php
$aiy = [
    'host' => 'your-mysql-host.com',
    'user' => 'your-username',
    'pass' => 'your-password',
    'name' => 'your-database-name'
];
```

**Opsi B: Setup Database Manual**
1. Import SQL files yang ada di folder `cron/`
2. Buat tabel-tabel yang diperlukan

### Langkah 4: Jalankan Aplikasi
```bash
# PHP Server otomatis berjalan di port 5000
# Akses melalui Webview di Replit
```

### Langkah 5: Konfigurasi
1. Buka browser dan akses aplikasi
2. Login ke admin panel
3. Setup provider API keys di database
4. Konfigurasi payment gateway

---

## 🌐 Instalasi di cPanel Hosting

### Persiapan File Backup
File backup sudah tersedia:
- `database_backup.sql` - Full database backup
- `cpanel_backup.zip` - File aplikasi siap upload

### Langkah 1: Upload File

#### Via File Manager:
1. Login ke **cPanel**
2. Buka **File Manager**
3. Masuk ke folder `public_html` atau folder domain Anda
4. Upload file `cpanel_backup.zip`
5. Extract file zip tersebut
6. Hapus file zip setelah extract

#### Via FTP:
1. Gunakan FileZilla atau FTP client lainnya
2. Connect ke server hosting
3. Upload semua file ke folder `public_html`

### Langkah 2: Setup Database MySQL

1. **Buat Database Baru:**
   - Login ke cPanel
   - Buka **MySQL Databases**
   - Buat database baru (contoh: `username_rgshenn`)
   - Buat user database baru
   - Assign user ke database dengan **ALL PRIVILEGES**

2. **Import Database:**
   - Buka **phpMyAdmin** di cPanel
   - Pilih database yang baru dibuat
   - Klik tab **Import**
   - Upload file `database_backup.sql`
   - Klik **Go** untuk import

3. **Atau via MySQL Import:**
   ```sql
   mysql -u username_rgshenn -p username_rgshenn < database_backup.sql
   ```

### Langkah 3: Konfigurasi Database Connection

Edit file `RGShenn.php` (line 10-15):

```php
$aiy = [
    'host' => 'localhost',              // Biasanya 'localhost' di cPanel
    'user' => 'username_rgshenn',       // User database Anda
    'pass' => 'password_database',      // Password database Anda
    'name' => 'username_rgshenn'        // Nama database Anda
];
```

### Langkah 4: Setup PHP Version

1. Di cPanel, buka **Select PHP Version**
2. Pilih **PHP 7.4** atau **PHP 8.0+**
3. Aktifkan ekstensi yang diperlukan:
   - ✅ mysqli
   - ✅ curl
   - ✅ json
   - ✅ mbstring
   - ✅ openssl
   - ✅ zip

### Langkah 5: Setup Permissions

Set permission folder/file yang tepat:
```bash
# Via Terminal SSH atau File Manager
chmod 755 -R /home/username/public_html
chmod 644 /home/username/public_html/.htaccess
```

### Langkah 6: Setup SSL (HTTPS)

1. Di cPanel, buka **SSL/TLS**
2. Install **Let's Encrypt SSL** (gratis)
3. Atau upload SSL certificate Anda sendiri

**Note:** File `.htaccess` sudah dikonfigurasi untuk force HTTPS

### Langkah 7: Testing

1. Akses domain Anda: `https://yourdomain.com`
2. Cek apakah aplikasi load dengan benar
3. Test login ke admin panel
4. Verifikasi koneksi database

---

## ⚙️ Konfigurasi

### Konfigurasi Utama (RGShenn.php)

```php
// Timezone
date_default_timezone_set('Asia/Jakarta');

// Database
$aiy = [
    'host' => 'localhost',
    'user' => 'your_db_user',
    'pass' => 'your_db_pass',
    'name' => 'your_db_name'
];

// Website Config (di database tabel webcfg)
$_CONFIG = [
    'title'         => 'Nama Website',
    'description'   => 'Deskripsi Website',
    'keyword'       => 'keyword, seo',
    'banner'        => 'url_banner',
    'icon'          => 'url_icon',
];
```

### Konfigurasi Provider API

Update di database tabel `provider`:

**DigiFlazz:**
```sql
UPDATE provider SET uid='username', ukey='api_key' WHERE code='DIGI';
```

**BosPanel (Sosmed):**
```sql
UPDATE provider SET ukey='api_token' WHERE code='BP';
```

### Konfigurasi Payment Gateway

**BCA API:**
```sql
UPDATE `account-bank` SET column1='client_id', column2='client_secret' WHERE id=1;
```

### Konfigurasi Email (PHPMailer)

```sql
UPDATE mailer SET 
    column1='smtp.gmail.com',     -- SMTP Host
    column2='your@email.com',      -- Email
    column3='your_password',       -- Password
    column4='Your Name'            -- From Name
WHERE id=1;
```

---

## 📱 Penggunaan

### Admin Panel
```
URL: https://yourdomain.com/admin
Default Login: (setup saat pertama install)
```

### User Panel
```
URL: https://yourdomain.com
Register: https://yourdomain.com/auth/register
Login: https://yourdomain.com/auth/login
```

### Desktop Version
```
URL: https://yourdomain.com/web
```

### API Endpoints
Aplikasi ini juga menyediakan API untuk:
- Order processing
- Balance checking
- Status inquiry
- Callback handling

---

## 🔧 Troubleshooting

### Error: Connection Failed
**Solusi:**
- Cek kredensial database di `RGShenn.php`
- Pastikan MySQL service berjalan
- Verifikasi user database punya privilege yang cukup

### Error: 500 Internal Server Error
**Solusi:**
- Cek PHP error log di cPanel
- Pastikan PHP version minimum 7.4
- Cek file permission (755 untuk folder, 644 untuk file)

### Error: .htaccess not working
**Solusi:**
- Pastikan mod_rewrite enabled di Apache
- Cek file `.htaccess` ada di root folder
- Hubungi hosting support untuk enable mod_rewrite

### HTTPS Force Redirect Issue
**Solusi:**
- Edit `.htaccess`, comment line FORCE HTTPS jika tidak punya SSL:
```apache
# RewriteCond %{HTTPS} !=on
# RewriteRule ^.*$ https://%{SERVER_NAME}%{REQUEST_URI} [R,L]
```

### Session/Cookie Issues
**Solusi:**
- Clear browser cache & cookies
- Cek `session.auto_start` di php.ini atau .htaccess
- Pastikan write permission di session folder

---

## 📁 Struktur Folder

```
.
├── admin/              # Admin panel
├── account/            # User account pages
├── auth/               # Authentication (login/register)
├── cron/               # Cron jobs & SQL files
├── deposit/            # Deposit/topup saldo
├── event/              # Event & promo
├── library/            # Core libraries & functions
│   ├── function/       # Custom functions & API classes
│   ├── header/         # Header templates
│   ├── footer/         # Footer templates
│   └── session/        # Session management
├── order/              # Order processing pages
├── page/               # Static pages (info, news, dll)
├── payment/            # Payment gateway
├── platform/           # Platform (desktop/mobile view)
├── premium/            # Premium features
├── shop/               # Shop/marketplace
├── transfer/           # Transfer saldo
├── .htaccess           # Apache configuration
├── index.php           # Main entry point
├── RGShenn.php         # Core configuration
└── connect.php         # Database connection
```

---

## 🔐 Keamanan

- ✅ CSRF Token protection
- ✅ SQL Injection prevention (mysqli_real_escape_string)
- ✅ XSS protection
- ✅ Session hijacking prevention
- ✅ Password hashing
- ✅ HTTPS enforcement
- ✅ Cookie security

---

## 📝 Catatan Penting

1. **Database Backup:** Selalu backup database secara berkala
2. **API Keys:** Simpan API keys dengan aman, jangan di-commit ke git
3. **PHP Version:** Minimum PHP 7.4, recommended PHP 8.0+
4. **MySQL Version:** MySQL 5.7+ atau MariaDB 10.3+
5. **Server Requirements:** 
   - Minimum 1GB RAM
   - PHP memory_limit: 512M (sudah diset di RGShenn.php)
   - max_execution_time: 300

---

## 📞 Support

Untuk bantuan lebih lanjut:
- Dokumentasi PHP: https://www.php.net/manual/
- cPanel Documentation: https://docs.cpanel.net/
- MySQL Documentation: https://dev.mysql.com/doc/

---

## 📄 Lisensi

Project ini untuk penggunaan internal. Hubungi developer untuk informasi lisensi.

---

## 🔄 Update & Maintenance

### Cara Update Aplikasi:
1. Backup database dan file lama
2. Upload file baru
3. Jalankan SQL migration jika ada
4. Clear cache browser

### Maintenance Mode:
Set maintenance via database:
```sql
UPDATE webmt SET status='maintenance' WHERE id=1;
```

---

**Dibuat dengan ❤️ menggunakan PHP & MySQL**

*Last Updated: September 2025*
