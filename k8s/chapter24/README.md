
# Helm di Kubernetes ⛵

**Helm** adalah package manager untuk Kubernetes yang mempermudah pengelolaan aplikasi di dalam cluster. Dengan Helm, kamu bisa melakukan deployment, upgrade, dan penghapusan aplikasi dengan cepat dan mudah menggunakan template yang disebut **chart**.

## Apa Itu Helm? 🤔

Helm adalah alat untuk **mengelola Kubernetes aplikasi** melalui penggunaan **chart**. **Chart** adalah sekumpulan file konfigurasi YAML yang mendefinisikan resource Kubernetes yang diperlukan untuk menjalankan aplikasi, seperti deployment, service, configmap, dan lainnya. 

Dengan Helm, kamu bisa:
1. Mengelola aplikasi Kubernetes sebagai satu kesatuan (sekumpulan resource).
2. Melakukan upgrade aplikasi secara aman.
3. Memutar balik (rollback) aplikasi ke versi sebelumnya jika diperlukan.

---

## Keuntungan Menggunakan Helm 🛠️

1. **Template-Driven**: Helm menggunakan template YAML yang memudahkan untuk membuat dan mengelola konfigurasi yang dinamis.
2. **Versioning**: Setiap rilis dari aplikasi kamu akan diberi nomor versi, sehingga memudahkan rollback jika ada masalah.
3. **Paket Aplikasi**: Helm menyederhanakan cara aplikasi Kubernetes di-deploy dengan menggunakan chart.
4. **Upgrade dan Rollback Mudah**: Proses update aplikasi dan mengembalikan ke versi sebelumnya bisa dilakukan dengan mudah.

---

## Cara Kerja Helm 🎛️

Helm bekerja dengan menggunakan konsep **chart**, yaitu template untuk deployment aplikasi Kubernetes. Saat kamu menggunakan Helm untuk menginstall aplikasi, Helm akan mengubah template YAML di dalam chart menjadi resource Kubernetes yang sesungguhnya, seperti deployment, service, ingress, dll.

Helm juga memungkinkan kamu untuk memodifikasi parameter-parameter chart sesuai kebutuhan kamu, tanpa perlu menulis ulang YAML secara manual.

---

## Menginstall Helm ⛵

Berikut adalah langkah-langkah dasar untuk menginstall Helm:

1. Unduh Helm CLI dari [situs resmi Helm](https://helm.sh/).
2. Install Helm CLI di lokal:
   ```bash
   curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
   ```

Setelah itu, kamu bisa memulai menggunakan Helm untuk mengelola aplikasi di Kubernetes.

---

## Menggunakan Helm Chart 🛠️

Berikut adalah contoh dasar bagaimana menggunakan Helm untuk menginstall aplikasi **nginx** dari Helm chart repository resmi.

### 1. Menambahkan Repository Helm:
```bash
helm repo add stable https://charts.helm.sh/stable
helm repo update
```

### 2. Menginstall Aplikasi Nginx:
```bash
helm install my-nginx stable/nginx-ingress
```

### 3. Melihat Rilis Helm:
Untuk melihat rilis aplikasi yang diinstall menggunakan Helm:
```bash
helm list
```

### 4. Menghapus Aplikasi:
Untuk menghapus aplikasi yang diinstall menggunakan Helm:
```bash
helm uninstall my-nginx
```

---

## Membuat Helm Chart Sendiri 🛠️

Jika kamu ingin membuat Helm chart untuk aplikasi kamu sendiri, kamu bisa melakukannya dengan menggunakan perintah:

```bash
helm create my-chart
```

Ini akan membuat struktur direktori dasar untuk chart baru.

### Struktur Helm Chart:
- **Chart.yaml**: File metadata yang mendeskripsikan chart.
- **values.yaml**: File yang berisi nilai default untuk chart.
- **templates/**: Folder ini berisi file-file template YAML untuk resource Kubernetes.

---

## Upgrade dan Rollback dengan Helm 🔄

Salah satu keunggulan utama Helm adalah kemudahan dalam melakukan **upgrade** dan **rollback** aplikasi.

### 1. Upgrade Aplikasi:
Untuk melakukan upgrade pada aplikasi yang sudah diinstall, kamu bisa menggunakan perintah:
```bash
helm upgrade my-nginx stable/nginx-ingress
```

### 2. Rollback Aplikasi:
Jika terjadi masalah setelah upgrade, kamu bisa mengembalikan aplikasi ke versi sebelumnya dengan perintah:
```bash
helm rollback my-nginx 1
```

---

## Helm Values 📦

File **values.yaml** digunakan untuk mengatur konfigurasi default dari chart. Kamu juga bisa menimpa nilai default ini dengan memberikan file values yang diubah ketika melakukan install atau upgrade:

```bash
helm install my-app ./my-chart -f custom-values.yaml
```

Ini memberikan fleksibilitas bagi tim DevOps untuk menyesuaikan konfigurasi tanpa mengubah file template asli.

---

## Helm Repositories 🗂️

Helm mendukung penggunaan **repository** untuk menyimpan chart. Beberapa repository Helm yang populer:
- **Artifact Hub**: https://artifacthub.io/
- **Bitnami**: https://charts.bitnami.com/bitnami
- **Stable**: https://charts.helm.sh/stable (deprecated, tapi masih banyak digunakan)
