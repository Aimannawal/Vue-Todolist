# 🖥️ Vue.js ToDo List Frontend

Frontend sederhana menggunakan **Vue.js** (tanpa framework tambahan) untuk berinteraksi dengan API ToDo List yang dibuat dengan Go Native & MongoDB.

---

## 📦 Fitur

- 📃 Lihat semua catatan
- ✅ Tambah catatan baru
- ❌ Hapus catatan

---

## ⚙️ Cara Install & Build (Linux)

### 1. Clone Proyek

```bash
git clone https://github.com/yourname/vue-todolist.git
cd vue-todolist
```

---

### 2. Install Node.js & npm

> Lewati langkah ini jika Node.js dan npm sudah terinstal.

```bash
sudo apt update
sudo apt install nodejs npm -y
```

Cek versi:

```bash
node -v
npm -v
```

---

### 3. Install Dependency

```bash
npm install
```

---

### 4. Jalankan Mode Development (Opsional)

```bash
npm run dev
```

Buka browser di:  
📍 `http://localhost:5173`

---

### 5. Build Untuk Produksi

```bash
npm run build
```

> File hasil build akan ada di folder `dist/`

---

### 6. Deploy dengan NGINX (Rekomendasi)

#### Salin isi folder `dist` ke `/var/www/html`:

```bash
sudo cp -r dist/* /var/www/html/
```

#### Restart NGINX:

```bash
sudo systemctl restart nginx
```

Akses via:  
📍 `http://IP_ADDRESS` (misalnya `http://192.168.1.10`)

---

## 📋 Contoh UI

UI sangat sederhana, menggunakan Tailwind CSS via CDN untuk styling:

- Input field untuk menambah note
- Tombol untuk hapus note

---

## 🔗 API Endpoint yang Digunakan

Pastikan backend Go kamu berjalan di `http://localhost:8080` atau domain/IP yang sama.

- `GET /notes` – Menampilkan daftar
- `POST /notes` – Menambah catatan
- `DELETE /notes/:id` – Menghapus catatan

---

## 🧑‍💻 Dibuat oleh

Aiman Wafi’i An Nawal  
SMK Telkom Sidoarjo – Backend Developer
