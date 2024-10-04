
# Membuat dan Mengelola Helm Charts Repository 🛠️🚀

Helm adalah package manager untuk Kubernetes yang memudahkan dalam deployment, pengelolaan, dan versioning aplikasi berbasis container di Kubernetes. Di sini kita akan belajar cara membuat serta mengelola Helm charts dan repository.

## Langkah-langkah Membuat dan Mengatur Helm Charts

### 1. Instalasi Helm
Sebelum membuat Helm chart, kita harus menginstal Helm terlebih dahulu.

```bash
# Instal Helm di macOS menggunakan Homebrew
brew install helm
```

### 2. Membuat Helm Chart Baru
Helm menyediakan template untuk mempermudah pembuatan chart.

```bash
helm create nama-chart
```

Struktur folder chart akan terlihat seperti ini:

```
nama-chart/
  ├── charts/
  ├── templates/
  ├── values.yaml
  └── Chart.yaml
```

- **templates/**: Berisi manifest Kubernetes seperti Deployment, Service, Ingress.
- **values.yaml**: Konfigurasi default yang dapat di-overwrite saat instalasi.
- **Chart.yaml**: Metadata tentang chart.

### 3. Deploy Aplikasi Menggunakan Helm Chart
Setelah template siap, kamu bisa melakukan deploy ke Kubernetes.

```bash
helm install nama-release ./nama-chart
```

### 4. Mengelola Versi dengan Helm
Untuk upgrade aplikasi:

```bash
helm upgrade nama-release ./nama-chart
```

### 5. Membuat Helm Chart Repository
Setelah chart siap, kamu bisa menyimpan chart di repository untuk digunakan orang lain.

```bash
helm repo index . --url https://example.com/charts
```

### 6. Mengelola Chart di Repository
Untuk memperbarui chart:

```bash
helm repo index .
```

Tambahkan repository ke Helm:

```bash
helm repo add nama-repo https://example.com/charts
```

---

## Kesimpulan:
- Helm membuat proses deployment dan pengelolaan aplikasi di Kubernetes lebih mudah.
- Kamu bisa membuat dan mengatur repository Helm sendiri untuk berbagi chart.
- Helm chart membantu dalam mengelola versi aplikasi dan memperbarui deployment.

