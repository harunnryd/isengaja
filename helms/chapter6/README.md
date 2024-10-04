
# Chapter 6: Menggunakan Helm Dependencies untuk Mengelola Chart yang Kompleks 📦

Helm memungkinkan kamu untuk membuat chart yang memiliki **dependensi** pada chart lain. Fitur ini sangat membantu jika aplikasi yang kamu deploy memerlukan beberapa komponen yang berbeda, misalnya database eksternal atau service lain yang saling bergantung.

---

## Apa Itu Helm Dependencies?

**Helm dependencies** adalah chart lain yang diperlukan oleh chart utama kamu agar aplikasi bisa berjalan. Misalnya, jika aplikasi kamu membutuhkan Redis sebagai penyimpanan sementara atau PostgreSQL sebagai database utama, kamu bisa mendefinisikan chart Redis atau PostgreSQL sebagai dependensi di chart aplikasi kamu.

---

## Cara Menambahkan Dependensi pada Chart

Untuk menambahkan dependensi pada Helm chart, ikuti langkah-langkah berikut:

### 1. Edit File `Chart.yaml`
Dependensi didefinisikan di dalam file `Chart.yaml` di chart utama. Kamu bisa menambahkan bagian `dependencies` seperti berikut:

```yaml
dependencies:
  - name: redis
    version: "14.8.5"
    repository: "https://charts.bitnami.com/bitnami"
  - name: postgresql
    version: "10.3.11"
    repository: "https://charts.bitnami.com/bitnami"
```

- **name**: Nama chart dependensi.
- **version**: Versi chart dependensi yang ingin kamu gunakan.
- **repository**: URL dari repository tempat chart dependensi berada.

### 2. Mengunduh Dependensi Chart
Setelah dependensi ditambahkan ke `Chart.yaml`, kamu perlu mengunduhnya ke dalam chart dengan perintah berikut:

```bash
helm dependency update
```

Perintah ini akan membuat folder `charts/` dan mengunduh semua chart dependensi ke dalamnya.

### 3. Melakukan Install dengan Dependensi
Setelah dependensi diunduh, kamu bisa melakukan install chart utama beserta dependensinya seperti biasa:

```bash
helm install my-release ./my-app
```

Helm akan menginstall chart utama dan semua dependensi yang telah kamu definisikan.

---

## Overriding Values dari Dependensi

Kamu juga bisa melakukan override pada `values.yaml` dari chart dependensi. Misalnya, jika kamu ingin mengubah konfigurasi Redis yang ada di dependensi, kamu bisa menambahkan nilai di `values.yaml` chart utama seperti ini:

```yaml
redis:
  master:
    replicaCount: 2
  image:
    tag: "6.2"
```

Dengan cara ini, kamu bisa menyesuaikan nilai dari chart dependensi tanpa harus memodifikasi chart itu sendiri.

---

## Mengelola Versi Dependensi

Helm memungkinkan kamu untuk mengunci versi dari chart dependensi menggunakan field `version` di `Chart.yaml`. Dengan mengunci versi, kamu bisa memastikan bahwa aplikasi kamu berjalan dengan versi yang tepat dari chart dependensi, dan menghindari ketidakcocokan versi yang dapat menyebabkan error.

Jika kamu ingin memperbarui dependensi ke versi terbaru, cukup jalankan perintah berikut:

```bash
helm dependency update
```

Ini akan memperbarui chart dependensi ke versi yang lebih baru sesuai dengan yang didefinisikan di repository.

---

## Menghapus Dependensi

Jika kamu tidak lagi memerlukan dependensi, kamu bisa menghapus entri dari `Chart.yaml` dan folder `charts/` akan dihapus ketika kamu menjalankan perintah `helm dependency update` berikutnya.

---

## Kesimpulan:

Pada **Chapter 6** ini, kita telah membahas cara mengelola **Helm dependencies** untuk chart yang lebih kompleks. Dependensi ini memungkinkan kamu untuk membuat aplikasi yang saling terhubung tanpa harus membuat ulang semua komponen. Ini sangat membantu ketika kamu memiliki aplikasi yang menggunakan beberapa service eksternal, seperti database atau cache.

Di **Chapter 7**, kita akan membahas lebih dalam tentang bagaimana melakukan **testing** dan **validasi** pada Helm chart untuk memastikan bahwa semua resource yang dihasilkan sudah benar.
