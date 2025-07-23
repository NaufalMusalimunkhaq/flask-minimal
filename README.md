# flask-minimal

Proyek starter minimal Flask untuk pengembangan web cepat. Berisi aplikasi Flask dalam satu file dengan struktur template dan aset statis dasar, dirancang untuk kesederhanaan dan produktivitas.

## Fitur Utama

* Aplikasi Flask dalam satu file (`app.py`) untuk memaksimalkan produktivitas dan kesederhanaan.
* Struktur template HTML dasar dengan styling dan JavaScript minimal.
* Setup proyek yang sederhana dan intuitif tanpa dependensi yang tidak perlu.

## Prasyarat

Sebelum memulai, pastikan Anda memiliki:

* Akses ke instance EC2 Ubuntu.
* Python 3 dan pip terinstal.
* Akses root atau sudo ke server.

## Instalasi

1. **Clone repositori:**

   ```bash
   git clone https://github.com/NaufalMusalimunkhaq/flask-minimal.git
   cd flask-minimal
   ```

2. **Buat dan aktifkan virtual environment:**

   ```bash
   python3 -m venv venv
   source venv/bin/activate
   ```

3. **Instal dependensi:**

   ```bash
   pip install -r requirements.txt
   ```

4. **Jalankan aplikasi secara lokal:**

   ```bash
   python app.py
   ```

   Akses aplikasi melalui browser di `http://localhost:5000`.

## Menjalankan Aplikasi Flask Secara Otomatis Saat Reboot dengan systemd

Untuk memastikan aplikasi Flask Anda berjalan otomatis saat instance EC2 Ubuntu di-reboot, ikuti langkah-langkah berikut:


### 1. Buat File Service systemd

Buat file service systemd untuk aplikasi Flask Anda:

```bash
sudo nano /etc/systemd/system/flask-minimal.service
```

Tambahkan konfigurasi berikut:

```
[Unit]
Description=Flask Minimal Web Application
After=network.target

[Service]
User=ubuntu
WorkingDirectory=/home/ubuntu/flask-minimal
ExecStart=/home/ubuntu/flask-minimal/venv/bin/gunicorn --workers 3 --bind unix:flask-minimal.sock -m 007 app:app
Restart=always

[Install]
WantedBy=multi-user.target
```

Gantilah `/home/ubuntu/flask-minimal` dengan path ke direktori proyek Anda.

### 2. Reload systemd dan Mulai Service

Reload systemd untuk membaca file service baru dan mulai service:

```bash
sudo systemctl daemon-reload
sudo systemctl start flask-minimal
sudo systemctl enable flask-minimal
```

Perintah `enable` memastikan aplikasi dimulai otomatis saat boot.

### 3. Verifikasi Status Service

Periksa status service untuk memastikan aplikasi berjalan dengan baik:

```bash
sudo systemctl status flask-minimal
```

Jika statusnya "active (running)", berarti aplikasi Anda berjalan dengan baik.

### 4. Uji Otomatisasi Setelah Reboot

Reboot instance EC2 Anda untuk menguji apakah aplikasi otomatis berjalan setelah reboot:

```bash
sudo reboot
```

Setelah reboot, periksa status service:

```bash
sudo systemctl status flask-minimal
```

Jika statusnya "active (running)", berarti aplikasi Anda berhasil diatur untuk berjalan otomatis setelah reboot.

## Lisensi

Proyek ini dilisensikan di bawah MIT License

---

README ini dirancang untuk memberikan panduan yang jelas dan mudah diikuti dalam menyiapkan aplikasi Flask minimal di EC2 Ubuntu dengan konfigurasi otomatis menggunakan systemd. Dengan mengikuti langkah-langkah ini, Anda dapat memastikan aplikasi Anda berjalan secara efisien dan otomatis setelah reboot.
