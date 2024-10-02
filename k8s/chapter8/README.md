# Menghapus Pod di Kubernetes 🗑️
Di Kubernetes, Pod adalah unit terkecil yang menjalankan container aplikasi. Namun, ada kalanya kamu perlu menghapus pod, entah karena aplikasi sudah tidak diperlukan, pod sedang mengalami error, atau kamu ingin membuat ulang pod dengan konfigurasi baru. Di chapter ini, kita akan bahas cara menghapus pod di Kubernetes dengan kubectl serta beberapa hal penting yang perlu kamu perhatikan.

## Cara Menghapus Pod 🛠️
Menghapus Pod Secara Langsung
Untuk menghapus pod, perintah yang paling umum digunakan adalah:

```bash
kubectl delete pod <pod-name>
```

Contoh:

```bash
kubectl delete pod nginx-pod
```

Dengan perintah ini, Kubernetes akan menghapus pod tersebut dari cluster. Kalau pod itu bagian dari Deployment atau ReplicaSet, pod yang baru akan otomatis dibuat untuk menggantikan pod yang hilang.

## Menghapus Pod di Namespace Tertentu:
Kalau kamu menggunakan namespace tertentu, kamu bisa menambahkan flag -n untuk menentukan namespace dari pod yang ingin dihapus:

```bash
kubectl delete pod <pod-name> -n <namespace>
```

Contoh:

```bash
kubectl delete pod nginx-pod -n dev-environment
```

Ini akan menghapus pod di namespace dev-environment.

## Menghapus Semua Pod Sekaligus 🧹
Jika kamu ingin menghapus semua pod yang ada di satu namespace, gunakan command ini:

```bash
kubectl delete pods --all -n <namespace>
```

Contoh:

```bash
kubectl delete pods --all -n dev-environment
```

Command ini akan menghapus semua pod yang ada di namespace dev-environment.

## Menghapus Pod dengan Force
Kadang, ada pod yang mungkin tidak mau terhapus karena sedang dalam status stuck atau crash. Kalau kamu ingin memaksa penghapusan pod, gunakan flag --force dan --grace-period=0:

```bash
kubectl delete pod <pod-name> --force --grace-period=0
```

Contoh:

```bash
kubectl delete pod nginx-pod --force --grace-period=0
```

Perintah ini akan langsung menghapus pod tanpa menunggu periode graceful termination.

## Memahami Proses Penghapusan Pod 🔄
Ketika kamu menghapus pod, proses yang terjadi adalah:

- Graceful Shutdown: Secara default, Kubernetes akan memberikan waktu ke container di dalam pod untuk shutdown dengan benar. Waktu ini disebut grace period, yang biasanya 30 detik. Kamu bisa mengubahnya dengan flag --grace-period.
- Pod dihapus dari Node: Setelah grace period habis, Kubernetes akan menghapus pod dari node.
- Re-scheduling (Jika Ada Deployment/ReplicaSet): Jika pod itu adalah bagian dari Deployment atau ReplicaSet, pod baru akan otomatis dibuat oleh Kubernetes untuk menggantikan pod yang dihapus, sehingga aplikasi tetap running sesuai dengan jumlah replika yang diinginkan.

## Kenapa Pod Sering Muncul Kembali Setelah Dihapus? 🤔
Jika kamu menghapus pod yang dikelola oleh Deployment, ReplicaSet, atau DaemonSet, Kubernetes akan otomatis membuat pod baru untuk menggantikan yang kamu hapus. Ini karena Kubernetes menganggap kamu ingin menjaga "state" aplikasi sesuai konfigurasi yang sudah diatur di Deployment.

Untuk menghentikan pod sepenuhnya, kamu harus menghapus Deployment atau ReplicaSet yang mengelola pod tersebut, seperti ini:

```bash
kubectl delete deployment <deployment-name>
```
