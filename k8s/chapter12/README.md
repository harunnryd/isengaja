
# Job di Kubernetes 🎯

**Job** di Kubernetes adalah resource yang dirancang untuk menjalankan task atau pekerjaan yang hanya perlu dijalankan sekali atau dalam jumlah tertentu, berbeda dengan pod yang biasanya dijalankan terus-menerus. **Job** memastikan bahwa task tersebut selesai dengan sukses, dan Kubernetes akan membuat ulang pod jika terjadi kegagalan.

## Apa Itu Job? 🤔

**Job** digunakan untuk menjalankan task yang **finite** (berakhir), yaitu pekerjaan yang hanya perlu dilakukan satu kali, seperti menjalankan script backup, pengiriman email, atau proses batch. Job akan memonitor eksekusi task tersebut, dan memastikan pod yang menjalankannya **berhasil menyelesaikan** task tersebut.

### Jenis-jenis Job:
1. **Single-run Job**: Job yang hanya dijalankan satu kali sampai selesai.
2. **Parallel Jobs**: Job yang dijalankan dengan beberapa pod secara paralel, biasanya untuk memproses data atau melakukan pekerjaan batch dalam skala besar.

---

## Cara Kerja Job 🔄

Job akan membuat satu atau lebih pod untuk menjalankan task yang ditentukan. Jika pod tersebut **berhasil menyelesaikan** task-nya, maka job dianggap selesai. Namun, jika ada pod yang gagal, Kubernetes akan membuat ulang pod tersebut sampai task berhasil diselesaikan.

- **Single-run Job**: Jika satu pod cukup untuk menyelesaikan task, Kubernetes akan menjalankannya satu kali.
- **Parallel Job**: Jika task membutuhkan lebih banyak pod yang berjalan paralel (misalnya untuk task yang besar atau membutuhkan distribusi), kamu bisa menentukan jumlah **parallelism** untuk job.

---

## Membuat Job 🛠️

Berikut adalah contoh YAML file untuk membuat **single-run Job** yang menjalankan script sederhana.

### Contoh YAML Job:
```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: simple-job
spec:
  template:
    spec:
      containers:
      - name: simple-job-container
        image: busybox
        command: ["echo", "Hi harun ganteng"]
      restartPolicy: Never
```

### Penjelasan:
- **kind: Job**: Menyatakan bahwa resource ini adalah sebuah Job.
- **command**: Ini adalah perintah yang akan dijalankan oleh container di pod. Dalam contoh ini, command tersebut hanya mencetak pesan "Hello from the Kubernetes Job!".
- **restartPolicy: Never**: Pod tidak akan di-restart jika task sudah selesai.

---

## Parallel Jobs 🏃‍♂️🏃‍♀️

Jika kamu ingin menjalankan beberapa pod untuk menyelesaikan task yang besar secara paralel, kamu bisa mengonfigurasi **parallelism** di dalam Job.

### Contoh YAML Parallel Job:
```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: parallel-job
spec:
  parallelism: 3
  completions: 6
  template:
    spec:
      containers:
      - name: parallel-job-container
        image: busybox
        command: ["echo", "Running parallel job"]
      restartPolicy: Never
```

### Penjelasan:
- **parallelism**: Menentukan berapa banyak pod yang akan berjalan secara paralel. Dalam contoh ini, tiga pod akan dijalankan sekaligus.
- **completions**: Menentukan berapa kali job harus selesai. Dalam contoh ini, job harus selesai sebanyak enam kali, jadi Kubernetes akan menjalankan beberapa set pod sampai task selesai enam kali.
- **restartPolicy: Never**: Sama seperti sebelumnya, pod tidak akan di-restart setelah task selesai.

---

## Memantau dan Mengelola Job 🧰

### Melihat Status Job:
Kamu bisa melihat status Job yang sedang berjalan menggunakan perintah:

```bash
kubectl get jobs
```

Output contoh:
```bash
NAME          COMPLETIONS   DURATION   AGE
simple-job    1/1           30s        3m
```

### Memantau Pod yang Dijalankan oleh Job:
Untuk melihat pod yang dijalankan oleh job, kamu bisa menggunakan:

```bash
kubectl get pods --selector=job-name=simple-job
```

Perintah ini akan menampilkan pod yang dikelola oleh job tertentu.

---

## Menghapus Job 🗑️
Jika kamu ingin menghapus job (dan semua pod yang dikelola oleh job tersebut), gunakan perintah:

```bash
kubectl delete job <job-name>
```

Jika kamu ingin menghentikan job secara mendadak, kamu bisa menambahkan flag `--force`:

```bash
kubectl delete job <job-name> --force
```

---

## Job vs CronJob: Apa Bedanya? ⚖️

- **Job**: Digunakan untuk task yang hanya perlu dijalankan satu kali atau sejumlah kali tertentu. Misalnya, task backup atau migrasi data.
- **CronJob**: Digunakan untuk task yang perlu dijalankan secara **berkala** atau terjadwal (misalnya setiap hari atau setiap jam).

Kalau kamu butuh menjalankan task berulang dengan jadwal tertentu, **CronJob** adalah pilihan yang tepat, yang akan kita bahas lebih lanjut di chapter berikutnya.
