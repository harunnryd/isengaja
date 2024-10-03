
# Ingress di Kubernetes 🌐

**Ingress** di Kubernetes adalah resource yang memungkinkan kamu mengatur **routing** HTTP dan HTTPS ke layanan yang berjalan di dalam cluster. Dengan **Ingress**, kamu bisa menentukan aturan bagaimana traffic dari luar cluster dialihkan ke **Service** yang ada di dalam cluster.

## Apa Itu Ingress? 🤔

Ingress bertindak sebagai **gateway** yang mengatur bagaimana permintaan HTTP/HTTPS dari luar cluster diarahkan ke aplikasi yang berjalan di dalam Kubernetes. Ingress memungkinkan kamu:
- **Menentukan URL** tertentu untuk mengarahkan traffic ke service.
- **Mendukung HTTPS** untuk mengamankan traffic dengan sertifikat SSL.
- **Load balancing** dan mengarahkan traffic ke banyak service.

Ingress memberikan cara yang lebih fleksibel dan kuat dibandingkan **Service** (NodePort atau LoadBalancer) untuk menangani traffic dari luar.

---

## Komponen Ingress 🛠️

1. **Ingress Controller**: Ini adalah komponen yang mengimplementasikan aturan yang didefinisikan di resource **Ingress**. Tanpa Ingress Controller, aturan Ingress tidak akan berfungsi. Ingress Controller ini harus di-deploy di cluster Kubernetes, contohnya:
   - **NGINX Ingress Controller**
   - **HAProxy Ingress Controller**
   - **Traefik Ingress Controller**

2. **Ingress Resource**: File YAML yang mendefinisikan aturan bagaimana traffic harus dialihkan, seperti domain mana yang mengarah ke service mana.

---

## Membuat Ingress 🛠️

Berikut adalah contoh YAML file untuk membuat Ingress yang mengarahkan traffic HTTP dari domain **example.com** ke service **nginx-service** di dalam cluster.

### Contoh YAML Ingress:
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
spec:
  rules:
  - host: "example.com"
    http:
      paths:
      - path: "/"
        pathType: Prefix
        backend:
          service:
            name: nginx-service
            port:
              number: 80
```

### Penjelasan:
- **host**: Menentukan domain mana yang ingin diarahkan, dalam contoh ini ke **example.com**.
- **paths**: Mendefinisikan URL mana yang akan dialihkan, dalam hal ini semua request ke **/** akan diarahkan ke **nginx-service**.
- **backend**: Mendefinisikan **service** yang akan menerima traffic, yaitu **nginx-service** di port 80.

---

## Mendukung HTTPS dengan Ingress 🔒

Untuk mendukung **HTTPS**, kamu bisa menambahkan **TLS** ke dalam Ingress, dan menyediakan sertifikat SSL untuk mengamankan traffic. Kamu bisa menggunakan **Cert-Manager** atau menyiapkan sertifikat SSL secara manual.

### Contoh Ingress dengan HTTPS:
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-https-ingress
spec:
  tls:
  - hosts:
    - "example.com"
    secretName: example-tls
  rules:
  - host: "example.com"
    http:
      paths:
      - path: "/"
        pathType: Prefix
        backend:
          service:
            name: nginx-service
            port:
              number: 80
```

### Penjelasan:
- **tls**: Bagian ini menambahkan konfigurasi TLS/SSL. **secretName** mengacu ke secret yang menyimpan sertifikat SSL.
- **hosts**: Domain yang menggunakan HTTPS.

---

## Mengelola Ingress 🧰

### Melihat Ingress:
Untuk melihat Ingress yang ada di dalam cluster, kamu bisa menggunakan perintah berikut:

```bash
kubectl get ingress
```

### Output Contoh:
```bash
NAME              CLASS    HOSTS           ADDRESS       PORTS   AGE
example-ingress   <none>   example.com     203.0.113.1   80      3h
```

### Melihat Detail Ingress:
Untuk melihat detail dari Ingress tertentu, gunakan perintah:

```bash
kubectl describe ingress <ingress-name>
```

---

## Ingress Controller: Kunci dari Ingress 🗝️

Ingat bahwa resource **Ingress** hanya akan berfungsi jika ada **Ingress Controller** yang berjalan di dalam cluster. Beberapa contoh Ingress Controller populer adalah:
1. **NGINX Ingress Controller**: Sangat populer dan banyak digunakan untuk berbagai kebutuhan.
2. **Traefik Ingress Controller**: Solusi yang lebih lightweight, sering digunakan di lingkungan kecil.
3. **HAProxy Ingress Controller**: Memiliki performa tinggi untuk beban kerja yang besar.

Ingress Controller bertanggung jawab untuk memproses aturan yang ada di resource Ingress dan menerapkan routing traffic sesuai dengan aturan tersebut.

---

## Menghapus Ingress 🗑️

Jika kamu ingin menghapus resource Ingress, gunakan perintah ini:

```bash
kubectl delete ingress <ingress-name>
```
