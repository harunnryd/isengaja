# Annotations di Kubernetes 📝

Selain Label, Kubernetes juga punya fitur lain yang disebut Annotations. Kalau label digunakan buat mengelompokkan dan mengatur resource berdasarkan key-value yang sederhana, annotation lebih fleksibel dan biasanya digunakan buat menyimpan metadata tambahan yang mungkin nggak diperlukan untuk filtering atau seleksi seperti pada label.

## Apa Itu Annotation? 🤔
Annotation adalah key-value pair (sama seperti label) yang bisa ditambahkan ke resource di Kubernetes. Bedanya dengan label, annotation biasanya digunakan buat menyimpan informasi tambahan atau metadata yang mungkin penting buat tools atau plugin lain, tapi nggak terlalu berguna buat operasional di dalam Kubernetes sendiri.

## Annotation bisa dipakai buat menyimpan apa saja, misalnya:

- URL dari dokumen terkait resource tersebut.
- Hash dari konfigurasi deployment.
- Informasi debugging atau audit log.

## Contoh Annotation:
```yaml
metadata:
  annotations:
    description: "This pod runs the Nginx web server"
    documentation-url: "https://example.com/docs/nginx"
```

Di contoh ini, pod diberikan annotation berupa deskripsi singkat dan URL ke dokumentasi terkait pod tersebut. Annotations ini nggak akan mempengaruhi cara Kubernetes bekerja, tapi bisa digunakan oleh tim atau tools eksternal.

## Kapan Annotation Dipakai? 📌
Annotations sering digunakan ketika kamu ingin menyimpan informasi tambahan yang:

- Tidak akan digunakan buat filter atau seleksi (beda dengan label).
- Ingin menyimpan metadata yang bisa dibaca oleh manusia atau sistem eksternal.
- Informasi yang mungkin butuh lebih dari sekadar key-value sederhana.

## Beberapa contoh penggunaan annotation di dunia nyata:

- Menyimpan informasi build atau versioning dari CI/CD pipeline.
- Menyimpan metadata dari monitoring tools seperti Prometheus atau Datadog.
- Menyimpan link ke dokumentasi atau kontak support.

## Menambahkan Annotation ke Resource 🛠️
Annotation bisa ditambahkan saat resource pertama kali dibuat, atau bisa juga ditambahkan ke resource yang sudah ada, sama seperti label.

## Menambahkan Annotation Lewat YAML:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  annotations:
    description: "This is a test pod running nginx"
    created-by: "Harun"
spec:
  containers:
  - name: nginx-container
    image: nginx:latest
    ports:
    - containerPort: 80
    resources:
      limits:
        memory: "128Mi"
        cpu: "500m"
```

Di YAML ini, ada dua annotation ditambahkan ke pod: description dan created-by. Annotations ini hanya berfungsi sebagai metadata tambahan dan tidak mempengaruhi cara pod berjalan.

## Menambahkan Annotation ke Resource yang Sudah Ada:
Kamu juga bisa menambahkan annotation ke resource yang sudah ada menggunakan perintah kubectl:
```bash
kubectl annotate pod nginx-pod created-by="Harun"
```
Command ini akan menambahkan annotation created-by ke pod `nginx-pod`.

## Melihat Annotation 📊
Untuk melihat annotation yang ada pada resource, kamu bisa gunakan perintah berikut:

## Melihat Annotation di Pod:
```bash
kubectl describe pod nginx-pod
```

## Output Contoh:
```bash
Name:         nginx-pod
Annotations:  description: This is a test pod running nginx
              created-by: Harun
```

Annotation akan terlihat di bagian Annotations ketika kamu mengecek detail resource.

## Annotation vs Label ⚖️
Meskipun annotation dan label sama-sama berupa key-value pair, penggunaannya sangat berbeda:

- Label digunakan buat identifikasi dan seleksi resource (misalnya buat filtering atau load balancing).
- Annotation digunakan buat menambahkan informasi tambahan atau metadata, tanpa tujuan seleksi atau operasional.

## Kapan Gunakan Label vs Annotation?
- Pakai Label kalau kamu butuh mengelompokkan atau memfilter resource berdasarkan key tertentu.
- Pakai Annotation kalau kamu hanya butuh menyimpan informasi tambahan yang nggak akan dipakai Kubernetes untuk filtering atau grouping.

## Contoh Penggunaan Annotation di Dunia Nyata 🌍
Beberapa skenario nyata di mana annotation sering digunakan:

1. Monitoring: Misalnya kamu bisa menambahkan annotation dengan detail monitoring spesifik yang bisa dibaca oleh tools seperti Prometheus atau Grafana.
2. Audit Log: Annotations bisa digunakan buat mencatat siapa yang membuat atau mengubah resource, sehingga kamu bisa punya jejak audit.
3. CI/CD Pipelines: Informasi seperti commit SHA, build number, atau link ke hasil test bisa disimpan sebagai annotation di resource deployment.

