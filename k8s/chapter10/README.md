# ReplicaSet di Kubernetes 📊
Di Kubernetes, ReplicaSet adalah komponen yang digunakan untuk memastikan sejumlah pod yang sama tetap berjalan pada satu waktu. ReplicaSet memastikan bahwa jika sebuah pod mati, Kubernetes akan membuat pod baru untuk menggantikannya, menjaga high availability aplikasi.

## Apa Itu ReplicaSet? 🤔
ReplicaSet adalah objek yang bertugas untuk menjaga agar jumlah replica pod yang berjalan di cluster selalu sesuai dengan jumlah yang diinginkan. Misalnya, jika kamu ingin selalu memiliki tiga pod yang menjalankan aplikasi web kamu, ReplicaSet akan memastikan hal tersebut.

## Fungsi Utama ReplicaSet:
- Auto-recovery: Jika ada pod yang mati, ReplicaSet akan otomatis membuat pod baru untuk menggantikannya.
- Scaling: ReplicaSet juga memungkinkan kamu melakukan scaling (menambah atau mengurangi) jumlah pod secara otomatis.
- Consistent State: ReplicaSet menjaga state aplikasi sesuai dengan jumlah pod yang ditentukan dalam spec.

## Cara Kerja ReplicaSet 🔄
### Pod Replacement:
Misalnya, jika ReplicaSet mendeteksi bahwa ada pod yang mati atau hilang, Kubernetes akan membuat pod baru dengan konfigurasi yang sama di node lain. Ini membantu menjaga uptime aplikasi, sehingga aplikasi tetap berjalan walaupun ada kegagalan di salah satu node.

### Scaling:
Dengan ReplicaSet, kamu bisa melakukan horizontal scaling dengan mudah. Kamu tinggal menambah atau mengurangi jumlah replica di konfigurasi ReplicaSet, dan Kubernetes akan membuat atau menghentikan pod sesuai dengan perubahan tersebut.

## Membuat ReplicaSet 🛠️
Berikut adalah contoh YAML file untuk membuat ReplicaSet yang menjaga tiga replica pod nginx selalu berjalan.

### Contoh YAML ReplicaSet:
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-replicaset
spec:
  replicas: 3
  selector:
    matchExpressions:
    - key: app
      operator: In
      values:
      - nginx
    - key: env
      operator: In
      values:
      - prod
      - stg
      - dev
  template:
    metadata:
      labels:
        app: nginx
        env: prod
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
```

### Penjelasan:
- replicas: Jumlah pod yang harus selalu dijaga oleh ReplicaSet. Dalam contoh ini, Kubernetes akan memastikan ada tiga pod yang berjalan.
- selector: Selector digunakan untuk mencocokkan pod mana yang akan dikelola oleh ReplicaSet. Di sini, pod dengan label app: nginx akan dikelola oleh ReplicaSet ini.
- template: Ini adalah template pod yang akan digunakan untuk membuat pod baru kalau ada pod yang mati. Setiap kali ReplicaSet butuh membuat pod baru, ia akan menggunakan template ini.

## Mengelola ReplicaSet 🧰
### Melihat Status ReplicaSet:
Setelah ReplicaSet dibuat, kamu bisa melihat statusnya menggunakan perintah ini:
```bash
kubectl get rs
```

### Output Contoh:
```bash
NAME               DESIRED   CURRENT   READY   AGE
nginx-replicaset   3         3         3       10m
```
### Penjelasan:

- DESIRED: Jumlah pod yang diinginkan oleh ReplicaSet.
- CURRENT: Jumlah pod yang saat ini berjalan.
- READY: Jumlah pod yang siap menerima traffic.

### Scaling ReplicaSet:
Kalau kamu ingin mengubah jumlah pod yang dijalankan oleh ReplicaSet, kamu bisa melakukannya dengan perintah berikut:

```bash
kubectl scale rs nginx-replicaset --replicas=5
```
Dengan perintah di atas, ReplicaSet akan diubah untuk menjaga lima pod berjalan.

### Menghapus ReplicaSet 🗑️
Kalau kamu ingin menghapus ReplicaSet, kamu bisa melakukannya dengan perintah:

```bash
kubectl delete rs nginx-replicaset
```
<b>Catatan</b>: Menghapus ReplicaSet tidak akan menghapus pod yang sudah dibuat oleh ReplicaSet. Jika kamu ingin menghapus pod juga, tambahkan flag <b>--cascade</b>:

```bash
kubectl delete rs nginx-replicaset --cascade=true
```

## ReplicaSet vs Deployment: Apa Bedanya? ⚖️
Sebenarnya, ReplicaSet jarang digunakan langsung, karena Kubernetes punya resource yang lebih powerful, yaitu Deployment. Deployment menggunakan ReplicaSet di belakang layar, tetapi memberikan lebih banyak fitur seperti rollout dan rollback.

Jadi, kapan harus menggunakan ReplicaSet? Biasanya, kamu akan lebih sering menggunakan Deployment, kecuali kamu punya use case spesifik di mana hanya ingin menjaga pod tetap berjalan tanpa fitur tambahan seperti deployment strategy.
