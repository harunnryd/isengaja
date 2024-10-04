
# Chapter 9: Menggunakan Helm di CI/CD Pipeline ⚙️

Pada chapter ini, kita akan membahas bagaimana mengintegrasikan **Helm** ke dalam alur kerja **CI/CD (Continuous Integration/Continuous Delivery)** untuk mempermudah otomatisasi deployment aplikasi di Kubernetes.

---

## Mengapa Helm di CI/CD?

Dengan mengintegrasikan Helm ke pipeline CI/CD, kamu bisa secara otomatis mengelola deployment aplikasi ke Kubernetes setiap kali ada perubahan kode, memastikan bahwa aplikasi selalu up-to-date dan siap untuk di-deploy ke berbagai lingkungan (dev, staging, production).

---

## Langkah-langkah Integrasi Helm di CI/CD

### 1. Membuat Pipeline CI/CD

Sebagai contoh, kita akan menggunakan **GitHub Actions** dan **Google Cloud Platform (GCP)** untuk membuat pipeline CI/CD yang melakukan deployment otomatis menggunakan Helm.

#### Contoh File GitHub Actions (`.github/workflows/helm-deploy.yml`)

```yaml
name: Helm Chart CI/CD

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout Repository
      uses: actions/checkout@v2

    - name: Set up Google Cloud SDK
      uses: google-github-actions/setup-gcloud@v0.2.1
      with:
        project_id: ${{ secrets.GCP_PROJECT_ID }}
        service_account_key: ${{ secrets.GCP_SA_KEY }}

    - name: Authenticate Docker to GCP
      run: |
        echo "${{ secrets.GCP_SA_KEY }}" | docker login -u _json_key --password-stdin https://gcr.io

    - name: Set up Helm
      run: |
        curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash

    - name: Configure kubectl
      run: |
        gcloud container clusters get-credentials my-cluster --zone us-central1-a --project ${{ secrets.GCP_PROJECT_ID }}

    - name: Deploy Helm Chart
      run: |
        helm upgrade --install my-app ./my-chart --namespace production --set image.tag=${{ github.sha }}

```

### 2. Setup Google Cloud SDK dan Helm

Di langkah ini, kita menginstall **Google Cloud SDK** untuk berinteraksi dengan GKE (Google Kubernetes Engine) dan menginstall **Helm** untuk melakukan deployment chart ke Kubernetes cluster.

### 3. Menggunakan `helm upgrade --install`

Perintah ini digunakan untuk melakukan install atau upgrade aplikasi di Kubernetes menggunakan Helm chart. Jika aplikasi sudah di-deploy sebelumnya, Helm akan melakukan upgrade. Jika belum, Helm akan melakukan install:

```bash
helm upgrade --install my-app ./my-chart --namespace production --set image.tag=${{ github.sha }}
```

Kita juga bisa menambahkan `--set` untuk meng-override nilai seperti tag image yang akan digunakan.

---

## Menggunakan Helm di Platform Lain

Selain GCP, pipeline CI/CD ini juga bisa digunakan di platform cloud lain seperti **AWS** atau **Azure**. Cukup sesuaikan langkah konfigurasi kubectl dan autentikasi sesuai dengan cloud provider yang kamu gunakan.

---

## Keuntungan Menggunakan Helm di CI/CD:

1. **Otomatisasi Deployment**: Setiap kali ada perubahan pada repository, aplikasi akan otomatis di-deploy atau di-upgrade di Kubernetes cluster.
2. **Consistency**: Menggunakan Helm memastikan bahwa deployment selalu konsisten di berbagai lingkungan, baik di development, staging, maupun production.
3. **Flexibilitas**: Kamu bisa dengan mudah mengubah konfigurasi deployment menggunakan `values.yaml` atau flag `--set` untuk menyesuaikan dengan kebutuhan lingkungan.

---

## Kesimpulan

Pada **Chapter 9** ini, kita telah membahas cara mengintegrasikan **Helm** ke dalam alur kerja **CI/CD pipeline**, menggunakan GitHub Actions dan GCP sebagai contoh. Integrasi ini memungkinkan deployment otomatis setiap kali ada perubahan kode, memudahkan pengelolaan aplikasi berbasis Kubernetes dengan lebih cepat dan efisien.

Selanjutnya, di **Chapter 10**, kita akan membahas tentang **Helm Chart Testing dengan `ct` (Chart Testing)**
