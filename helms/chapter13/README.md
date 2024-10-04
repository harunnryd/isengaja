
# Chapter 13: Private Helm Repository 🔐

Pada chapter ini, kita akan membahas bagaimana meng-host **private Helm repository** yang aman dan cara mengaksesnya dengan autentikasi.

---

## 1. Hosting di GitHub Pages
Untuk repository publik, GitHub Pages adalah solusi yang populer. Namun, jika kamu ingin repository privat, kamu perlu menggunakan layanan seperti **JFrog Artifactory** atau **AWS S3**.

### Contoh Hosting di S3:
1. Upload chart ke bucket S3:
   ```bash
   aws s3 cp ./my-chart-1.0.0.tgz s3://my-helm-repo/charts/
   ```

2. Buat file `index.yaml`:
   ```bash
   helm repo index s3://my-helm-repo/charts/
   ```

3. Konfigurasi S3 untuk private access dan autentikasi pengguna.

---

## 2. Menambahkan Repository Privat
Jika repository sudah di-host secara privat, pengguna bisa menambahkannya dengan autentikasi:

```bash
helm repo add my-repo https://my-private-repo.com --username USER --password PASS
```

---

## Kesimpulan:
Pada **Chapter 13**, kita telah belajar cara meng-host **private Helm repository** menggunakan layanan seperti AWS S3 dan bagaimana menambahkannya dengan autentikasi.
