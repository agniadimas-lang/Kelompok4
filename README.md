# sistem-operasi-si25-kelompok4

# Tugas Instalasi Debian 13 Headless - Kelompok 4

# Tampilan Status Nginx Kelompok 4

# Laporan Tugas Kelompok
## Instalasi Debian 13 Headless Web Server

**Mata Kuliah:** Sistem Operasi (SI-25)  
**Program Studi:** Sistem Informasi  
**Universitas Galuh**

---

# 👥 Anggota Kelompok 4 (SI-2025B)

1. Agnia Dimas Suniar /  7020250043
2. Bagja Bintang Mulyana / 7020250036
3. Sany Kusumadipraja / 7020250024
4. Syahrul Ramadhan / 7020250042

---

# 🎯 Spesifikasi Lingkungan Server

| Komponen | Keterangan |
|----------|------------|
| Hypervisor | VMware Workstation Pro |
| Sistem Operasi | Debian 13 Headless (CLI) |
| Hostname | server-SI2025B |
| IP Address VM | 192.168.146.132 |
| Web Server | Nginx |
| Port Forwarding | Host 8080 → Guest 80 |

IP Address diperoleh menggunakan perintah:

```bash
ip a
```

Kemudian melihat interface **ens33**.

---

# 🛠️ Langkah-Langkah Praktikum

# 1. Instalasi Debian 13 Headless

Pada tahap pertama dilakukan instalasi sistem operasi Debian 13 menggunakan mode teks (CLI) tanpa desktop environment sehingga server menjadi lebih ringan dan efisien.

## Langkah 1. Boot Installer Debian

- Jalankan virtual machine menggunakan file ISO Debian 13.
- Pada menu awal installer pilih **Install** (bukan Graphical Install).

**Penjelasan**

Mode Install digunakan agar proses instalasi berlangsung menggunakan tampilan terminal (headless).

---

## Langkah 2. Memilih Bahasa

Pilih bahasa installer.

Contoh:

- English

Klik **Continue**.

**Penjelasan**

Bahasa installer hanya digunakan selama proses instalasi.

---

## Langkah 3. Memilih Negara

Pilih lokasi sesuai wilayah.

Contoh:

- Indonesia

Klik **Continue**.

**Penjelasan**

Lokasi digunakan untuk menentukan timezone dan mirror repository Debian.

---

## Langkah 4. Layout Keyboard

Pilih keyboard:

- American English

Klik Continue.

---

## Langkah 5. Konfigurasi Jaringan

Installer akan mendeteksi kartu jaringan secara otomatis.

Apabila menggunakan DHCP maka alamat IP akan diperoleh otomatis.

Jika diminta Domain Name dapat dikosongkan.

---

## Langkah 6. Konfigurasi Hostname

Masukkan hostname:

```text
server-SI2025B
```

Klik Continue.

**Penjelasan**

Hostname merupakan nama komputer yang akan digunakan di dalam jaringan.

---

## Langkah 7. Membuat User

Masukkan:

- Password root
- Nama user
- Username
- Password user

**Penjelasan**

Root digunakan sebagai administrator sedangkan user biasa digunakan untuk aktivitas harian.

---

## Langkah 8. Partisi Harddisk

Pilih menu:

```
Guided - use entire disk
```

Pilih harddisk:

```
/dev/sda
```

Kemudian pilih:

```
All files in one partition (recommended)
```

Lalu pilih:

```
Finish partitioning and write changes to disk
```

Pilih **Yes**.

**Penjelasan**

Metode Guided membuat partisi secara otomatis menggunakan seluruh kapasitas harddisk.

---

## Langkah 9. Instalasi Sistem

Installer mulai menyalin seluruh file sistem Debian ke harddisk.

Tunggu hingga proses selesai.

---

## Langkah 10. Software Selection

Pada menu Software Selection hanya centang:

- SSH Server
- Standard System Utilities

Hilangkan centang lainnya.

**Penjelasan**

SSH Server digunakan untuk mengakses server dari komputer lain.

Standard System Utilities merupakan utilitas dasar yang dibutuhkan sistem.

Tidak menginstal Desktop Environment agar server tetap ringan.

### Screenshot Software Selection

![Software Selection](images/01-software-selection-png.jpg)

---

## Langkah 11. Instalasi GRUB

Saat muncul pertanyaan:

Install the GRUB boot loader?

Pilih:

```
Yes
```

Kemudian pilih lokasi:

```
/dev/sda
```

**Penjelasan**

GRUB merupakan bootloader yang digunakan untuk menjalankan sistem operasi ketika komputer dinyalakan.

---

## Langkah 12. Login Pertama

Setelah restart, login menggunakan user yang telah dibuat.

### Screenshot Login Debian

![Login Terminal](images/02-debian-login.png.jpg)

---

# 2. Konfigurasi User Sudo dan Update Repository

Setelah instalasi selesai dilakukan konfigurasi hak akses administrator pada user biasa.

Masuk sebagai root kemudian jalankan:

```bash
apt update && apt upgrade -y
```

**Penjelasan**

Perintah ini memperbarui daftar repository dan menginstal seluruh paket terbaru sehingga sistem menjadi lebih aman dan stabil.

Selanjutnya install sudo:

```bash
apt install sudo -y
```

Tambahkan user ke grup sudo.

```bash
usermod -aG sudo kelompok4
```

Restart sistem.

```bash
reboot
```

Setelah login kembali, uji menggunakan:

```bash
sudo apt update
```

Apabila meminta password dan berhasil dijalankan berarti konfigurasi sudo berhasil.

### Screenshot

![Update](images/03-install-sudo.png.jpg)

![Install Sudo](images/04-sudo-config.png.jpeg)

![Usermod](images/05-usermod.png.jpeg)

---

# 3. Instalasi Web Server Nginx

Install seluruh paket yang dibutuhkan.

```bash
sudo apt install net-tools curl git nginx -y
```

Keterangan:

- net-tools digunakan untuk melihat konfigurasi jaringan.
- curl digunakan untuk melakukan request HTTP.
- git digunakan untuk clone repository.
- nginx digunakan sebagai web server.

Jalankan service.

```bash
sudo systemctl start nginx
```

Agar otomatis aktif saat boot.

```bash
sudo systemctl enable nginx
```

Periksa status service.

```bash
sudo systemctl status nginx
```

Status harus menunjukkan:

```
active (running)
```

### Screenshot

![Status Nginx](images/06-nginx-status.png.jpg)

---

# 4. Membuat Halaman Profil Kelompok

Masuk ke direktori web.

```bash
cd /var/www/html
```

Edit file bawaan.

```bash
sudo nano index.html
```

Masukkan halaman HTML berisi identitas kelompok.

Simpan perubahan.

Restart nginx.

```bash
sudo systemctl restart nginx
```

**Penjelasan**

Restart dilakukan agar konfigurasi dan halaman web terbaru dimuat kembali oleh Nginx.

### Screenshot

![Edit HTML](images/07-edit-html.png.jpg)

---

# 5. Konfigurasi Port Forwarding VMware

Buka:

```
Edit
→ Virtual Network Editor
→ NAT Settings
```

Tambahkan aturan:

| Host Port | Guest Port |
|------------|------------|
| 8080 | 80 |

Simpan konfigurasi.

Buka browser Windows Host.

Akses:

```
http://localhost:8080
```

Apabila halaman profil kelompok muncul berarti konfigurasi berhasil.

### Screenshot NAT

![NAT](images/08-nat-settings.png.jpg)

### Screenshot Browser

![Browser](images/09-browser-host.png.jpg)

---

# 🎥 Link Video Demo

Tambahkan tautan video YouTube atau Google Drive di bawah ini.

```
https://youtube.com/...
```

---

# Link Laporan Ilmiah Dokumen

Tambahkan tautan Google Drive di bawah ini.

```
([link drive](https://drive.google.com/drive/folders/1Znc95MTW0s4zlw-_ItxSxLXwEtN4KA_w?usp=sharing))
```

---

# 📝 Kesimpulan

Berdasarkan rangkaian kegiatan praktikum yang telah dilaksanakan, dapat disimpulkan bahwa proses instalasi sistem operasi Debian 13 dalam mode headless pada platform virtualisasi VMware Workstation dapat diselesaikan dengan baik dan sesuai prosedur. Setiap tahapan, meliputi pembuatan Virtual Machine, instalasi sistem operasi, pemutakhiran repository dan paket sistem, pemasangan web server Nginx, penyusunan halaman web berbasis HTML, serta konfigurasi port forwarding, telah dilaksanakan secara sistematis dan menghasilkan luaran yang sesuai dengan target praktikum.
## Poin-poin yang Dipelajari

- Memahami proses instalasi Debian 13 dalam mode headless.
- Memahami konfigurasi hostname, user, password, dan partisi harddisk.
- Memahami konfigurasi jaringan dasar pada Debian.
- Memahami penggunaan perintah apt untuk melakukan update sistem.
- Memahami cara memberikan hak akses administrator menggunakan sudo.
- Memahami instalasi dan konfigurasi web server Nginx.
- Memahami pengelolaan layanan menggunakan systemctl.
- Memahami cara melakukan port forwarding pada VMware.
- Memahami cara menguji web server melalui browser host.
- Menambah pengalaman dalam melakukan administrasi server Linux berbasis terminal.

Secara keseluruhan, praktikum ini memberikan pengalaman langsung mengenai instalasi, konfigurasi, dan pengelolaan server Debian 13 secara headless sebagai dasar administrasi sistem operasi Linux.
