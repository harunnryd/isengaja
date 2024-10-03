
# ConfigMap dan Secret di Kubernetes 🔐

**ConfigMap** dan **Secret** adalah dua resource penting di Kubernetes yang digunakan untuk menyimpan konfigurasi dan data sensitif secara terpisah dari pod dan container. Keduanya memungkinkan kamu untuk mengelola konfigurasi aplikasi dengan cara yang lebih aman, dinamis, dan fleksibel.

## Apa Itu ConfigMap? 🤔

**ConfigMap** adalah resource di Kubernetes yang digunakan untuk menyimpan **konfigurasi non-sensitif** dalam bentuk pasangan **key-value**. ConfigMap memungkinkan kamu untuk mendeklarasikan konfigurasi aplikasi, seperti variabel environment, file konfigurasi, atau argumen command-line.

### Contoh Penggunaan ConfigMap:
1. Menyimpan URL database atau API endpoint.
2. Menyimpan konfigurasi aplikasi dalam bentuk file, seperti `.ini`, `.json`, atau `.yaml`.

---

## Membuat ConfigMap 🛠️

Berikut adalah contoh YAML file untuk membuat ConfigMap yang menyimpan konfigurasi aplikasi sederhana.

### Contoh YAML ConfigMap:
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  database_url: "postgres://db.example.com:5432/mydb"
  redis_url: "redis://cache.example.com:6379"
```

### Penjelasan:
- **data**: Di sinilah konfigurasi disimpan dalam bentuk pasangan **key-value**.
- **database_url** dan **redis_url** adalah contoh konfigurasi URL yang bisa diakses oleh pod.

---

## Menggunakan ConfigMap di Pod 🎛️

Setelah ConfigMap dibuat, kamu bisa menggunakannya di pod sebagai **environment variables** atau **volume**. Berikut adalah contoh bagaimana mengakses ConfigMap sebagai environment variable.

### Contoh YAML Pod Menggunakan ConfigMap:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: app-pod
spec:
  containers:
  - name: app-container
    image: myapp:latest
    env:
    - name: DATABASE_URL
      valueFrom:
        configMapKeyRef:
          name: app-config
          key: database_url
```

### Penjelasan:
- **env**: Environment variable untuk pod.
- **valueFrom**: Mengambil nilai dari **ConfigMap** yang telah kita buat, menggunakan key **database_url**.

---

## Apa Itu Secret? 🔒

**Secret** adalah resource di Kubernetes yang digunakan untuk menyimpan **data sensitif**, seperti password, token, atau kunci API, dalam bentuk yang lebih aman. Secret biasanya di-encode dalam **base64** dan dapat dienkripsi di backend Kubernetes untuk menjaga kerahasiaan data.

### Contoh Penggunaan Secret:
1. Menyimpan password database.
2. Menyimpan token akses API.
3. Menyimpan sertifikat SSL atau kunci enkripsi.

---

## Membuat Secret 🛠️

Berikut adalah contoh YAML file untuk membuat Secret yang menyimpan password database.

### Contoh YAML Secret:
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
data:
  db_password: cGFzc3dvcmQ=  # "password" dalam base64
```

### Penjelasan:
- **data**: Di sinilah secret disimpan, dalam bentuk yang sudah di-encode dengan **base64**. Kamu bisa mengubah string ke base64 dengan perintah:
  ```bash
  echo -n "password" | base64
  ```

---

## Menggunakan Secret di Pod 🛡️

Secret juga dapat digunakan di pod sebagai **environment variable** atau **volume**. Berikut adalah contoh bagaimana mengakses Secret sebagai environment variable.

### Contoh YAML Pod Menggunakan Secret:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: app-pod
spec:
  containers:
  - name: app-container
    image: myapp:latest
    env:
    - name: DB_PASSWORD
      valueFrom:
        secretKeyRef:
          name: db-secret
          key: db_password
```

### Penjelasan:
- **secretKeyRef**: Mengacu pada **Secret** bernama **db-secret** dan mengambil nilai dari key **db_password**.

---

## Melihat dan Mengelola ConfigMap dan Secret 🧰

### Melihat ConfigMap:
Untuk melihat ConfigMap yang ada di cluster, gunakan perintah berikut:

```bash
kubectl get configmaps
```

Untuk melihat isi dari ConfigMap, gunakan perintah:

```bash
kubectl describe configmap <configmap-name>
```

### Melihat Secret:
Untuk melihat Secret yang ada di cluster, gunakan perintah:

```bash
kubectl get secrets
```

Untuk melihat detail Secret, gunakan perintah:

```bash
kubectl describe secret <secret-name>
```

**Catatan**: Nilai dari Secret akan di-encode dalam **base64**, dan kamu harus mendecode-nya untuk melihat nilai aslinya.

---

## Menghapus ConfigMap dan Secret 🗑️

### Menghapus ConfigMap:
```bash
kubectl delete configmap <configmap-name>
```

### Menghapus Secret:
```bash
kubectl delete secret <secret-name>
```
