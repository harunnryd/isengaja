
# Chapter 1: Pengenalan Helm dan Helm Charts 🛠️

## Apa itu Helm?
Helm adalah package manager untuk Kubernetes yang dirancang untuk membuat deployment, pengelolaan, dan upgrade aplikasi lebih mudah. Helm memungkinkan kita mengemas aplikasi Kubernetes dalam **charts**, yang merupakan paket berisi definisi resource Kubernetes dan konfigurasi default untuk aplikasi tersebut.

---

## Manfaat Menggunakan Helm:
1. **Sederhana dan Konsisten**: Dengan Helm, kamu bisa menggunakan satu definisi chart untuk men-deploy aplikasi ke berbagai lingkungan (dev, staging, production) dengan sedikit atau tanpa modifikasi.
2. **Versioning**: Helm menyediakan manajemen versi untuk chart, sehingga memudahkan rollback atau upgrade aplikasi ke versi tertentu.
3. **Mudah Dikelola**: Chart Helm memungkinkan kita untuk mendefinisikan aplikasi yang kompleks dan memiliki banyak komponen dengan cara yang terstruktur dan rapi.
4. **Bersifat Reusable**: Kamu bisa menggunakan chart yang sama berulang kali dan di-customize sesuai kebutuhan dengan file `values.yaml`.

---

## Helm Chart: Apa yang Ada di Dalamnya?
Helm chart berisi kumpulan file yang mendefinisikan resource Kubernetes, termasuk file template yang digunakan Helm untuk membuat YAML Kubernetes saat di-deploy. Struktur umum Helm chart seperti berikut:

```
my-app/
  ├── Chart.yaml       # Metadata tentang chart
  ├── values.yaml      # Konfigurasi default chart
  ├── templates/       # Template manifest Kubernetes
  ├── charts/          # Dependensi chart lainnya (opsional)
```

- **`Chart.yaml`**: Berisi metadata seperti nama chart, versi, dan deskripsi.
- **`values.yaml`**: Menyediakan konfigurasi default yang bisa di-overwrite saat deploy.
- **`templates/`**: Folder yang menyimpan template manifest seperti Deployment, Service, Ingress.
- **`charts/`**: Jika chart memiliki dependensi ke chart lain, maka akan disimpan di sini.

---

## Cara Kerja Helm:
Helm bekerja dengan dua komponen utama:

1. **Helm Client**: CLI yang digunakan untuk membuat, mengelola, dan men-deploy chart.
2. **Helm Server (Tiller)**: Di versi Helm sebelumnya, Tiller berfungsi sebagai server-side komponen yang menerima perintah dari Helm Client. Namun, Tiller telah dihapus di versi Helm 3, sehingga semua komando dijalankan langsung dari client ke cluster Kubernetes.

## Proses Deploy Aplikasi dengan Helm:
1. **Helm Package**: Helm mengemas chart menjadi paket yang bisa di-deploy.
2. **Install**: Helm melakukan `helm install` untuk deploy aplikasi dengan chart yang telah dibuat.
3. **Upgrade**: Helm bisa digunakan untuk memperbarui aplikasi dengan `helm upgrade`.
4. **Rollback**: Jika ada masalah, kita bisa mengembalikan aplikasi ke versi sebelumnya dengan `helm rollback`.

---

Pada **Chapter 1** ini, kita mengenal dasar dari Helm, struktur Helm chart, dan bagaimana Helm mempermudah deployment di Kubernetes. Selanjutnya, di **Chapter 2**, kita akan membahas bagaimana membuat Helm chart pertama kita dan melakukan deployment ke Kubernetes.
