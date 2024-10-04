
# Chapter 4: Mengelola Helm Charts yang Kompleks dan Kustomisasi `values.yaml` 🎛️

Pada chapter ini, kita akan belajar bagaimana mengelola Helm chart yang lebih kompleks dan bagaimana melakukan kustomisasi melalui `values.yaml` untuk menangani berbagai kasus penggunaan yang berbeda.

---

## 1. Mengelola Helm Chart yang Lebih Kompleks

Helm memungkinkan kita untuk mengelola aplikasi yang memiliki lebih dari satu komponen (misalnya database, backend, frontend) di dalam satu chart. Setiap komponen dapat didefinisikan di dalam `templates/` dengan YAML yang berbeda untuk mengelola berbagai resource Kubernetes.

### Struktur Chart yang Kompleks

Misalnya, aplikasi yang memiliki backend dan frontend bisa memiliki struktur chart seperti ini:

```
my-app/
  ├── Chart.yaml       # Metadata tentang chart
  ├── values.yaml      # Konfigurasi default
  ├── templates/
  │   ├── backend-deployment.yaml
  │   ├── frontend-deployment.yaml
  │   ├── backend-service.yaml
  │   └── frontend-service.yaml
```

### Menggunakan `.Values` untuk Kustomisasi
Di dalam setiap template YAML, kamu bisa menggunakan `.Values` untuk mengambil nilai dari `values.yaml`. Ini memungkinkan pengguna chart untuk mengkonfigurasi aplikasi tanpa harus memodifikasi template secara langsung.

Contoh `backend-deployment.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Values.backend.name }}
spec:
  replicas: {{ .Values.backend.replicaCount }}
  template:
    spec:
      containers:
        - name: {{ .Values.backend.containerName }}
          image: {{ .Values.backend.image }}
          resources:
            limits:
              cpu: {{ .Values.backend.resources.cpu }}
              memory: {{ .Values.backend.resources.memory }}
```

Contoh isi `values.yaml`:

```yaml
backend:
  name: my-backend
  replicaCount: 3
  containerName: backend-container
  image: nginx:1.21
  resources:
    cpu: "200m"
    memory: "256Mi"
```

Dengan pendekatan ini, pengguna chart dapat dengan mudah mengatur berbagai konfigurasi hanya dengan mengubah nilai di `values.yaml`.

---

## 2. Overriding `values.yaml` Saat Install atau Upgrade

Selain mengedit langsung `values.yaml`, kamu juga bisa meng-override nilainya saat melakukan instalasi atau upgrade chart dengan menggunakan flag `--set`.

```bash
helm install my-release ./my-app --set backend.replicaCount=5
```

Perintah ini akan meng-override nilai `replicaCount` dari 3 menjadi 5 tanpa mengubah file `values.yaml`.

### File Override Menggunakan `-f`

Jika ada banyak nilai yang ingin di-override, kamu juga bisa menggunakan file lain untuk menggantikan atau menambahkan nilai dari `values.yaml`.

```bash
helm install my-release ./my-app -f custom-values.yaml
```

---

## 3. Hierarki `values.yaml`

Helm mendukung hierarki yang kompleks di `values.yaml`, sehingga memungkinkan pengaturan nested values. Ini sangat berguna jika chart memiliki banyak subkomponen atau modul yang memerlukan konfigurasi berbeda.

Misalnya, pengaturan untuk backend dan frontend di `values.yaml` bisa dibuat seperti ini:

```yaml
backend:
  replicaCount: 2
  image: nginx:1.19
  resources:
    cpu: "100m"
    memory: "128Mi"

frontend:
  replicaCount: 3
  image: nginx:1.21
  resources:
    cpu: "200m"
    memory: "256Mi"
```

Chart dapat menggunakan `.Values.backend` atau `.Values.frontend` untuk mengambil konfigurasi sesuai dengan komponen yang dibutuhkan.

---

## 4. Tips Mengelola Helm Chart yang Kompleks

1. **Pisahkan Komponen Besar**: Jika aplikasi memiliki banyak komponen besar (misalnya database, cache, API), pertimbangkan untuk memecahnya menjadi beberapa chart kecil. Kamu bisa menggunakan dependensi chart untuk mengatur ini.
   
2. **Gunakan Default yang Masuk Akal**: Berikan nilai default yang masuk akal di `values.yaml` agar pengguna chart yang tidak familiar tetap bisa menggunakan chart dengan konfigurasi minimal.

3. **Dokumentasikan Konfigurasi**: Pastikan setiap parameter yang ada di `values.yaml` didokumentasikan dengan baik, sehingga pengguna chart bisa memahami apa yang bisa mereka ubah dan bagaimana pengaruhnya terhadap aplikasi.

---

## Kesimpulan:
Pada **Chapter 4** ini, kita telah membahas cara mengelola Helm chart yang lebih kompleks dan bagaimana melakukan kustomisasi melalui `values.yaml`. Kustomisasi ini memungkinkan Helm chart untuk lebih fleksibel dan reusable di berbagai lingkungan.

Selanjutnya, di **Chapter 5**, kita akan mempelajari lebih dalam tentang **Helm hooks** dan bagaimana cara menggunakannya untuk mengelola proses deployment yang lebih dinamis.
