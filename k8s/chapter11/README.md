# DaemonSet di Kubernetes 📡
DaemonSet adalah komponen penting di Kubernetes yang memungkinkan kamu memastikan bahwa pod tertentu berjalan di setiap Node dalam cluster. DaemonSet sering digunakan untuk menjalankan tugas-tugas yang bersifat background atau yang perlu ada di setiap node, seperti logging, monitoring, atau network configuration.

## Apa Itu DaemonSet? 🤔
DaemonSet adalah sebuah resource di Kubernetes yang bertanggung jawab untuk memastikan bahwa pod-pod tertentu berjalan di setiap node yang ada di cluster. Berbeda dengan ReplicationSet atau Deployment yang menjaga sejumlah pod yang ditentukan, DaemonSet akan memastikan ada satu pod (atau lebih, tergantung konfigurasi) di setiap node yang tersedia.

### Contoh Penggunaan DaemonSet:
- Logging agent: Seperti Fluentd, untuk mengumpulkan log dari semua node.
- Monitoring agent: Seperti Prometheus Node Exporter untuk memonitor kesehatan node.
- Network services: Seperti menjalankan network plugin di setiap node untuk mengatur network overlay.

## Cara Kerja DaemonSet 🔄
DaemonSet bekerja dengan membuat pod di setiap node dalam cluster. Ketika node baru ditambahkan ke cluster, DaemonSet akan otomatis membuat pod di node tersebut. Begitu juga jika ada node yang dihapus, maka pod di node tersebut juga akan dihapus.

### Pod Scheduling:
- Setiap Node: Satu pod akan dijalankan di setiap node di cluster secara otomatis oleh DaemonSet.
- Specific Nodes: Kamu juga bisa mengkonfigurasi DaemonSet hanya untuk berjalan di node tertentu, misalnya menggunakan node selector atau tolerations.

## Membuat DaemonSet 🛠️
Berikut adalah contoh YAML file untuk membuat DaemonSet yang menjalankan Fluentd di setiap node.

### Contoh YAML DaemonSet:
```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: fluentd-daemonset
spec:
  selector:
    matchLabels:
      name: fluentd
  template:
    metadata:
      labels:
        name: fluentd
    spec:
      containers:
      - name: fluentd
        image: fluent/fluentd-kubernetes-daemonset:v1.16-debian-papertrail-2
        imagePullPolicy: Always
        resources:
          limits:
            memory: "200Mi"
            cpu: "0.5"
```

### Penjelasan:
- apiVersion: Versi API yang digunakan.
- kind: Menyatakan bahwa resource ini adalah DaemonSet.
- selector: DaemonSet menggunakan selector untuk menentukan pod mana yang harus dikelola.
- template: Template pod yang akan dibuat di setiap node. Dalam contoh ini, DaemonSet akan menjalankan Fluentd sebagai container di setiap node.

## Mengelola DaemonSet 🧰
### Melihat DaemonSet:
Setelah DaemonSet dibuat, kamu bisa melihat statusnya menggunakan perintah:

```bash
kubectl get daemonsets
```
### Output Contoh:
```bash
NAME                DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
fluentd-daemonset   3         3         3       3            3           <none>          5m
```
### Penjelasan:

- DESIRED: Jumlah pod yang seharusnya dijalankan (satu per node).
- CURRENT: Jumlah pod yang saat ini berjalan.
- READY: Jumlah pod yang sudah siap.
Menghapus DaemonSet:
Untuk menghapus DaemonSet, gunakan perintah ini:

```bash
kubectl delete daemonset fluentd-daemonset
```
Ini akan menghapus DaemonSet dan semua pod yang dikelola oleh DaemonSet dari setiap node di cluster.

## Spesifikasi Tambahan DaemonSet:
### Node Selector:
Kamu bisa mengonfigurasi DaemonSet untuk hanya berjalan di node tertentu menggunakan nodeSelector. Misalnya, jika kamu hanya ingin menjalankan pod di node dengan label env=production:

```yaml
spec:
  template:
    spec:
      nodeSelector:
        env: production
```
### Tolerations:
Jika kamu ingin menjalankan DaemonSet di node yang memiliki taints (misalnya, node khusus untuk workload tertentu), kamu bisa menggunakan tolerations di DaemonSet.

```yaml
spec:
  template:
    spec:
      tolerations:
      - key: "key1"
        operator: "Equal"
        value: "value1"
        effect: "NoSchedule"
```

## DaemonSet vs ReplicationSet: Apa Bedanya? ⚖️
Perbedaan utama antara DaemonSet dan ReplicationSet (atau Deployment) adalah pada distribution pod:

- ReplicationSet bertujuan untuk menjaga jumlah replica pod tertentu, terlepas di node mana pod itu berjalan.
- DaemonSet bertujuan untuk memastikan ada satu pod per node, menjadikannya ideal untuk layanan yang perlu ada di setiap node.
