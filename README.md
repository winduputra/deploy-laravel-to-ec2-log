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
- PHP 8.2
- PostgreSQL
- Nginx
- Ubuntu 22.04 LTS (on EC2 instance)
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
