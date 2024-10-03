
# Chapter 23: Network Policies di Kubernetes 🌐🔒

**Network Policies** di Kubernetes memungkinkan kamu untuk mengontrol lalu lintas jaringan ke dan dari pod di dalam cluster. Dengan Network Policies, kamu bisa mengatur aturan keamanan jaringan untuk memastikan bahwa hanya pod atau service yang diizinkan dapat berkomunikasi dengan pod-pod tertentu.

## Apa Itu Network Policy? 🤔

**Network Policy** adalah resource di Kubernetes yang mendefinisikan aturan lalu lintas jaringan pada level pod. Dengan Network Policies, kamu bisa mengontrol siapa yang bisa berkomunikasi dengan pod kamu, baik dari dalam cluster maupun dari luar.

Network Policies memungkinkan kamu mengatur:
1. Lalu lintas **masuk** ke pod.
2. Lalu lintas **keluar** dari pod.

---

## Mengapa Menggunakan Network Policies? 🔐

1. **Keamanan Jaringan**: Memastikan bahwa pod hanya berkomunikasi dengan service atau pod yang diizinkan.
2. **Isolasi**: Memisahkan dan melindungi pod sensitif dari akses yang tidak diinginkan.
3. **Kontrol Traffic**: Mengatur dan mengoptimalkan bagaimana pod berkomunikasi satu sama lain di dalam cluster.

---

## Membuat Network Policy 🛠️

Berikut adalah contoh YAML file untuk membuat **Network Policy** yang mengizinkan lalu lintas HTTP dari pod dengan label tertentu.

### Contoh YAML Network Policy:
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-http
spec:
  podSelector:
    matchLabels:
      app: my-app
  policyTypes:
  - Ingress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          role: frontend
    ports:
    - protocol: TCP
      port: 80
```

### Penjelasan:
- **podSelector**: Menentukan pod mana yang dikenakan aturan ini, dalam contoh ini, pod dengan label **app: my-app**.
- **policyTypes**: Menentukan jenis policy, dalam hal ini **Ingress** (lalu lintas masuk).
- **from**: Mengizinkan lalu lintas dari pod dengan label **role: frontend**.
- **ports**: Mengizinkan lalu lintas yang menggunakan **TCP** pada port **80** (HTTP).

---

## Isolasi Pod dengan Network Policy 🛡️

Secara default, pod di Kubernetes bisa saling berkomunikasi tanpa batasan. Namun, dengan Network Policies, kamu bisa mengisolasi pod tertentu agar hanya menerima lalu lintas dari pod atau IP tertentu.

Contoh di bawah ini memperlihatkan bagaimana mengisolasi pod sehingga hanya bisa menerima lalu lintas dari namespace tertentu.

### Contoh YAML Isolasi Pod:
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny
spec:
  podSelector: {}
  policyTypes:
  - Ingress
  ingress: []
```

### Penjelasan:
- **podSelector: {}**: Ini berarti aturan berlaku untuk semua pod di namespace yang sama.
- **ingress: []**: Tidak ada aturan ingress yang ditetapkan, sehingga pod diisolasi dan tidak menerima lalu lintas dari mana pun.

---

## Membatasi Egress (Lalu Lintas Keluar) 🚦

Selain mengatur lalu lintas masuk, Network Policies juga bisa digunakan untuk membatasi lalu lintas keluar dari pod.

### Contoh YAML Membatasi Egress:
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: restrict-egress
spec:
  podSelector:
    matchLabels:
      app: my-app
  policyTypes:
  - Egress
  egress:
  - to:
    - ipBlock:
        cidr: 192.168.0.0/16
    ports:
    - protocol: TCP
      port: 443
```

### Penjelasan:
- **egress**: Mengatur lalu lintas keluar dari pod.
- **ipBlock**: Mengizinkan lalu lintas ke IP di rentang **192.168.0.0/16**.
- **port 443**: Mengizinkan koneksi HTTPS (port 443).

---

## Mengelola Network Policies 🧰

### Melihat Network Policies:
Untuk melihat Network Policies yang ada di cluster, gunakan perintah berikut:

```bash
kubectl get networkpolicies
```

### Melihat Detail Network Policy:
Untuk melihat detail dari Network Policy tertentu, gunakan perintah:

```bash
kubectl describe networkpolicy <policy-name>
```

---

## Menghapus Network Policy 🗑️

Jika kamu ingin menghapus Network Policy, gunakan perintah berikut:

```bash
kubectl delete networkpolicy <policy-name>
```
