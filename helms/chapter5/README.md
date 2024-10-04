
# Chapter 5: Menggunakan Helm Hooks untuk Mengelola Deployment Dinamis 🎣

Pada chapter ini, kita akan belajar tentang **Helm hooks**, yaitu mekanisme yang memungkinkan kita mengelola siklus hidup deployment dengan lebih dinamis. Hooks sangat berguna untuk menjalankan perintah tertentu di berbagai tahap deployment, seperti sebelum atau sesudah aplikasi di-deploy, atau bahkan sebelum rollback dilakukan.

---

## Apa Itu Helm Hooks?

Helm hooks adalah cara untuk menjalankan pekerjaan tambahan di berbagai titik siklus hidup Helm chart. Misalnya, jika kamu ingin menjalankan job inisialisasi sebelum aplikasi mulai dijalankan atau menjalankan job cleanup setelah aplikasi dihapus, kamu bisa menggunakan hooks.

Helm mendukung beberapa hook lifecycle events seperti:

- **pre-install**: Dijalankan sebelum resource utama di-install.
- **post-install**: Dijalankan setelah resource utama selesai di-install.
- **pre-delete**: Dijalankan sebelum resource dihapus.
- **post-delete**: Dijalankan setelah resource selesai dihapus.
- **pre-upgrade**: Dijalankan sebelum upgrade dimulai.
- **post-upgrade**: Dijalankan setelah upgrade selesai.
- **pre-rollback**: Dijalankan sebelum rollback dimulai.
- **post-rollback**: Dijalankan setelah rollback selesai.

---

## Contoh Penggunaan Helm Hooks

Berikut adalah contoh penggunaan hook `pre-install` untuk menjalankan job inisialisasi sebelum aplikasi di-deploy:

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: pre-install-job
  annotations:
    "helm.sh/hook": pre-install
spec:
  template:
    spec:
      containers:
      - name: init
        image: busybox
        command: ['sh', '-c', 'echo Pre-install Job is running']
      restartPolicy: Never
```

Pada contoh di atas, job `pre-install-job` akan dijalankan sebelum aplikasi di-install. Kamu bisa menambahkan perintah inisialisasi lain, seperti migrasi database, pengecekan environment, atau perintah lain yang diperlukan.

---

## Daftar Helm Hooks yang Umum Digunakan

Berikut beberapa hook yang sering digunakan dalam deployment:

1. **pre-install**: Berguna untuk mempersiapkan lingkungan sebelum deployment (contoh: migrasi database, setup konfigurasi).
2. **post-install**: Digunakan setelah deployment selesai, misalnya untuk mengirimkan notifikasi sukses atau menjalankan test smoke.
3. **pre-upgrade**: Digunakan sebelum melakukan upgrade, seperti backup data atau melakukan validasi terhadap versi aplikasi yang baru.
4. **post-upgrade**: Dijalankan setelah upgrade selesai, bisa digunakan untuk memverifikasi deployment baru atau membersihkan resource yang tidak diperlukan.
5. **pre-delete**: Digunakan untuk melakukan cleanup atau backup sebelum aplikasi dihapus.
6. **post-delete**: Digunakan setelah aplikasi dihapus untuk membersihkan resource yang terkait.

---

## Mengelola Hooks

Helm menyediakan beberapa parameter untuk mengelola hooks:

- **hook-weight**: Menentukan urutan eksekusi hook jika ada beberapa hook di event yang sama. Hook dengan nilai `hook-weight` lebih kecil akan dijalankan lebih dahulu.
  
  Contoh penambahan `hook-weight`:

  ```yaml
  annotations:
    "helm.sh/hook": pre-install
    "helm.sh/hook-weight": "-5"
  ```

- **hook-delete-policy**: Menentukan kapan resource hook harus dihapus setelah dijalankan. Misalnya, hook bisa dihapus segera setelah berhasil dieksekusi, atau dibiarkan tetap ada hingga job selesai.

  Contoh penggunaan `hook-delete-policy`:

  ```yaml
  annotations:
    "helm.sh/hook": post-install
    "helm.sh/hook-delete-policy": hook-succeeded
  ```

---

## Kesimpulan

Pada **Chapter 5** ini, kita telah membahas penggunaan **Helm hooks** untuk mengelola siklus hidup deployment dengan lebih fleksibel. Hooks memungkinkan kita untuk menjalankan job tambahan di berbagai tahap deployment, mulai dari instalasi, upgrade, hingga penghapusan aplikasi. 

Di **Chapter 6**, kita akan membahas lebih lanjut tentang bagaimana menggunakan **Helm dependencies** untuk mengelola chart yang memiliki dependensi pada chart lain.
