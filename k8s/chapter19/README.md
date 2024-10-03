
# Deployment di Kubernetes 🚀

**Deployment** di Kubernetes adalah resource yang memungkinkan kamu untuk mengelola aplikasi **stateless**. Dengan **Deployment**, kamu bisa melakukan **scaling**, **rollout**, **rollback**, dan **update** pada aplikasi dengan mudah. Deployment menyediakan cara untuk mengelola aplikasi yang tidak memerlukan state atau penyimpanan persisten.

## Apa Itu Deployment? 🤔

**Deployment** adalah salah satu resource yang paling umum digunakan di Kubernetes untuk mengelola pod-pod stateless. Pod-pod yang dikelola oleh Deployment bersifat **identik**, dan Deployment memastikan bahwa jumlah pod yang diinginkan selalu berjalan. Jika ada pod yang gagal, Deployment akan membuat pod baru untuk menggantikannya.

### Fitur Deployment:
1. **Rolling Update**: Deployment memungkinkan kamu untuk mengupdate aplikasi secara bertahap tanpa downtime.
2. **Rollback**: Kamu bisa dengan mudah kembali ke versi aplikasi sebelumnya jika terjadi masalah setelah update.
3. **Scaling**: Deployment memungkinkan scaling aplikasi secara horizontal dengan menambah atau mengurangi jumlah pod.

---

## Membuat Deployment 🛠️

Berikut adalah contoh YAML file untuk membuat Deployment yang menjalankan aplikasi berbasis **nginx**.

### Contoh YAML Deployment:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
```

### Penjelasan:
- **replicas**: Menentukan jumlah pod yang akan dijalankan (dalam contoh ini, 3 pod nginx).
- **selector**: Deployment akan mencari pod yang memiliki label **app: nginx** dan mengelolanya.
- **template**: Template pod yang akan dibuat oleh Deployment, dalam hal ini container **nginx** yang akan berjalan di port 80.

---

## Rolling Update dalam Deployment 🔄

**Rolling Update** memungkinkan kamu untuk memperbarui aplikasi secara bertahap tanpa menghentikan layanan. Kubernetes akan memperbarui pod satu per satu atau sesuai konfigurasi, memastikan bahwa aplikasi tetap tersedia selama proses update.

Untuk melakukan rolling update, kamu cukup mengupdate image yang digunakan dalam Deployment:

```bash
kubectl set image deployment/nginx-deployment nginx=nginx:1.19.1
```

Ini akan memperbarui **nginx** ke versi 1.19.1, dan Deployment akan mengganti pod yang ada dengan pod baru yang menggunakan versi terbaru.

---

## Rollback Deployment 🔙

Jika terjadi kesalahan atau masalah setelah update, kamu bisa melakukan **rollback** ke versi sebelumnya dari aplikasi. Untuk melakukan rollback, gunakan perintah berikut:

```bash
kubectl rollout undo deployment/nginx-deployment
```

Ini akan mengembalikan Deployment ke versi yang sebelumnya berjalan.

---

## Scaling Deployment 📈

Kubernetes memungkinkan kamu untuk melakukan **scaling** pada aplikasi dengan menambah atau mengurangi jumlah pod yang berjalan. Untuk melakukan scaling pada Deployment, gunakan perintah:

```bash
kubectl scale deployment/nginx-deployment --replicas=5
```

Ini akan menambah jumlah pod nginx yang berjalan menjadi 5.

---

## Mengelola Deployment 🧰

### Melihat Deployment:
Untuk melihat Deployment yang sedang berjalan di cluster, gunakan perintah berikut:

```bash
kubectl get deployments
```

### Output Contoh:
```bash
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   3/3     3            3           2h
```

### Melihat Detail Deployment:
Untuk melihat detail dari Deployment, gunakan perintah:

```bash
kubectl describe deployment nginx-deployment
```

---

## Menghapus Deployment 🗑️

Jika kamu ingin menghapus Deployment, gunakan perintah berikut:

```bash
kubectl delete deployment nginx-deployment
```

Ini akan menghapus Deployment beserta semua pod yang dikelola olehnya.

---

## Deployment vs StatefulSet: Apa Bedanya? ⚖️

- **Deployment**: Digunakan untuk aplikasi **stateless**, yang tidak memerlukan penyimpanan persisten atau identitas jaringan yang tetap.
- **StatefulSet**: Digunakan untuk aplikasi **stateful** yang memerlukan identitas jaringan tetap dan penyimpanan persisten.

Jika aplikasi kamu tidak memerlukan penyimpanan data yang tetap atau identitas unik untuk setiap pod, maka **Deployment** adalah pilihan terbaik.
