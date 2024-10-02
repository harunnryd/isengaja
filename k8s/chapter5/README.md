# Labels di Kubernetes 🏷️

Dalam Kubernetes, Label adalah salah satu fitur yang sangat berguna untuk mengorganisir dan mengelompokkan resource (seperti pod, service, dan node) berdasarkan kriteria tertentu. Dengan label, kita bisa lebih mudah mengelola, mencari, dan memanipulasi resource di dalam cluster. Bayangkan label seperti tag yang bisa ditempel di resource kamu untuk memberikan informasi tambahan.

## Apa Itu Label? 🤔
Label adalah key-value pair yang ditempelkan ke resource Kubernetes (seperti pod, service, node, dll). Label ini memungkinkan kamu untuk mengidentifikasi dan mengelompokkan resource berdasarkan atribut tertentu yang kamu tentukan sendiri. Misalnya, kamu bisa memberi label berdasarkan environment (dev, staging, production), version (v1, v2), atau app type (frontend, backend).

## Contoh Label:
```yaml
metadata:
  labels:
    app: nginx
    env: production
    version: v1
```

Dalam contoh ini, ada tiga label yang diterapkan ke resource:

- app: nginx: Mengidentifikasi bahwa resource ini adalah aplikasi nginx.
- env: production: Menandakan bahwa resource ini untuk lingkungan production.
- version: v1: Menyatakan bahwa resource ini versi v1.

## Kapan Label Dipakai?
- Memudahkan pengelompokkan resource.
- Filter resource tertentu dengan query menggunakan perintah kubectl.
- Membuat seleksi pod untuk Service atau ReplicationController.
- Menggunakan label selector dalam Deployment untuk menentukan pod mana yang akan di-manage.

## Cara Menambahkan Label 🏷️
Label bisa ditambahkan ke berbagai resource seperti Pod, Service, dan Deployment. Kamu bisa menambahkannya langsung saat membuat resource, atau setelah resource dibuat dengan menggunakan command kubectl.

## Menambahkan Label Lewat YAML
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
    env: production
spec:
  containers:
  - name: nginx-container
    image: nginx:latest
```

Di sini, pod diberi label app: nginx dan env: production. Jadi, pod ini bisa dengan mudah difilter atau diidentifikasi berdasarkan label tersebut.

## Menambahkan Label Setelah Resource Dibuat
Kalau kamu lupa menambahkan label saat membuat resource, kamu bisa pakai perintah ini:
```bash
kubectl label pod nginx-pod version=v1
```
Dengan command di atas, kamu menambahkan label version=v1 ke pod yang sudah ada bernama nginx-pod.

## Cara Melihat Label 📊
Untuk melihat label yang diterapkan ke suatu resource, kamu bisa menggunakan perintah kubectl:

## Melihat Label di Pod
```bash
kubectl get pods --show-labels
```

## Output Contoh:
```bash
NAME         READY   STATUS    RESTARTS   AGE   LABELS
nginx-pod    1/1     Running   0          5m    app=nginx,env=production,version=v1
```
Dari output di atas, kamu bisa lihat bahwa pod nginx-pod punya tiga label: app=nginx, env=production, dan version=v1.

## Menggunakan Label Selector 🕵️‍♂️
Label selector digunakan untuk memilih resource tertentu yang punya label spesifik. Ini berguna kalau kamu ingin mengoperasikan subset dari resource yang punya label tertentu.

## Contoh Selector:
Untuk memilih semua pod yang punya label env=production, kamu bisa pakai command ini:
```bash
kubectl get pods -l env=production
```

## Output:
```bash
NAME         READY   STATUS    RESTARTS   AGE
nginx-pod    1/1     Running   0          5m
```

## Selector dengan Beberapa Label
Kamu juga bisa membuat filter dengan beberapa label sekaligus. Misalnya, untuk memilih pod dengan label app=nginx dan version=v1, gunakan:
```bash
kubectl get pods -l app=nginx,version=v1
```

## Label pada Service 🔄
Salah satu penggunaan label yang paling sering adalah pada Service. Service di Kubernetes menggunakan label selector untuk menentukan pod mana yang harus di-load balance.

## Contoh Service dengan Label Selector:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
    env: production
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
```
Dalam contoh ini, service `nginx-service` hanya akan menargetkan pod-pod yang punya label app=nginx dan env=production. Ini membuat load balancing jadi lebih spesifik.
