
# Chapter 7: Testing dan Validasi Helm Charts 🔍

Pada chapter ini, kita akan membahas cara melakukan **testing** dan **validasi** Helm chart untuk memastikan bahwa semua resource Kubernetes yang dihasilkan sudah sesuai dengan ekspektasi. Testing sangat penting untuk memastikan bahwa chart dapat di-deploy dengan baik di berbagai lingkungan.

---

## 1. Menggunakan Helm Lint

**Helm lint** adalah alat bawaan Helm yang digunakan untuk melakukan validasi dasar terhadap chart. Helm lint memeriksa struktur chart, memastikan bahwa file YAML valid, dan memeriksa kesalahan umum lainnya.

Untuk menjalankan Helm lint pada chart, gunakan perintah berikut:

```bash
helm lint ./my-chart
```

Jika terdapat kesalahan atau masalah pada chart, Helm lint akan memberikan feedback yang berguna untuk memperbaikinya.

---

## 2. Testing dengan `helm install --dry-run`

Salah satu cara untuk memverifikasi chart tanpa benar-benar melakukan deployment adalah menggunakan **helm install** dengan flag `--dry-run`. Perintah ini akan menjalankan Helm hingga tahap rendering template, tetapi tidak benar-benar membuat resource di Kubernetes.

```bash
helm install --dry-run --debug my-release ./my-chart
```

Dengan menggunakan `--dry-run`, kamu bisa melihat YAML yang dihasilkan oleh template tanpa melakukan perubahan apa pun di cluster.

---

## 3. Menambahkan Hook Testing

Helm memungkinkan kamu untuk menambahkan **hook** khusus untuk menjalankan testing setelah chart di-install. Kamu bisa menggunakan hook `test-success` untuk mendefinisikan job testing yang dijalankan setelah chart berhasil di-deploy.

Contoh penambahan hook testing pada chart:

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: "{{ .Release.Name }}-test"
  labels:
    helm.sh/hook: test-success
spec:
  template:
    spec:
      containers:
      - name: test-container
        image: busybox
        command: ['sh', '-c', 'echo "Running test"']
      restartPolicy: Never
```

Job testing ini akan dijalankan setelah aplikasi berhasil di-deploy, memastikan bahwa resource yang dihasilkan sesuai dengan ekspektasi.

Untuk menjalankan test setelah install:

```bash
helm test my-release
```

---

## 4. Memverifikasi Resource dengan Kubectl

Setelah melakukan deploy, kamu bisa memverifikasi resource yang dihasilkan dengan menggunakan `kubectl`. Pastikan semua pod, service, dan deployment berjalan sesuai harapan:

```bash
kubectl get pods
kubectl get services
kubectl describe deployment my-deployment
```

Ini akan memberikan rincian tentang status resource yang dihasilkan oleh Helm chart.

---

## 5. Testing pada Lingkungan CI/CD

Jika kamu menggunakan pipeline CI/CD, kamu bisa mengintegrasikan testing Helm chart ke dalam pipeline. Berikut adalah contoh pipeline sederhana menggunakan GitHub Actions:

```yaml
name: Helm Chart CI

on:
  push:
    branches:
      - main

jobs:
  helm-lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Install Helm
        run: |
          curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
      - name: Run Helm Lint
        run: helm lint ./my-chart
      - name: Run Helm Dry-Run
        run: helm install --dry-run --debug my-release ./my-chart
```

Pipeline ini menjalankan linting dan dry-run untuk memastikan bahwa chart bebas dari kesalahan sebelum di-deploy.

---

## Kesimpulan

Pada **Chapter 7** ini, kita telah membahas cara melakukan **testing** dan **validasi** pada Helm chart. Mulai dari menggunakan **helm lint** hingga menambahkan **hook testing** untuk memverifikasi bahwa chart yang dihasilkan bekerja sesuai dengan harapan.

Selanjutnya, di **Chapter 8**, kita akan membahas lebih dalam tentang **best practices dalam penulisan Helm chart** dan bagaimana cara menjaga kualitas chart saat bekerja dalam tim.
