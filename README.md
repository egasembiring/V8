
# 🎮 Platform Pembayaran Digital untuk Gaming & Layanan Media Sosial

Platform pembayaran berbasis PHP yang komprehensif dirancang khusus untuk top-up gaming, layanan media sosial, dan transaksi digital. Dilengkapi dengan multiple payment gateway, integrasi provider, notifikasi otomatis, dan sistem keamanan yang ekstensif.

![Platform Preview](https://img.shields.io/badge/PHP-8.x-blue.svg)
![License](https://img.shields.io/badge/License-Commercial-red.svg)
![Status](https://img.shields.io/badge/Status-Development-orange.svg)

## 🌟 Fitur Utama

### 💳 **Integrasi Pembayaran**
- **Tripay**: Payment gateway utama dengan dukungan komprehensif
- **Mutasi.co.id**: Auto-mutasi untuk transfer bank dan e-wallet
- **iPaymu**: Payment gateway alternatif dengan nomor VA
- **OVO & GoPay**: Integrasi langsung e-wallet dengan auto-mutasi
- **Pembayaran Manual**: Verifikasi pembayaran yang dikontrol admin

### 🎯 **Dukungan Provider Gaming**
- **DigiFlazz (DF)**: Top-up pulsa dan voucher digital
- **MediaPulsa (MP)**: Layanan pulsa dan paket data  
- **AtlasGaming (AG)**: Voucher gaming dan mata uang dalam game
- **VouchGamers (VR)**: Layanan top-up gaming
- **Lapak Gaming (LG)**: Layanan gaming premium *(BARU)*
- **SMM Panel (SMM)**: Layanan pemasaran media sosial *(BARU)*

### 🔔 **Sistem Notifikasi**
- **Bot WhatsApp**: Notifikasi pesanan otomatis dan update status
- **Email SMTP**: Notifikasi email profesional dan struk pembayaran
- **Notifikasi Real-time**: Update status pesanan secara instan

### 🌍 **Dukungan Multi-Bahasa**
- **Bahasa Indonesia**: Bahasa utama
- **English**: Dukungan terjemahan lengkap *(BARU DIAKTIFKAN)*
- **Pergantian Bahasa Dinamis**: Penyimpanan bahasa berbasis sesi
- **Implementasi AJAX Aman**: Pergantian bahasa dengan perlindungan CSRF

### 🔒 **Fitur Keamanan**
- **Perlindungan CSRF**: Validasi token CSRF untuk form utama (pengaturan provider, pembayaran, pergantian bahasa)
- **Pencegahan SQL Injection**: Modul inti menggunakan prepared statement melalui fungsi safe_query()
- **Perlindungan XSS**: Sanitasi input dan encoding output
- **Keamanan Sesi**: Manajemen sesi yang aman
- **Validasi Input**: Validasi data yang komprehensif

## 📋 Persyaratan

### Persyaratan Server
- **PHP**: 8.0+ (Diuji pada PHP 8.4.10)
- **MySQL**: 5.7+ atau MariaDB 10.2+
- **Web Server**: Apache/Nginx dengan dukungan PHP
- **Sertifikat SSL**: Diperlukan untuk produksi (keamanan pembayaran)

### Ekstensi PHP
```
- mysqli (Konektivitas MySQL)
- curl (Komunikasi API)
- json (Pemrosesan data)
- session (Sesi pengguna)
- mbstring (Penanganan string)
- openssl (Komunikasi aman)
```

### Dependensi Pihak Ketiga
- **PHPMailer 5.2+**: Fungsi email SMTP
- **jQuery 3.7.1**: Interaksi frontend
- **Bootstrap 4**: Framework UI
- **Feather Icons**: Library ikon

## ⚙️ Instalasi

### 1. Clone Repository
```bash
git clone <repository-url>
cd payment-platform
```

### 2. Setup Database
**PENTING: Gunakan file database yang benar!**

```sql
-- Import skema database utama (gunakan file ini untuk setup awal)
mysql -u username -p database_name < Databasenya.sql

-- JANGAN gunakan database_voucher_constraint.sql untuk import pertama
-- File ini hanya untuk update constraint voucher setelah database utama sudah ada

-- Buat user database (opsional)
CREATE USER 'payment_user'@'localhost' IDENTIFIED BY 'password_aman';
GRANT ALL PRIVILEGES ON payment_db.* TO 'payment_user'@'localhost';
FLUSH PRIVILEGES;
```

**Cara Import Database yang Benar:**

1. **Gunakan phpMyAdmin atau command line:**
   - Import file `Databasenya.sql` terlebih dahulu
   - Setelah berhasil, baru jalankan `database_voucher_constraint.sql` jika diperlukan

2. **Melalui Command Line:**
   ```bash
   # Import database utama
   mysql -u root -p dewamyid_vip < Databasenya.sql
   
   # Setelah berhasil, import constraint voucher (opsional)
   mysql -u root -p dewamyid_vip < database_voucher_constraint.sql
   ```

3. **Melalui phpMyAdmin:**
   - Login ke phpMyAdmin
   - Pilih database `dewamyid_vip`
   - Klik tab "Import" 
   - Pilih file `Databasenya.sql` (bukan database_voucher_constraint.sql)
   - Klik "Go"

### 3. Konfigurasi
```php
// Edit pengaturan database di mainconfig.php
// PENTING: Jangan commit kredensial asli ke version control
$_CONFIG['db']['host'] = 'localhost';
$_CONFIG['db']['username'] = 'user_db_anda';
$_CONFIG['db']['password'] = 'password_aman_anda';
$_CONFIG['db']['name'] = 'nama_db_anda';
```

### 4. Set Permissions
```bash
# Set kepemilikan yang tepat (sesuaikan user/group sesuai kebutuhan)
sudo chown -R www-data:www-data /path/to/platform

# Set permission yang aman
chmod -R 755 /path/to/platform
chmod 644 /path/to/platform/cron/*.php
chmod 644 /path/to/platform/cron/tripay/*.php
chmod 600 /path/to/platform/mainconfig.php

# User Apache: Lihat bagian konfigurasi web server untuk setup cron/.htaccess

# Verifikasi setup Apache
# apachectl -M | grep rewrite  # Harus menampilkan rewrite_module
# apachectl -M | grep headers  # Harus menampilkan headers_module

# Verifikasi konfigurasi keamanan (setelah setup web server)
# Script CLI harus diblokir (403):
# curl -I https://domain-anda/cron/df-status.php

# Webhook Tripay harus dapat diakses (200/405):
# curl -I https://domain-anda/cron/tripay/callback.php

# PENTING: Jika rules tidak berfungsi, cek AllowOverride FileInfo di vhost anda
```

### 5. Konfigurasi Web Server

#### Apache 
**Prasyarat:**
```bash
# Aktifkan modul yang diperlukan
sudo a2enmod rewrite headers
sudo systemctl reload apache2
```

**Konfigurasi Virtual Host (diperlukan):**
```apache
<VirtualHost *:80>
    DocumentRoot /path/to/platform
    # KRITIS: Izinkan .htaccess untuk bekerja
    <Directory "/path/to/platform">
        AllowOverride FileInfo
        Require all granted
    </Directory>
</VirtualHost>
```

**File .htaccess utama:**
```apache
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ index.php [QSA,L]

# Header Keamanan
Header always set X-Content-Type-Options nosniff
Header always set X-Frame-Options DENY
Header always set X-XSS-Protection "1; mode=block"
```

**KRITIS: Buat /path/to/platform/cron/.htaccess:**
```apache
RewriteEngine On
# Izinkan akses webhook Tripay (callback pembayaran)
RewriteRule ^tripay/callback\.php$ - [L]
# Blokir semua script cron lainnya (hanya CLI)
RewriteRule ^ - [F]
```

#### Nginx
```nginx
# PENTING: Izinkan webhook Tripay sebelum memblokir /cron
location = /cron/tripay/callback.php {
    fastcgi_pass unix:/var/run/php/php8.0-fpm.sock;
    fastcgi_index index.php;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    include fastcgi_params;
}

# KRITIS: Blokir akses ke script cron lainnya (hanya CLI)
location ^~ /cron {
    deny all;
    return 403;
}

location / {
    try_files $uri $uri/ /index.php?$query_string;
}

location ~ \.php$ {
    fastcgi_pass unix:/var/run/php/php8.0-fpm.sock;
    fastcgi_index index.php;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    include fastcgi_params;
}
```

## 🛠️ Panduan Konfigurasi

### Setup Payment Gateway

#### 1. Konfigurasi Tripay
```php
// Admin → Pengaturan → Pembayaran → Tripay
Merchant ID: id_merchant_anda
API ID: id_api_anda  
API Key: kunci_private_anda
```

#### 2. Setup Mutasi.co.id
```php
// Admin → Pengaturan → Pembayaran → Mutasi
API Key: api_key_mutasi_anda
```

#### 3. Konfigurasi iPaymu
```php
// Admin → Pengaturan → Pembayaran → iPaymu  
API Key: api_key_ipaymu_anda
VA Number: virtual_account_anda
```

### Konfigurasi Provider

#### Provider Gaming
Buka **Admin → Pengaturan → Provider** dan konfigurasi:

```php
// DigiFlazz
API ID: username_digiflazz_anda
API Key: api_key_digiflazz_anda

// MediaPulsa
API ID: id_mediapulsa_anda
API Key: key_mediapulsa_anda

// AtlasGaming
Merchant ID: merchant_atlas_anda
API ID: id_atlas_anda
API Key: key_atlas_anda
```

### Setup Notifikasi

#### Bot WhatsApp
```php
// Admin → Pengaturan → WhatsApp
Nomor HP: 628xxxxxxxxxx
API Key: api_key_whatsapp_anda
URL API: https://wa-gateway-anda.com
```

#### Email SMTP
```php
// Admin → Pengaturan → Email
Host SMTP: smtp.gmail.com
Port SMTP: 587
Username: email_anda@gmail.com
Password: password_app_anda
Keamanan: TLS
```

## 🚀 Panduan Penggunaan

### Untuk Pengguna

#### 1. Buat Pesanan
1. Pilih layanan/produk gaming
2. Masukkan ID game/username
3. Pilih metode pembayaran
4. Selesaikan pembayaran
5. Terima notifikasi

#### 2. Cek Status Pesanan
- Gunakan fitur **Cek Invoice**
- Masukkan ID pesanan untuk melacak status
- Update status real-time

### Untuk Administrator

#### 1. Kelola Pesanan
```php
// Akses panel admin
/admin

// Lihat pesanan
Admin → Pesanan → Lihat Semua

// Proses pesanan manual
Admin → Pesanan → Pemrosesan Manual
```

#### 2. Manajemen Provider
```php
// Sinkronisasi layanan dari provider
Cron → Sinkron Layanan

// Update harga
Admin → Layanan → Manajemen Harga

// Monitor status provider
Admin → Provider → Cek Status
```

#### 3. Monitoring Pembayaran
```php
// Cek mutasi
Admin → Pembayaran → Mutasi

// Verifikasi manual
Admin → Pembayaran → Verifikasi Manual

// Laporan transaksi
Admin → Laporan → Transaksi
```

## 🔄 Sistem Otomatis (Cron Jobs)

### Setup Cron Jobs
```bash
# Tambahkan ke crontab (crontab -e)

# Sinkronisasi layanan (setiap 30 menit)
*/30 * * * * php /path/to/platform/cron/df-service.php
*/30 * * * * php /path/to/platform/cron/vr-service.php

# Mutasi pembayaran (setiap 5 menit)  
*/5 * * * * php /path/to/platform/cron/mutasi.php
*/5 * * * * php /path/to/platform/cron/ipaymu.php
*/5 * * * * php /path/to/platform/cron/mutasi_gopay.php
*/5 * * * * php /path/to/platform/cron/mutasi_ovo.php

# Update status pesanan (setiap 2 menit)
*/2 * * * * php /path/to/platform/cron/df-status.php
*/2 * * * * php /path/to/platform/cron/ag-status.php
*/2 * * * * php /path/to/platform/cron/vr-status.php
```

### Fitur Otomatisasi
- **Sinkron Layanan Otomatis**: Update layanan yang tersedia dari provider
- **Deteksi Pembayaran Otomatis**: Monitor mutasi payment gateway
- **Pemrosesan Pesanan Otomatis**: Proses pembayaran terkonfirmasi secara otomatis
- **Update Status Otomatis**: Update status pesanan dari respon provider
- **Notifikasi Otomatis**: Kirim notifikasi WhatsApp/Email untuk perubahan status

## 🌐 Dukungan Multi-Bahasa

### Pergantian Bahasa
Pengguna dapat beralih antara Bahasa Indonesia dan English menggunakan dropdown bahasa di header.

### File Terjemahan
```php
// Terjemahan Bahasa Indonesia
system/languages/indonesia.php

// Terjemahan English  
system/languages/english.php
```

### Menambah Bahasa Baru
```php
// 1. Buat file bahasa baru
system/languages/bahasa_anda.php

// 2. Tambahkan ke bahasa yang diizinkan
ajax/switch-language.php → $allowed_languages[]

// 3. Update helper bahasa
system/helpers/default_helper.php → load_language()
```

## 🔒 Praktik Keamanan Terbaik

### Langkah Keamanan yang Telah Diimplementasi
- ✅ **Perlindungan CSRF**: Form provider, pembayaran, dan pergantian bahasa menyertakan token CSRF
- ✅ **Pencegahan SQL Injection**: Operasi database inti menggunakan prepared statement melalui safe_query()
- ✅ **Perlindungan XSS**: Sanitasi input dan encoding output
- ✅ **Keamanan Sesi**: Konfigurasi sesi yang aman
- ✅ **Validasi Input**: Validasi sisi server untuk semua input
- ✅ **Penanganan Error**: Pesan error aman tanpa data sensitif

### Rekomendasi Keamanan Tambahan
```php
// 1. Aktifkan HTTPS di produksi
$_CONFIG['web']['force_https'] = true;

// 2. Set cookie sesi yang aman
ini_set('session.cookie_secure', 1);
ini_set('session.cookie_httponly', 1);

// 3. Implementasi rate limiting
// Pertimbangkan menggunakan fail2ban atau tools serupa

// 4. Update keamanan berkala
// Jaga agar PHP dan dependensi tetap terbaru
```

## 📊 Skema Database

### Tabel Utama
```sql
-- Tabel pengguna
users (id, username, email, password, saldo, level, status)

-- Tabel pesanan  
pembelian_guest (id, order_id, username, layanan, data, harga, status, provider)

-- Tabel layanan
layanan_pulsa (id, provider, layanan, harga_member, harga_reseller, status)

-- Metode pembayaran
metode_guest (id, nama, tipe, minimal, rate, status)

-- Tracking mutasi
mutasi (id, reference, amount, description, created_at, status, code)
```

### Tabel Provider
```sql
-- Konfigurasi provider
provider (id, provider, api_id, api_key, status)

-- Konfigurasi payment gateway
payment (id, nama, code, api_key, merchant_id, status)
```

## 🚨 Troubleshooting

### Masalah Umum

#### 1. Error Koneksi Database
```bash
# Periksa kredensial database
# Verifikasi layanan MySQL berjalan
systemctl status mysql

# Test koneksi
mysql -u username -p database_name
```

#### 2. Error Token CSRF
```php
// Periksa konfigurasi sesi
// Verifikasi generasi token CSRF
// Hapus cache browser dan cookies
```

#### 3. Error Payment Gateway
```php
// Verifikasi kredensial API
// Periksa konektivitas jaringan
// Review dokumentasi gateway
// Monitor log API
```

#### 4. WhatsApp/Email Tidak Berfungsi
```php
// Periksa kredensial API
// Verifikasi pengaturan SMTP
// Test konektivitas
// Review error logs
```

### Log Error
```bash
# Periksa log error PHP
tail -f /var/log/php/error.log

# Periksa log web server
tail -f /var/log/apache2/error.log
tail -f /var/log/nginx/error.log
```

## 📈 Optimasi Performa

### Optimasi Database
```sql
-- Tambahkan index untuk kolom yang sering di-query
CREATE INDEX idx_order_id ON pembelian_guest(order_id);
CREATE INDEX idx_username ON pembelian_guest(username);
CREATE INDEX idx_status ON pembelian_guest(status);
CREATE INDEX idx_provider ON pembelian_guest(provider);
```

### Strategi Caching
```php
// Implementasi Redis/Memcached untuk:
// - Penyimpanan sesi
// - Caching data layanan
// - Rate limiting
```

### Optimasi File
```bash
# Aktifkan kompresi gzip
# Optimasi gambar
# Minify file CSS/JS
# Gunakan CDN untuk static assets
```

## 🔧 Development

### Struktur File
```
/
├── admin/           # Panel admin
├── ajax/           # Handler AJAX
├── assets/         # CSS, JS, gambar
├── cron/           # Tugas otomatis
│   ├── tripay/     # Callback Tripay
│   ├── games/      # Tugas provider game
│   └── custom/     # Otomatisasi kustom
├── id/             # Antarmuka pengguna
│   └── ajax/       # Handler AJAX pengguna
├── layouts/        # Template
├── lib/            # Library eksternal
├── page/           # Halaman statis
├── system/         # Sistem inti
│   ├── helpers/    # Fungsi helper
│   ├── languages/  # File terjemahan
│   └── core.php    # File sistem utama
├── mainconfig.php  # Konfigurasi utama
├── Databasenya.sql # Skema database
└── index.php       # Entry point
```

### Menambah Fitur Baru
```php
// 1. Buat file fitur
// 2. Tambahkan tabel database jika diperlukan
// 3. Implementasi langkah keamanan
// 4. Tambahkan terjemahan
// 5. Test menyeluruh
// 6. Dokumentasikan perubahan
```

## 📝 Dokumentasi API

### HTTP Webhook Endpoints
```php
// Callback pembayaran dari Tripay (HTTP webhook)
POST /cron/tripay/callback.php
```

### Script Otomatisasi CLI
```php
// Ini adalah script cron khusus CLI, BUKAN web endpoints
// Pemrosesan mutasi
CLI: php /path/to/platform/cron/mutasi.php
CLI: php /path/to/platform/cron/ipaymu.php
CLI: php /path/to/platform/cron/mutasi_gopay.php
CLI: php /path/to/platform/cron/mutasi_ovo.php

// Update status provider  
CLI: php /path/to/platform/cron/df-status.php
CLI: php /path/to/platform/cron/ag-status.php
CLI: php /path/to/platform/cron/vr-status.php

// Sinkronisasi layanan
CLI: php /path/to/platform/cron/df-service.php
CLI: php /path/to/platform/cron/vr-service.php
```

### API Internal
```php
// Dapatkan harga layanan (antarmuka pengguna)
POST /id/ajax/get-harga.php

// Dapatkan harga layanan SMM (admin)
POST /admin/guest/ajax/get-harga.php

// Ganti bahasa
POST /ajax/switch-language.php

// Validasi nickname (terintegrasi dalam form produk)
// Ditangani dalam halaman produk individual
```

## 🤝 Kontribusi

### Panduan Development
1. Ikuti standar coding PSR
2. Tambahkan perlindungan CSRF ke semua form
3. Gunakan prepared statement untuk query database
4. Implementasi error handling yang tepat
5. Tambahkan terjemahan untuk teks baru
6. Test menyeluruh sebelum deployment

### Melaporkan Issues
1. Periksa issues yang sudah ada terlebih dahulu
2. Berikan informasi error yang detail
3. Sertakan langkah untuk mereproduksi
4. Sebutkan detail lingkungan

## 📄 Lisensi

Ini adalah platform perangkat lunak komersial. Semua hak dilindungi undang-undang.

## 🆘 Dukungan

Untuk dukungan teknis dan pertanyaan:
- **Dokumentasi**: README.md ini
- **Issues**: GitHub Issues (jika tersedia)
- **Email**: Hubungi administrator sistem

---

## 📋 Update Terbaru

### Versi 2.1.0 (September 2025)
- ✅ Keamanan yang ditingkatkan dengan perlindungan CSRF untuk form utama
- ✅ Perbaikan kerentanan SQL injection di seluruh modul
- ✅ Menambahkan dua provider gaming baru (Lapak Gaming, SMM Panel)
- ✅ Implementasi dukungan lengkap Bahasa English
- ✅ Bot WhatsApp yang ditingkatkan dengan error handling yang lebih baik
- ✅ Sistem email SMTP yang diperbaiki dengan keamanan TLS
- ✅ Perbaikan sistem auto-mutasi OVO/GoPay
- ✅ Peningkatan semua integrasi payment gateway
- ✅ Perbaikan otomatisasi cron job
- ✅ Penambahan error logging yang komprehensif

### Peningkatan Keamanan
- Form utama menyertakan validasi token CSRF (pengaturan provider, pembayaran, pergantian bahasa)
- Query database dikonversi ke prepared statement
- Implementasi sanitasi input dan encoding output
- Manajemen sesi yang aman
- Peningkatan error handling tanpa disclosure informasi

---

*Platform ini dalam pengembangan aktif dengan fitur keamanan tingkat enterprise. Pastikan untuk melakukan testing yang tepat di lingkungan staging sebelum deployment produksi.*
