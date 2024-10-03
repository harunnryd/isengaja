
# StatefulSet di Kubernetes 🏛️

**StatefulSet** di Kubernetes adalah controller yang dirancang untuk mengelola deployment dan scaling dari set pod yang memerlukan identitas tetap dan penyimpanan persisten. **StatefulSet** sangat berguna untuk aplikasi yang **stateful** (memerlukan state yang tetap), seperti database atau sistem distribusi yang menyimpan data.

## Apa Itu StatefulSet? 🤔

**StatefulSet** adalah resource di Kubernetes yang mengelola deployment dari pod yang membutuhkan identitas unik dan state yang tetap di seluruh cycle hidupnya. Berbeda dengan **Deployment** yang bertanggung jawab untuk pod stateless, StatefulSet digunakan untuk aplikasi yang memerlukan **stable network identity** dan **persistent storage**.

### Fitur StatefulSet:
1. **Stable, Unique Network Identifiers**: Setiap pod dalam StatefulSet mendapatkan hostname yang unik berdasarkan urutan.
2. **Ordered Deployment and Scaling**: Pod di-deploy dan di-scale secara berurutan (misalnya, pod-0, pod-1, pod-2).
3. **Persistent Storage**: Setiap pod bisa memiliki volume yang persisten, sehingga data tetap ada meskipun pod dihapus atau di-restart.

---

## Use Case StatefulSet 🛠️

StatefulSet cocok digunakan untuk aplikasi-aplikasi seperti:
- **Database** seperti MySQL, PostgreSQL, Cassandra.
- **Sistem Distribusi** seperti Apache Kafka, Zookeeper.
- **Sistem Cache** seperti Redis dan memcached.

Aplikasi-aplikasi ini membutuhkan identitas jaringan yang stabil dan penyimpanan data yang persisten selama lifecycle pod.

---

## Membuat StatefulSet 🛠️

Berikut adalah contoh YAML file untuk membuat StatefulSet dengan aplikasi Redis, yang memerlukan penyimpanan persisten.

### Contoh YAML StatefulSet:
```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: redis
spec:
  serviceName: "redis"
  replicas: 3
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
      - name: redis
        image: redis:latest
        ports:
        - containerPort: 6379
        volumeMounts:
        - name: redis-storage
          mountPath: /data
  volumeClaimTemplates:
  - metadata:
      name: redis-storage
    spec:
      accessModes: [ "ReadWriteOnce" ]
      resources:
        requests:
          storage: 1Gi
```

### Penjelasan:
- **replicas**: Menentukan jumlah pod yang akan di-deploy (dalam contoh ini, 3 pod).
- **serviceName**: Menghubungkan StatefulSet dengan service untuk mengarahkan traffic.
- **volumeClaimTemplates**: Setiap pod memiliki Persistent Volume yang terpisah untuk penyimpanan data.

---

## Network Identity dalam StatefulSet 🌐

Setiap pod yang dibuat oleh StatefulSet memiliki **hostname** yang stabil dan unik, mengikuti format: `<statefulset-name>-<ordinal>`. Sebagai contoh, pod pertama dalam StatefulSet bernama `redis` akan memiliki hostname **redis-0**.

Dengan menggunakan network identity yang stabil ini, aplikasi seperti database bisa menyimpan dan mengakses data dengan konsisten berdasarkan identitas mereka.

---

## Scaling dalam StatefulSet 📈

StatefulSet memungkinkan scaling baik secara vertikal (memperbesar kapasitas) maupun horizontal (menambah jumlah pod). Saat pod baru ditambahkan dalam StatefulSet, pod tersebut akan dibuat secara berurutan (misalnya **redis-3** setelah **redis-2**).

Untuk scaling StatefulSet, kamu bisa menggunakan perintah berikut:

```bash
kubectl scale statefulset redis --replicas=5
```

Ini akan menambah dua pod baru **redis-3** dan **redis-4**.

---

## Mengelola StatefulSet 🧰

### Melihat StatefulSet:
Untuk melihat StatefulSet yang sedang berjalan di cluster, gunakan perintah:

```bash
kubectl get statefulsets
```

### Output Contoh:
```bash
NAME    READY   AGE
redis   3/3     2h
```

### Melihat Pod dalam StatefulSet:
Pod dalam StatefulSet memiliki identitas yang unik. Kamu bisa melihat pod-pod tersebut dengan perintah:

```bash
kubectl get pods -l app=redis
```

### Output Contoh:
```bash
NAME      READY   STATUS    RESTARTS   AGE
redis-0   1/1     Running   0          2h
redis-1   1/1     Running   0          2h
redis-2   1/1     Running   0          2h
```

---

## Menghapus StatefulSet 🗑️

Ketika kamu menghapus StatefulSet, pod-pod yang terkait dengan StatefulSet akan dihapus, tetapi Persistent Volume Claim (PVC) tidak akan dihapus secara otomatis. Ini memastikan bahwa data tetap tersedia meskipun StatefulSet dihapus.

Untuk menghapus StatefulSet, gunakan perintah berikut:

```bash
kubectl delete statefulset <statefulset-name>
```

Jika kamu ingin menghapus StatefulSet beserta PVC-nya, kamu perlu menghapus PVC secara manual:

```bash
kubectl delete pvc <pvc-name>
```

---

## StatefulSet vs Deployment: Apa Bedanya? ⚖️

- **StatefulSet**: Digunakan untuk aplikasi **stateful** yang membutuhkan identitas jaringan tetap dan penyimpanan persisten, seperti database.
- **Deployment**: Digunakan untuk aplikasi **stateless** yang tidak memerlukan penyimpanan atau identitas unik, seperti web server atau API.

Jika aplikasi kamu membutuhkan data yang tetap tersimpan di luar lifecycle pod dan memerlukan urutan deployment yang stabil, **StatefulSet** adalah pilihan terbaik.
