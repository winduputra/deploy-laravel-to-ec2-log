
# 🚀 Laravel Deployment to AWS EC2 — Technical Log & KP Collaboration

Ini adalah dokumentasi teknis dan catatan pengalaman pribadi saya saat melakukan deploy aplikasi Laravel ke server AWS EC2 berbasis Ubuntu, bersama dua mahasiswa Kuliah Praktik (KP) dari Universitas Lampung (Unila), jurusan Ilmu Komputer: **Rofiq** dan **Naura**.

Kami bekerja kolaboratif dalam menyusun langkah-langkah dari provisioning hingga Laravel dapat diakses via domain publik.

---

## 🎯 Tujuan

- Men-deploy aplikasi Laravel ke server cloud (EC2)
- Menerapkan ilmu Linux server, Nginx, PHP, dan PostgreSQL secara langsung
- Memberikan pengalaman nyata kepada mahasiswa KP tentang DevOps & deployment
- Menjadikan dokumentasi ini sebagai bagian dari portfolio saya di bidang Cloud Engineering

---

## 🛠️ Stack & Tools

- Laravel 10
- PHP 8.3
- PostgreSQL
- Nginx
- Ubuntu 24.04 LTS (on EC2 instance)
- Domain publik (via penyedia eksternal)
- SSL (Let's Encrypt)
- SSH, Git, Composer

---

## 🔧 Langkah-Langkah Deployment

### 1. Provision EC2 Instance
- Launch instance menggunakan Ubuntu Server 22.04 LTS
- Buka port: `22 (SSH)`, `80 (HTTP)`, dan `443 (HTTPS)`
- Akses server menggunakan SSH via keypair `.pem`

### 2. Setup Server Environment
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install nginx php php-fpm php-mbstring php-xml php-bcmath php-curl php-pgsql unzip curl git composer -y
```

### 3. Install dan Konfigurasi PostgreSQL
```bash
sudo apt install postgresql postgresql-contrib -y
sudo -u postgres psql
```

Lalu buat database dan user PostgreSQL. Pastikan `.env` Laravel kamu dikonfigurasi dengan benar (tanpa expose data sensitif).

### 4. Upload Project Laravel
- Project diunggah melalui `scp` atau clone dari GitHub privat
- Jalankan perintah:
```bash
composer install
cp .env.example .env
php artisan key:generate
php artisan migrate
```
- Set permission:
```bash
sudo chown -R www-data:www-data .
sudo chmod -R 775 storage bootstrap/cache
```

### 5. Konfigurasi Nginx
Buat file baru di `/etc/nginx/sites-available/laravel`:
```nginx
server {
    listen 80;
    server_name your-domain.com;

    root /home/ubuntu/laravel_project/public;
    index index.php index.html;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.2-fpm.sock;
    }

    location ~ /\.ht {
        deny all;
    }
}
```

Aktifkan config:
```bash
sudo ln -s /etc/nginx/sites-available/laravel /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```

### 6. (Opsional) Setting Domain dan SSL
- Tambahkan A Record pada DNS domain ke IP EC2
- Pasang SSL dengan Let's Encrypt:
```bash
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d your-domain.com
```

---

## 🤝 Kolaborasi dengan Mahasiswa KP Unila

Dalam proses ini saya dibantu oleh dua mahasiswa KP dari Unila:

- **Rofiq** membantu konfigurasi database dan pengecekan environment Laravel
- **Naura** bertanggung jawab atas dokumentasi teknis dan setup awal server

Mereka aktif belajar dan berdiskusi selama proses berlangsung, serta menunjukkan antusiasme tinggi terhadap dunia cloud computing dan DevOps.

---

## 📌 Pembelajaran yang Didapat

- Deploy Laravel di server Linux bukan hanya soal coding, tapi juga soal environment dan keamanan
- Server-side configuration (Nginx, permission, DB) sangat krusial
- Mahasiswa KP bisa cepat memahami prinsip kerja sistem cloud jika diarahkan dengan simulasi langsung

---

## 🗺️ Rencana Selanjutnya

- Menambahkan CI/CD sederhana dengan GitHub Actions
- Setup auto-deploy ke EC2 via SSH
- Tambah monitoring uptime & resource (Netdata / UptimeRobot)

---

> Dokumentasi ini saya tulis sebagai bagian dari perjalanan saya menuju karier di bidang Cloud Engineering dan DevOps. Saya berharap log ini bisa menjadi referensi belajar bagi siapa saja yang baru pertama kali melakukan deployment Laravel ke cloud.

Silakan gunakan, fork, atau bagikan jika menurut kamu ini bermanfaat 🙌
