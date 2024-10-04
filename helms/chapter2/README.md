
# Chapter 2: Membuat Helm Chart Pertama dan Deployment 🚀

Pada chapter ini, kita akan membuat **Helm chart** pertama dan menggunakannya untuk mendefinisikan aplikasi serta melakukan deployment di Kubernetes. Langkah-langkah ini akan memperkenalkan dasar dari bagaimana mengelola aplikasi dengan Helm.

---

## Langkah-langkah Membuat Helm Chart Pertama

### 1. Buat Helm Chart Baru
Helm menyediakan template bawaan untuk mempermudah pembuatan chart. Kamu bisa memulai dengan perintah:

```bash
helm create my-app
```

Perintah ini akan membuat sebuah chart baru dengan struktur direktori seperti berikut:

```
my-app/
  ├── Chart.yaml       # Metadata tentang chart
  ├── values.yaml      # Konfigurasi default
  ├── templates/       # Template manifest Kubernetes (seperti Deployment, Service, dll.)
  ├── charts/          # Dependensi chart lainnya (opsional)
```

### 2. Edit `values.yaml`
`values.yaml` berisi konfigurasi default untuk chart. Kamu bisa menyesuaikan nilai default ini, misalnya untuk image aplikasi yang akan digunakan, jumlah replicas, dan resource limits.

```yaml
# Contoh isi values.yaml
replicaCount: 2

image:
  repository: nginx
  tag: "1.21"
  pullPolicy: IfNotPresent

resources:
  limits:
    cpu: 100m
    memory: 128Mi
```

### 3. Edit Template Manifest di `templates/`
Di folder `templates/`, kamu bisa membuat file YAML Kubernetes seperti Deployment dan Service yang akan digunakan untuk deploy aplikasi. Misalnya, kita bisa membuat file `deployment.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Chart.Name }}
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      app: {{ .Chart.Name }}
  template:
    metadata:
      labels:
        app: {{ .Chart.Name }}
    spec:
      containers:
      - name: {{ .Chart.Name }}
        image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
        resources:
          limits:
            cpu: {{ .Values.resources.limits.cpu }}
            memory: {{ .Values.resources.limits.memory }}
```

### 4. Deploy Helm Chart ke Kubernetes
Setelah membuat dan mengedit chart, langkah berikutnya adalah melakukan deployment ke cluster Kubernetes:

```bash
helm install my-release ./my-app
```

- **`my-release`**: Nama release yang kamu pilih untuk deployment ini.
- **`./my-app`**: Path ke chart yang baru kamu buat.

Helm akan membuat resource di Kubernetes berdasarkan definisi di template.

### 5. Verifikasi Deployment
Setelah deployment selesai, kamu bisa memverifikasi bahwa aplikasi berjalan dengan benar menggunakan perintah `kubectl`:

```bash
kubectl get pods
kubectl get svc
```

### 6. Upgrade Helm Chart
Jika kamu melakukan perubahan pada chart, misalnya mengganti image versi atau konfigurasi lainnya, kamu bisa menggunakan `helm upgrade` untuk memperbarui deployment yang sudah ada:

```bash
helm upgrade my-release ./my-app
```

### 7. Rollback Deployment
Jika ada masalah setelah upgrade, Helm memudahkan untuk rollback ke versi sebelumnya:

```bash
helm rollback my-release
```

---

## Kesimpulan:
Pada **Chapter 2** ini, kita telah membuat Helm chart pertama dan melakukan deployment ke Kubernetes. Kita juga belajar bagaimana mengelola aplikasi dengan `helm upgrade` dan `helm rollback` untuk mempermudah pengelolaan versi aplikasi.

Di **Chapter 3**, kita akan mengeksplor lebih jauh tentang bagaimana cara membuat dan mengelola Helm repository untuk berbagi Helm charts.
