
# Chapter 11: Helm Plugin dan Customization 🔧

Pada chapter ini, kita akan membahas penggunaan **Helm plugins** untuk menambah fungsionalitas Helm dan cara membuat plugin custom untuk memenuhi kebutuhan spesifik.

---

## 1. Apa Itu Helm Plugins?
**Helm plugins** adalah ekstensi yang menambah fitur baru ke Helm CLI. Plugin ini memungkinkan kita untuk mengotomatisasi tugas tambahan yang tidak didukung secara default oleh Helm.

### Beberapa Plugin Populer:
- **helm-diff**: Membandingkan perbedaan antara release Helm.
- **helm-secrets**: Menyimpan dan mengelola secrets secara aman di Helm.
- **tillerless**: Menggunakan Helm tanpa Tiller (versi Helm lama).

---

## 2. Instalasi Plugin
Untuk menginstal plugin, gunakan perintah berikut:

```bash
helm plugin install https://github.com/databus23/helm-diff
```

Setelah terinstall, plugin bisa digunakan seperti perintah Helm lainnya:

```bash
helm diff upgrade my-release ./my-chart
```

---

## 3. Membuat Plugin Custom
Jika Helm tidak menyediakan plugin yang sesuai dengan kebutuhan kamu, kamu bisa membuat plugin sendiri. Plugin adalah skrip yang dijalankan oleh Helm.

### Contoh Membuat Plugin Custom:

1. Buat folder di `~/.helm/plugins/`
2. Buat file `plugin.yaml` yang mendefinisikan plugin:
   ```yaml
   name: my-plugin
   version: "0.1.0"
   usage: "helm my-plugin"
   description: "My custom Helm plugin"
   command: "./my-script.sh"
   ```

3. Buat `my-script.sh` berisi perintah yang kamu butuhkan:
   ```bash
   echo "Hello from my Helm plugin!"
   ```

Jalankan plugin dengan:

```bash
helm my-plugin
```

---

## Kesimpulan:
Pada **Chapter 11**, kita telah belajar tentang Helm plugins dan cara membuat plugin custom untuk memperluas fungsionalitas Helm.
