# StreamFlow v2.0 - Panduan Instalasi dan Menjalankan Aplikasi

## 🚀 **Cara Cepat (Instalasi Otomatis)**

### Untuk VPS/Server Linux:
```bash
curl -o install.sh https://raw.githubusercontent.com/bangtutorial/streamflow/main/install.sh && chmod +x install.sh && ./install.sh
```

Script ini akan otomatis:
- Install Node.js, FFmpeg, dan dependencies
- Clone repository
- Setup aplikasi
- Menjalankan dengan PM2

## 🔧 **Instalasi Manual**

### 1. **Persiapan System Requirements**

**Minimum Requirements:**
- Node.js v18+ 
- FFmpeg
- 1 Core CPU & 1GB RAM
- Port 7575 (bisa diubah)

### 2. **Install Dependencies**

**Ubuntu/Debian:**
```bash
# Update system
sudo apt update && sudo apt upgrade -y

# Install Node.js
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt-get install -y nodejs

# Install FFmpeg dan Git
sudo apt install ffmpeg git -y

# Verifikasi instalasi
node --version
npm --version
ffmpeg -version
```

**CentOS/RHEL:**
```bash
# Install Node.js
curl -fsSL https://rpm.nodesource.com/setup_22.x | sudo bash -
sudo yum install -y nodejs

# Install FFmpeg dan Git
sudo yum install epel-release -y
sudo yum install ffmpeg git -y
```

### 3. **Clone dan Setup Project**

```bash
# Clone repository
git clone https://github.com/bangtutorial/streamflow
cd streamflow

# Install dependencies
npm install

# Generate session secret
npm run generate-secret
```

### 4. **Konfigurasi Environment**

File `.env` akan dibuat otomatis, tapi bisa diedit:
```bash
nano .env
```

Contoh isi `.env`:
```env
PORT=7575
SESSION_SECRET=your_generated_secret_here
NODE_ENV=development
```

### 5. **Setup Firewall**

```bash
# Buka port aplikasi
sudo ufw allow 7575

# Aktifkan firewall
sudo ufw enable

# Cek status
sudo ufw status
```

## ▶️ **Cara Menjalankan Aplikasi**

### **Opsi 1: Development Mode**
```bash
# Jalankan dalam mode development
npm run dev

# Atau langsung dengan node
node app.js
```

### **Opsi 2: Production Mode dengan PM2**
```bash
# Install PM2 globally
sudo npm install -g pm2

# Jalankan aplikasi
pm2 start app.js --name streamflow

# Lihat status
pm2 status

# Lihat logs
pm2 logs streamflow

# Restart aplikasi
pm2 restart streamflow

# Stop aplikasi
pm2 stop streamflow
```

### **Opsi 3: Docker**
```bash
# Build dan jalankan dengan Docker Compose
docker-compose up --build

# Jalankan di background
docker-compose up -d
```

## 🌐 **Akses Aplikasi**

Setelah aplikasi berjalan, buka browser dan akses:
```
http://IP_SERVER:7575
```

Contoh:
- Local: `http://localhost:7575`
- VPS: `http://88.12.34.56:7575`

## 👤 **Setup Akun Admin**

1. **Pertama kali akses**, akan muncul halaman setup account
2. **Buat username dan password** untuk admin
3. **Upload foto profil** (opsional)
4. **Klik "Complete Setup"**

⚠️ **PENTING**: Setelah membuat akun, lakukan **Sign Out** kemudian **restart aplikasi**:
```bash
pm2 restart streamflow
```

## 🔧 **Troubleshooting**

### **Permission Error**
```bash
chmod -R 755 public/uploads/
```

### **Port Already in Use**
```bash
# Cek proses yang menggunakan port
sudo lsof -i :7575

# Kill proses jika diperlukan
sudo kill -9 <PID>
```

### **Database Error**
```bash
# Reset database (PERINGATAN: akan menghapus semua data)
rm db/*.db

# Restart aplikasi
pm2 restart streamflow
```

### **FFmpeg Not Found**
```bash
# Install FFmpeg jika belum ada
sudo apt install ffmpeg -y

# Atau untuk CentOS
sudo yum install ffmpeg -y
```

## 🔄 **Update Aplikasi**

```bash
# Masuk ke direktori aplikasi
cd streamflow

# Pull update terbaru
git pull origin main

# Install dependencies baru (jika ada)
npm install

# Restart aplikasi
pm2 restart streamflow
```

## 🔐 **Reset Password**

Jika lupa password admin:
```bash
cd streamflow
node reset-password.js
```

## ⏰ **Setup Timezone**

Untuk scheduled streaming yang akurat:
```bash
# Cek timezone saat ini
timedatectl status

# Set timezone ke WIB
sudo timedatectl set-timezone Asia/Jakarta

# Restart aplikasi
pm2 restart streamflow
```

## 📊 **Monitoring**

### **Cek Status Aplikasi**
```bash
# Status PM2
pm2 status

# Logs real-time
pm2 logs streamflow --lines 50

# Monitor resource usage
pm2 monit
```

### **Cek System Resources**
```bash
# CPU dan Memory usage
htop

# Disk usage
df -h

# Network usage
iftop
```

## 🐳 **Docker Deployment**

### **Persiapan**
```bash
# Buat file .env
cat > .env << EOF
PORT=7575
SESSION_SECRET=your_random_secret_here
NODE_ENV=development
EOF
```

### **Build dan Run**
```bash
# Build dan jalankan
docker-compose up --build

# Jalankan di background
docker-compose up -d

# Lihat logs
docker-compose logs -f

# Stop containers
docker-compose down
```

### **Reset Password (Docker)**
```bash
docker-compose exec app node reset-password.js
```

## 🔧 **Advanced Configuration**

### **Custom Port**
Edit file `.env`:
```env
PORT=8080
```

### **HTTPS Setup**
Untuk production, gunakan reverse proxy seperti Nginx:
```nginx
server {
    listen 80;
    server_name yourdomain.com;
    
    location / {
        proxy_pass http://localhost:7575;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

## 📱 **Mobile Access**

Aplikasi sudah responsive dan bisa diakses dari mobile browser. Pastikan port 7575 terbuka untuk akses dari luar.

## 🎯 **Quick Start Checklist**

- [ ] Install Node.js v18+
- [ ] Install FFmpeg
- [ ] Clone repository
- [ ] Run `npm install`
- [ ] Run `npm run generate-secret`
- [ ] Open firewall port 7575
- [ ] Start with `pm2 start app.js --name streamflow`
- [ ] Access `http://IP:7575`
- [ ] Create admin account
- [ ] Sign out and restart app
- [ ] Ready to stream! 🎬

---

**Need Help?** 
- GitHub Issues: https://github.com/bangtutorial/streamflow/issues
- YouTube Tutorial: https://youtube.com/@bangtutorial