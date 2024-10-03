
# Volumes di Kubernetes 📦

**Volumes** di Kubernetes adalah cara untuk menyediakan penyimpanan persisten bagi pod yang bersifat sementara. Pod dan container bersifat ephemeral (tidak permanen), yang berarti data di dalamnya akan hilang jika pod dihentikan atau container diganti. **Volumes** digunakan untuk menyimpan data secara persisten agar tetap tersedia meskipun pod atau container berhenti berjalan.

## Apa Itu Volume? 🤔

Volume adalah resource di Kubernetes yang menyediakan penyimpanan yang dapat diakses oleh pod. Berbeda dengan penyimpanan lokal di container, data yang disimpan dalam **volume** tetap tersedia bahkan ketika pod berhenti atau diganti. Volumes memungkinkan kamu untuk menyimpan data aplikasi, file log, atau apa pun yang perlu dipertahankan di luar siklus hidup pod.

### Jenis-jenis Volume:
1. **emptyDir**: Volume sementara yang hanya ada selama pod berjalan.
2. **hostPath**: Menghubungkan direktori di node dengan pod.
3. **persistentVolume (PV)**: Volume permanen yang terpisah dari siklus hidup pod dan node, sering digunakan bersama **PersistentVolumeClaim (PVC)**.

---

## Membuat Volume 🛠️

Berikut adalah contoh YAML file untuk membuat volume **emptyDir** yang digunakan oleh container.

### Contoh YAML Volume:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: volume-pod
spec:
  containers:
  - name: app-container
    image: busybox
    command: ["sleep", "3600"]
    volumeMounts:
    - name: app-storage
      mountPath: /usr/share/app-data
  volumes:
  - name: app-storage
    emptyDir: {}
```

### Penjelasan:
- **emptyDir**: Volume ini adalah direktori kosong yang dibuat ketika pod dimulai. Volume ini dihapus ketika pod dihentikan.
- **volumeMounts**: Menyatakan bahwa container akan menggunakan volume dan mengaitkannya di path **/usr/share/app-data**.

---

## Persistent Volume (PV) dan Persistent Volume Claim (PVC) 💾

Untuk penyimpanan yang lebih permanen, Kubernetes memiliki konsep **Persistent Volume (PV)** dan **Persistent Volume Claim (PVC)**. PV adalah volume yang dikelola oleh cluster, sedangkan PVC adalah cara bagi pod untuk meminta akses ke PV.

### Membuat Persistent Volume (PV) 🛠️

Berikut adalah contoh YAML file untuk membuat **Persistent Volume (PV)**.

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-pv
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/data"
```

### Penjelasan:
- **capacity**: Kapasitas penyimpanan yang tersedia pada volume, dalam contoh ini 1Gi.
- **accessModes**: Mode akses untuk volume. **ReadWriteOnce** berarti volume hanya bisa di-mount oleh satu pod dalam mode baca-tulis.
- **hostPath**: Path di node yang digunakan sebagai penyimpanan untuk volume.

---

### Membuat Persistent Volume Claim (PVC) 🛠️

Setelah PV dibuat, pod perlu menggunakan **Persistent Volume Claim (PVC)** untuk meminta akses ke volume tersebut. Berikut adalah contoh YAML file untuk PVC.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

### Penjelasan:
- **resources.requests.storage**: PVC meminta 1Gi storage dari PV yang tersedia di cluster.

---

## Menggunakan Persistent Volume di Pod 🎛️

Setelah PV dan PVC dibuat, pod bisa mengakses storage tersebut. Berikut adalah contoh bagaimana PVC digunakan dalam pod.

### Contoh YAML Pod Menggunakan PVC:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: app-pod
spec:
  containers:
  - name: app-container
    image: busybox
    command: ["sleep", "3600"]
    volumeMounts:
    - name: persistent-storage
      mountPath: /usr/share/app-data
  volumes:
  - name: persistent-storage
    persistentVolumeClaim:
      claimName: my-pvc
```

### Penjelasan:
- **persistentVolumeClaim**: Pod menggunakan **PVC** yang bernama **my-pvc** untuk mengakses **Persistent Volume**.

---

## Mengelola Volume 🧰

### Melihat Persistent Volume (PV):
Untuk melihat PV yang ada di cluster, kamu bisa menggunakan perintah:

```bash
kubectl get pv
```

### Output Contoh:
```bash
NAME     CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM            STORAGECLASS   AGE
my-pv    1Gi        RWO            Retain           Bound    default/my-pvc   standard       2h
```

### Melihat Persistent Volume Claim (PVC):
Untuk melihat PVC yang ada di cluster, gunakan perintah:

```bash
kubectl get pvc
```

### Output Contoh:
```bash
NAME      STATUS   VOLUME   CAPACITY   ACCESS MODES   STORAGECLASS   AGE
my-pvc    Bound    my-pv    1Gi        RWO            standard       2h
```

---

## Menghapus Volume 🗑️

### Menghapus Persistent Volume (PV):
```bash
kubectl delete pv <pv-name>
```

### Menghapus Persistent Volume Claim (PVC):
```bash
kubectl delete pvc <pvc-name>
```

**Catatan**: Menghapus PVC tidak selalu menghapus PV. Tergantung pada kebijakan **Reclaim Policy**, PV bisa **di-retain** atau **dihapus** ketika PVC dihapus.
