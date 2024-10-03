
# Horizontal Pod Autoscaler (HPA) di Kubernetes 📈

**Horizontal Pod Autoscaler (HPA)** adalah fitur di Kubernetes yang secara otomatis melakukan scaling (menambah atau mengurangi jumlah pod) berdasarkan pemakaian sumber daya, seperti **CPU** atau **memory**. HPA memungkinkan aplikasi untuk menangani perubahan beban kerja secara dinamis dengan menyesuaikan jumlah pod yang berjalan.

## Apa Itu Horizontal Pod Autoscaler (HPA)? 🤔

**Horizontal Pod Autoscaler** adalah resource yang mengawasi metrik, seperti penggunaan CPU, dan secara otomatis melakukan scaling pada pod yang dikelola oleh **Deployment**, **ReplicaSet**, atau **StatefulSet**. HPA bisa digunakan untuk meningkatkan ketersediaan aplikasi di saat beban kerja meningkat, dan menghemat sumber daya saat beban berkurang.

### Kapan Menggunakan HPA?
1. Aplikasi yang memiliki beban kerja yang fluktuatif.
2. Untuk memastikan aplikasi tetap responsive selama peningkatan traffic.
3. Mengoptimalkan penggunaan sumber daya di cluster.

---

## Cara Kerja Horizontal Pod Autoscaler 🔄

HPA memantau penggunaan sumber daya dari pod-pod yang dikelola, misalnya penggunaan CPU atau memory. Berdasarkan target yang ditentukan, HPA akan menambah atau mengurangi jumlah pod agar aplikasi dapat menyesuaikan dengan beban kerja.

---

## Membuat Horizontal Pod Autoscaler 🛠️

Berikut adalah contoh YAML file untuk membuat HPA yang melakukan scaling pod berdasarkan penggunaan **CPU**.

### Contoh YAML HPA:
```yaml
apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
  name: nginx-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: nginx-deployment
  minReplicas: 2
  maxReplicas: 10
  targetCPUUtilizationPercentage: 50
```

### Penjelasan:
- **scaleTargetRef**: Menunjukkan resource mana yang akan di-scale, dalam hal ini **Deployment** bernama **nginx-deployment**.
- **minReplicas**: Jumlah pod minimum yang akan dijaga oleh HPA (dalam contoh ini, 2 pod).
- **maxReplicas**: Jumlah pod maksimum yang bisa di-scale oleh HPA (dalam contoh ini, 10 pod).
- **targetCPUUtilizationPercentage**: HPA akan menyesuaikan jumlah pod agar penggunaan CPU rata-rata tetap berada di sekitar 50%.

---

## Melihat HPA yang Berjalan 📊

Setelah HPA dibuat, kamu bisa melihat statusnya dan bagaimana ia melakukan scaling pada pod.

### Melihat Status HPA:
Untuk melihat status HPA dan metrik yang dipantau, gunakan perintah berikut:

```bash
kubectl get hpa
```

### Output Contoh:
```bash
NAME        REFERENCE                  TARGETS    MINPODS   MAXPODS   REPLICAS   AGE
nginx-hpa   Deployment/nginx-deploy    40% / 50%  2         10        4          1h
```

Perintah ini akan menampilkan penggunaan CPU saat ini dan jumlah pod yang di-scale oleh HPA.

---

## Mengubah Konfigurasi HPA 🛠️

Jika kamu ingin mengubah konfigurasi HPA, seperti target penggunaan CPU atau jumlah pod, kamu bisa mengedit HPA secara langsung menggunakan perintah:

```bash
kubectl edit hpa nginx-hpa
```

Ini akan membuka file YAML dari HPA di editor untuk diedit.

---

## Menghapus HPA 🗑️

Jika kamu ingin menghapus HPA, kamu bisa menggunakan perintah berikut:

```bash
kubectl delete hpa <hpa-name>
```

Ini akan menghapus Horizontal Pod Autoscaler dari cluster.
