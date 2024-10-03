
# CronJob di Kubernetes ⏰

**CronJob** di Kubernetes adalah resource yang digunakan untuk menjalankan task atau pekerjaan secara **terjadwal**. Sama seperti cron di Linux, **CronJob** memungkinkan kamu menjalankan task otomatis pada interval waktu yang telah ditentukan, seperti setiap jam, setiap hari, atau sesuai kebutuhan.

## Apa Itu CronJob? 🤔

**CronJob** adalah salah satu resource Kubernetes yang memungkinkan task atau pod dijalankan secara **periodik** sesuai jadwal yang telah kamu tentukan. CronJob sangat ideal untuk task berulang seperti:
- **Backup** harian.
- **Mengirim email** secara berkala.
- **Membersihkan database** atau log.

CronJob dibangun di atas resource **Job**, tetapi dengan penjadwalan otomatis.

### Contoh Penggunaan CronJob:
1. **Backup otomatis** database setiap hari pada pukul 00:00.
2. **Pengiriman laporan** email secara otomatis setiap minggu.
3. **Pembersihan log** aplikasi setiap malam.

---

## Cara Kerja CronJob 🔄

CronJob menggunakan format **cron** yang sama seperti di sistem Unix untuk menentukan jadwal. Setiap CronJob akan menghasilkan **Job** baru sesuai jadwal yang telah ditentukan. Jika ada kegagalan saat menjalankan job, Kubernetes bisa mencoba menjalankan ulang sesuai konfigurasi.

Format cron standar adalah:
```
* * * * *
┬ ┬ ┬ ┬ ┬
│ │ │ │ │
│ │ │ │ └─ Hari dalam minggu (0 - 6) (Minggu ke Sabtu)
│ │ │ └─── Bulan (1 - 12)
│ │ └───── Hari dalam bulan (1 - 31)
│ └─────── Jam (0 - 23)
└───────── Menit (0 - 59)
```

### Contoh Format:
- **0 0 * * ***: Menjalankan task setiap hari pada pukul 00:00.
- ***/5 * * * ***: Menjalankan task setiap 5 menit.
- **0 8 * * 1**: Menjalankan task setiap Senin pada pukul 08:00.

---

## Membuat CronJob 🛠️

Berikut adalah contoh YAML file untuk membuat CronJob yang menjalankan task setiap hari pada pukul 00:00.

### Contoh YAML CronJob:
```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: backup-cronjob
spec:
  schedule: "0 0 * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: backup-container
            image: busybox
            command: ["echo", "Backup database running..."]
          restartPolicy: OnFailure
```

### Penjelasan:
- **schedule**: Jadwal dalam format cron. Dalam contoh ini, job akan dijalankan setiap hari pada pukul 00:00.
- **jobTemplate**: Template job yang akan dibuat oleh CronJob. Di sini, job akan menjalankan command **echo** untuk menampilkan pesan "Backup database running...".
- **restartPolicy: OnFailure**: Pod hanya akan di-restart jika task gagal dijalankan.

---

## Memantau dan Mengelola CronJob 🧰

### Melihat Status CronJob:
Kamu bisa melihat status CronJob menggunakan perintah:

```bash
kubectl get cronjobs
```

### Output Contoh:
```bash
NAME              SCHEDULE    SUSPEND   ACTIVE   LAST SCHEDULE   AGE
backup-cronjob    0 0 * * *   False     0        1h              3d
```

Penjelasan:
- **SCHEDULE**: Jadwal kapan CronJob akan dijalankan.
- **ACTIVE**: Berapa banyak job yang aktif saat ini.
- **LAST SCHEDULE**: Kapan terakhir kali job dijadwalkan.

### Melihat Job yang Dijalankan oleh CronJob:
Untuk melihat job yang telah dijalankan oleh CronJob, gunakan perintah:

```bash
kubectl get jobs --selector=cronjob-name=backup-cronjob
```

Perintah ini akan menampilkan semua job yang telah dijalankan oleh CronJob bernama `backup-cronjob`.

---

## Menghapus CronJob 🗑️
Jika kamu ingin menghapus CronJob (beserta semua job yang telah dijalankan oleh CronJob tersebut), gunakan perintah:

```bash
kubectl delete cronjob <cronjob-name>
```
