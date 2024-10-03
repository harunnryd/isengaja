
# Computational Resources, Tolerations, dan Node Affinity di Kubernetes 🖥️⚙️

Di Kubernetes, kamu bisa mengatur bagaimana pod menggunakan sumber daya komputasi seperti **CPU** dan **memory**. Selain itu, Kubernetes juga mendukung mekanisme untuk mengontrol di node mana pod akan dijalankan dengan menggunakan **Tolerations** dan **Node Affinity**. 

## Computational Resources 🤖

**Computational Resources** memungkinkan kamu menentukan batasan dan request untuk penggunaan **CPU** dan **memory** oleh pod. Request memastikan berapa banyak sumber daya minimum yang harus disediakan untuk pod, sementara limit adalah batas maksimum yang dapat digunakan oleh pod.

### Contoh YAML Penggunaan Resources:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: resource-pod
spec:
  containers:
  - name: busybox
    image: busybox
    command: ["sleep", "3600"]
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
```

### Penjelasan:
- **requests**: Sumber daya minimum yang dibutuhkan pod. Dalam contoh ini, pod meminta 64Mi memory dan 250m CPU (milliCPU).
- **limits**: Batas maksimum sumber daya yang dapat digunakan. Pod dibatasi menggunakan 128Mi memory dan 500m CPU.

---

## Tolerations 🛑

**Tolerations** memungkinkan pod untuk "menoleransi" node yang memiliki **taints** tertentu. **Taints** digunakan untuk menandai node dengan kondisi khusus, dan pod hanya bisa dijalankan di node tersebut jika memiliki toleration yang sesuai.

### Contoh YAML Toleration:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: toleration-pod
spec:
  containers:
  - name: nginx
    image: nginx
  tolerations:
  - key: "example-key"
    operator: "Equal"
    value: "example-value"
    effect: "NoSchedule"
```

### Penjelasan:
- **tolerations**: Pod ini akan menoleransi node yang memiliki taint dengan **key: example-key** dan **value: example-value**. Dengan toleration ini, pod dapat dijalankan di node yang memiliki taint tersebut.

---

## Node Affinity 🧲

**Node Affinity** adalah mekanisme yang memungkinkan kamu menentukan preferensi atau persyaratan tentang di node mana pod harus dijalankan. Ini seperti "label-based scheduling" di mana pod akan dijalankan pada node yang memiliki label tertentu.

### Contoh YAML Node Affinity:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: affinity-pod
spec:
  containers:
  - name: nginx
    image: nginx
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: kubernetes.io/e2e-az-name
            operator: In
            values:
            - e2e-az1
            - e2e-az2
```

### Penjelasan:
- **requiredDuringSchedulingIgnoredDuringExecution**: Pod hanya bisa dijalankan di node yang memiliki label **kubernetes.io/e2e-az-name** dengan nilai **e2e-az1** atau **e2e-az2**.
- **nodeSelectorTerms**: Mencari node yang memiliki label yang sesuai dengan kriteria ini.

---

## Mengelola Computational Resources, Tolerations, dan Node Affinity 🧰

### Melihat Sumber Daya yang Digunakan Pod:
Untuk melihat sumber daya yang digunakan oleh pod, kamu bisa menggunakan perintah berikut:

```bash
kubectl top pod <pod-name>
```

### Melihat Node yang Dapat Ditoleransi Pod:
Untuk melihat node yang memiliki taint dan toleration yang relevan, gunakan perintah:

```bash
kubectl get nodes -o wide
kubectl describe node <node-name>
```

### Melihat Pod dengan Node Affinity:
Untuk melihat pod yang memiliki node affinity, gunakan perintah berikut:

```bash
kubectl get pods -o wide
```

---

## Menghapus Pod dengan Spesifikasi Toleration dan Affinity 🗑️

Jika kamu ingin menghapus pod yang menggunakan tolerations atau node affinity, gunakan perintah berikut:

```bash
kubectl delete pod <pod-name>
```
