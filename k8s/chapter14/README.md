
# Service di Kubernetes 🛠️

**Service** di Kubernetes adalah komponen yang menghubungkan pod-pod dan menyediakan cara yang konsisten untuk mengakses aplikasi yang berjalan dalam cluster. Service bertindak sebagai load balancer internal yang mendistribusikan traffic ke pod-pod yang tepat, bahkan saat pod tersebut dimulai ulang atau dipindahkan.

## Apa Itu Service? 🤔

Pod di Kubernetes bersifat **dinamis** — mereka bisa mati, diganti, atau dipindahkan ke node lain kapan saja. Karena pod memiliki IP yang berbeda-beda, hal ini dapat menyulitkan akses ke aplikasi. **Service** hadir untuk mengatasi masalah ini dengan menyediakan **alamat IP virtual** yang stabil dan tetap, serta cara konsisten untuk berkomunikasi dengan pod.

### Tiga Jenis Utama Service di Kubernetes:
1. **ClusterIP**: Menghubungkan pod dalam cluster dan hanya dapat diakses dari dalam cluster (internal traffic).
2. **NodePort**: Mengizinkan akses dari luar cluster dengan membuka port pada node di cluster.
3. **LoadBalancer**: Menggunakan load balancer eksternal untuk mendistribusikan traffic dari luar ke dalam cluster.

---

## Cara Kerja Service 🔄

Service menggunakan label **selector** untuk menghubungkan pod-pod yang sesuai. Dengan cara ini, service dapat mendistribusikan traffic ke pod yang memiliki label yang sama, meskipun pod tersebut diganti atau dipindahkan. Service juga dapat bekerja sebagai **load balancer** internal untuk mendistribusikan request secara merata di antara pod.

---

## Membuat Service 🛠️

Berikut adalah contoh YAML file untuk membuat **ClusterIP Service** yang menghubungkan pod dengan aplikasi **nginx**.

### Contoh YAML Service:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: ClusterIP
```

### Penjelasan:
- **selector**: Service akan mencari semua pod yang memiliki label **app: nginx**.
- **port**: Port yang terbuka untuk service ini, dalam contoh ini port 80.
- **targetPort**: Port di dalam container pod yang akan menerima traffic.
- **type: ClusterIP**: Menyediakan IP internal yang hanya bisa diakses dalam cluster.

---

## Jenis-jenis Service di Kubernetes 🛤️

### 1. ClusterIP (Default) 📡
Service ini hanya dapat diakses dari dalam cluster. **ClusterIP** digunakan untuk menghubungkan pod satu sama lain tanpa membuka akses ke luar cluster.

### 2. NodePort 🌐
**NodePort** membuka akses ke luar cluster dengan cara membuka port pada setiap node di cluster. Ini membuat aplikasi dapat diakses dari luar menggunakan **IP node** dan **port yang dibuka**.

Contoh YAML untuk **NodePort**:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-nodeport
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 30007
  type: NodePort
```

### 3. LoadBalancer 📶
**LoadBalancer** digunakan untuk mengintegrasikan Kubernetes dengan load balancer eksternal (misalnya di cloud provider seperti AWS atau GCP). Service jenis ini membuat IP eksternal yang dapat diakses dari luar cluster, dan mendistribusikan traffic ke pod yang ada di dalam cluster.

Contoh YAML untuk **LoadBalancer**:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-loadbalancer
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: LoadBalancer
```

---

## Mengelola Service 🧰

### Melihat Daftar Service:
Untuk melihat semua service yang ada di cluster, kamu bisa menggunakan perintah berikut:

```bash
kubectl get services
```

### Output Contoh:
```bash
NAME                 TYPE           CLUSTER-IP      EXTERNAL-IP    PORT(S)        AGE
nginx-service        ClusterIP      10.96.0.1       <none>         80/TCP         5h
nginx-nodeport       NodePort       10.96.0.2       <none>         80:30007/TCP   2h
nginx-loadbalancer   LoadBalancer   10.96.0.3       203.0.113.1    80/TCP         1h
```

### Melihat Detail Service:
Jika kamu ingin melihat detail dari service tertentu, gunakan perintah:

```bash
kubectl describe service <service-name>
```

---

## Menghapus Service 🗑️
Jika kamu ingin menghapus service, gunakan perintah ini:

```bash
kubectl delete service <service-name>
```

