
# CI/CD di Kubernetes 🚀

**Continuous Integration (CI)** dan **Continuous Deployment (CD)** adalah proses penting dalam pengembangan modern yang memungkinkan tim untuk merilis perubahan secara cepat dan aman. Dengan CI/CD, setiap perubahan kode diuji secara otomatis dan di-deploy ke lingkungan produksi atau staging tanpa campur tangan manual.

Dalam konteks **Kubernetes**, CI/CD digunakan untuk secara otomatis membangun, menguji, dan mendistribusikan aplikasi ke dalam cluster Kubernetes.

## Mengapa CI/CD di Kubernetes? 🤔

Dengan menggunakan CI/CD di Kubernetes, kamu bisa:
1. **Otomatisasi Deployment**: Setiap perubahan pada kode atau konfigurasi dapat langsung di-deploy ke Kubernetes setelah melewati pipeline CI/CD.
2. **Reliability**: Pipeline CI/CD memastikan bahwa aplikasi diuji secara menyeluruh sebelum dirilis ke lingkungan produksi.
3. **Scalability**: Kubernetes mempermudah scaling aplikasi, dan dengan CI/CD, scaling ini bisa dilakukan secara otomatis berdasarkan pipeline yang terintegrasi.

---

## Arsitektur CI/CD di Kubernetes 🔧

Berikut adalah contoh pipeline CI/CD yang melibatkan:
- **GitHub** sebagai repository kode.
- **Google Cloud Platform (GCP)** sebagai penyedia infrastruktur cloud.
- **Google Kubernetes Engine (GKE)** sebagai cluster Kubernetes untuk hosting aplikasi.
- **GitHub Actions** untuk pipeline CI/CD.

### Alur Kerja CI/CD:
1. Developer melakukan push perubahan ke GitHub.
2. GitHub Actions menjalankan pipeline untuk membangun image Docker, menjalankan tes, dan melakukan deploy ke GKE.
3. Setelah tes lulus, aplikasi secara otomatis di-deploy ke cluster Kubernetes.

---

## Contoh Pipeline CI/CD dengan GitHub Actions 🛠️

Berikut adalah contoh YAML file untuk pipeline CI/CD yang menggunakan GitHub Actions. Pipeline ini akan membangun aplikasi, membuat image Docker, mendorong image ke **Google Container Registry (GCR)**, dan melakukan deployment ke cluster GKE.

```yaml
name: CI/CD Pipeline

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Set up Google Cloud SDK
      uses: google-github-actions/setup-gcloud@v0.2.0
      with:
        project_id: ${{ secrets.GCP_PROJECT_ID }}
        service_account_key: ${{ secrets.GCP_SA_KEY }}

    - name: Configure Docker to use the gcloud command-line tool as a credential helper
      run: |
        gcloud auth configure-docker

    - name: Build Docker image
      run: |
        docker build -t gcr.io/${{ secrets.GCP_PROJECT_ID }}/my-app:$GITHUB_SHA .

    - name: Push Docker image to GCR
      run: |
        docker push gcr.io/${{ secrets.GCP_PROJECT_ID }}/my-app:$GITHUB_SHA

  deploy:
    needs: build
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Set up Google Cloud SDK
      uses: google-github-actions/setup-gcloud@v0.2.0
      with:
        project_id: ${{ secrets.GCP_PROJECT_ID }}
        service_account_key: ${{ secrets.GCP_SA_KEY }}

    - name: Deploy to GKE
      run: |
        kubectl set image deployment/my-app my-app=gcr.io/${{ secrets.GCP_PROJECT_ID }}/my-app:$GITHUB_SHA
```

---

### Penjelasan Pipeline CI/CD:
- **Checkout code**: Pipeline mulai dengan melakukan checkout terhadap kode di GitHub.
- **Set up Google Cloud SDK**: Mengkonfigurasi Google Cloud SDK untuk menggunakan service account di GCP.
- **Build Docker image**: Pipeline membangun image Docker untuk aplikasi dan memberikan tag sesuai dengan SHA dari commit.
- **Push Docker image**: Image yang sudah dibangun kemudian di-push ke Google Container Registry (GCR).
- **Deploy to GKE**: Setelah image di-push, pipeline melakukan deploy ke Google Kubernetes Engine (GKE) dengan meng-update image deployment di cluster Kubernetes.

---

## Menyiapkan Cluster Kubernetes di GKE 🛠️

Sebelum menjalankan pipeline CI/CD, pastikan kamu sudah menyiapkan cluster di **Google Kubernetes Engine (GKE)**. Berikut adalah langkah-langkah singkat untuk membuat cluster GKE:

1. Buka Google Cloud Console dan buat project baru.
2. Buka **Kubernetes Engine** dan buat cluster baru.
3. Unduh **Google Cloud SDK** dan autentikasi ke akun GCP kamu dengan perintah berikut:
   ```bash
   gcloud auth login
   ```
4. Hubungkan ke cluster GKE dengan perintah:
   ```bash
   gcloud container clusters get-credentials <cluster-name> --zone <zone>
   ```

Setelah cluster siap, kamu bisa menjalankan pipeline CI/CD untuk deployment otomatis ke Kubernetes.

---

## Melihat Status Deployment

Setelah pipeline berhasil dijalankan, kamu bisa melihat status deployment dan pod yang berjalan di cluster GKE dengan perintah berikut:

```bash
kubectl get deployments
kubectl get pods
```
