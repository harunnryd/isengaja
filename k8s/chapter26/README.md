
# Monitoring di Kubernetes dengan Prometheus dan Grafana 📊

**Monitoring** adalah bagian penting dari pengelolaan aplikasi yang berjalan di Kubernetes. Dengan monitoring, kamu bisa memantau kinerja aplikasi, penggunaan sumber daya, dan mendeteksi masalah lebih awal. Dua alat yang paling umum digunakan untuk monitoring di Kubernetes adalah **Prometheus** dan **Grafana**.

## Mengapa Monitoring di Kubernetes Penting? 🤔

1. **Mendeteksi Masalah**: Monitoring membantu mengidentifikasi masalah, seperti bottleneck sumber daya atau kegagalan pod, sebelum menyebabkan kerusakan lebih besar.
2. **Optimasi Kinerja**: Kamu bisa mengoptimalkan penggunaan sumber daya seperti CPU dan memory dengan memantau metrik secara real-time.
3. **Visibilitas**: Memberikan visibilitas penuh terhadap kesehatan aplikasi dan cluster Kubernetes.

---

## Apa Itu Prometheus dan Grafana? 🛠️

- **Prometheus** adalah sistem monitoring dan alerting berbasis time-series. Di Kubernetes, Prometheus digunakan untuk mengumpulkan metrik dari node, pod, dan aplikasi di dalam cluster.
- **Grafana** adalah platform analitik yang digunakan untuk memvisualisasikan data dari Prometheus dan alat monitoring lainnya. Dengan Grafana, kamu bisa membuat dashboard yang menarik dan intuitif untuk memantau cluster Kubernetes.

---

## Cara Kerja Prometheus di Kubernetes 🎛️

Prometheus bekerja dengan mengumpulkan data dari **endpoint** yang mengekspor metrik. Di Kubernetes, Prometheus mengumpulkan data dari aplikasi dan service melalui **kube-state-metrics**, **node-exporter**, dan metrik aplikasi lainnya.

Prometheus menggunakan **scrape** untuk mengumpulkan metrik pada interval tertentu. Data ini disimpan sebagai time-series dan bisa dianalisis untuk mendapatkan insight mengenai performa aplikasi.

---

## Menggunakan Prometheus di Kubernetes 🛠️

Berikut adalah langkah-langkah untuk menginstal **Prometheus** di Kubernetes menggunakan Helm.

### 1. Menambahkan Repository Helm Prometheus:
```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

### 2. Menginstal Prometheus:
```bash
helm install prometheus prometheus-community/prometheus
```

Setelah instalasi selesai, Prometheus akan mulai mengumpulkan metrik dari Kubernetes.

### Melihat Dashboard Prometheus:
Untuk mengakses dashboard Prometheus, kamu bisa meneruskan port lokal ke service Prometheus:
```bash
kubectl port-forward deploy/prometheus-server 9090
```
Buka browser dan navigasikan ke `http://localhost:9090` untuk melihat dashboard Prometheus.

---

## Menggunakan Grafana untuk Visualisasi 📊

Setelah Prometheus diinstal dan mengumpulkan metrik, kamu bisa menggunakan **Grafana** untuk memvisualisasikan data tersebut dalam bentuk dashboard.

### 1. Menginstal Grafana:
```bash
helm install grafana grafana/grafana
```

### 2. Menghubungkan Grafana dengan Prometheus:
- Setelah Grafana terinstal, buka dashboard Grafana dengan port-forwarding:
  ```bash
  kubectl port-forward service/grafana 3000:80
  ```
- Masuk ke Grafana melalui `http://localhost:3000` dengan username dan password default (`admin`/`admin`).
- Tambahkan **data source** dengan memilih Prometheus dan masukkan URL Prometheus (`http://prometheus-server:9090`).

---

## Membuat Dashboard di Grafana 📊

Grafana memungkinkan kamu untuk membuat custom dashboard berdasarkan metrik yang dikumpulkan oleh Prometheus. Berikut adalah beberapa metrik Kubernetes yang umum ditampilkan di dashboard Grafana:
1. **Usage CPU dan Memory per Pod**.
2. **Jumlah Pod yang Berjalan di Cluster**.
3. **Kesehatan Node**.
4. **Status Deployment dan DaemonSet**.

Grafana menyediakan berbagai template dashboard yang siap pakai untuk Kubernetes dan Prometheus, sehingga kamu bisa dengan cepat mengatur monitoring yang sesuai.

---

## Alerting di Prometheus 🔔

Selain memantau metrik, Prometheus juga bisa dikonfigurasi untuk memberikan **alert** jika terjadi anomali atau masalah pada aplikasi. Contohnya, kamu bisa mengatur alert jika:
- **Penggunaan CPU** melampaui batas tertentu.
- **Pod** gagal dijalankan setelah beberapa percobaan.
- **Node** mengalami penurunan kinerja atau terputus dari cluster.

Untuk mengkonfigurasi alert, kamu perlu mengedit **Prometheus Rule** di file konfigurasi Prometheus.

### Contoh Konfigurasi Alert:
```yaml
groups:
  - name: instance-rules
    rules:
    - alert: HighCPUUsage
      expr: sum(rate(container_cpu_usage_seconds_total[1m])) > 0.85
      for: 2m
      labels:
        severity: critical
      annotations:
        summary: "Penggunaan CPU tinggi pada pod!"
```

Alert ini akan mengirimkan peringatan jika penggunaan CPU melebihi 85% selama lebih dari 2 menit.
