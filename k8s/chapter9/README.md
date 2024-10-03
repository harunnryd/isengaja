# Probes di Kubernetes 🔍

Probes di Kubernetes adalah cara untuk mengecek kesehatan atau status dari pod dan container yang berjalan. Mereka memungkinkan Kubernetes untuk mengetahui apakah container siap untuk menerima traffic atau apakah container berjalan dengan baik, atau perlu restart. Ada tiga jenis probe utama di Kubernetes: Liveness Probe, Readiness Probe, dan Startup Probe. Yuk, kita bahas satu per satu!

## Apa Itu Probe? 🤔
Probe adalah pemeriksaan berkala yang dilakukan oleh Kubernetes terhadap container di dalam pod untuk memastikan mereka berjalan dengan baik. Setiap probe bekerja dengan menjalankan pemeriksaan tertentu (misalnya HTTP request, command, atau TCP check) dan menentukan apakah container sehat atau tidak.

## Tiga Jenis Probe:
1. Liveness Probe: Digunakan untuk memeriksa apakah container masih hidup (sehat). Jika probe ini gagal, Kubernetes akan me-restart container.
2. Readiness Probe: Digunakan untuk memeriksa apakah container siap menerima traffic. Jika probe ini gagal, container akan ditandai sebagai not ready, dan traffic tidak akan dialihkan ke container ini.
3. Startup Probe: Digunakan untuk memeriksa apakah aplikasi di dalam container sudah siap untuk dimulai. Ini berguna kalau aplikasi butuh waktu lama untuk startup.

## Liveness Probe 💉
Liveness probe digunakan untuk memeriksa apakah container mengalami masalah. Kalau probe ini gagal, Kubernetes akan me-restart container, berharap masalahnya bisa teratasi setelah restart.

### Contoh Penggunaan:
Kamu bisa menggunakan liveness probe untuk aplikasi yang mungkin bisa crash atau mengalami deadlock (berhenti bekerja, tapi container tetap running).
```yaml
livenessProbe:
  httpGet:
    path: /healthz
    port: 8080
  initialDelaySeconds: 3
  periodSeconds: 3
```

## Penjelasan:
- httpGet: Memeriksa endpoint /healthz di port 8080 untuk melihat apakah container masih hidup.
- initialDelaySeconds: Menunggu 3 detik sebelum liveness probe pertama dijalankan, setelah container mulai.
- periodSeconds: Liveness probe dijalankan setiap 3 detik.

## Readiness Probe ✅
Readiness probe memeriksa apakah container siap untuk menerima traffic. Kalau container belum siap, Kubernetes tidak akan mengarahkan traffic ke sana. Ini berguna untuk aplikasi yang mungkin butuh waktu buat inisialisasi atau mempersiapkan dirinya sebelum menerima request.

### Contoh Penggunaan:
Readiness probe bisa dipakai untuk aplikasi yang mungkin perlu menunggu database siap atau menunggu resource lainnya sebelum menerima traffic.
```yaml
readinessProbe:
  exec:
    command:
      - "cat"
      - "/tmp/ready"
  initialDelaySeconds: 5
  periodSeconds: 5
```
### Penjelasan:
- exec: Menjalankan command cat /tmp/ready di dalam container. Jika file /tmp/ready ada, berarti container siap menerima traffic.
- initialDelaySeconds: Menunggu 5 detik sebelum readiness probe pertama dijalankan.
- periodSeconds: Readiness probe dijalankan setiap 5 detik.

## Startup Probe 🚀
Startup probe dipakai untuk aplikasi yang butuh waktu lama untuk memulai. Probe ini memastikan container sudah siap berjalan. Selama startup probe masih berlangsung, Kubernetes tidak akan menjalankan liveness dan readiness probe.

### Contoh Penggunaan:
Startup probe berguna untuk aplikasi yang butuh proses startup lama, seperti aplikasi monolitik atau aplikasi yang membutuhkan resource besar.
```yaml
startupProbe:
  httpGet:
    path: /healthz
    port: 8080
  failureThreshold: 30
  periodSeconds: 10
```
### Penjelasan:
- httpGet: Mengecek endpoint /healthz di port 8080 untuk memastikan aplikasi sudah siap berjalan.
- failureThreshold: Kubernetes akan mencoba 30 kali sebelum menganggap startup gagal.
- periodSeconds: Probe dijalankan setiap 10 detik.

## Menambahkan Probes ke Pod 🛠️
Probes di-declare dalam YAML file pada spesifikasi container di pod. Kamu bisa menggunakan salah satu probe, atau bahkan ketiganya (liveness, readiness, startup), tergantung kebutuhan aplikasi.

### Contoh YAML Lengkap dengan Probes:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
    resources:
      limits:
        memory: "128Mi"
        cpu: "500m"
    livenessProbe:
      httpGet:
        path: /healthz
        port: 8080
      initialDelaySeconds: 3
      periodSeconds: 3
    readinessProbe:
      httpGet:
        path: /readyz
        port: 8080
      initialDelaySeconds: 5
      periodSeconds: 5
    startupProbe:
      httpGet:
        path: /startup
        port: 8080
      failureThreshold: 30
      periodSeconds: 10
```
### Apa yang Terjadi Kalau Probe Gagal?
- Liveness probe gagal: Kubernetes akan me-restart container.
- Readiness probe gagal: Container ditandai sebagai not ready, dan traffic tidak akan dialihkan ke container tersebut sampai probe berhasil.
- Startup probe gagal: Container dianggap gagal startup, dan Kubernetes akan menghentikan upaya menjalankan container.
