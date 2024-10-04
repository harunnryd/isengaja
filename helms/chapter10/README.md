
# Chapter 10: Helm Chart Testing dengan `ct` (Chart Testing) 🧪

Pada chapter ini, kita akan mempelajari cara menggunakan **ct** (Chart Testing), sebuah tool yang digunakan untuk melakukan testing pada Helm chart di CI/CD pipeline. Testing yang dilakukan dengan `ct` meliputi linting, unit testing, dan e2e testing untuk memastikan chart bekerja dengan baik sebelum di-deploy.

---

## 1. Instalasi `ct`
Langkah pertama untuk memulai adalah menginstal **ct**. Kamu bisa menginstal tool ini dengan menggunakan Homebrew:

```bash
brew install helm/chart-testing/chart-testing
```

Atau melalui Docker:
```bash
docker pull quay.io/helmpack/chart-testing
```

---

## 2. Menjalankan Linting dengan `ct`
Setelah instalasi, kamu bisa menjalankan linting pada chart untuk memeriksa struktur dan kesalahan umum:

```bash
ct lint --charts ./my-chart
```

Perintah ini akan memeriksa validitas YAML dan struktur chart.

---

## 3. Integrasi di GitHub Actions
Untuk menjalankan `ct` di CI/CD pipeline, kamu bisa menambahkan langkah ini di GitHub Actions. Berikut contoh GitHub Actions pipeline yang menggunakan `ct`:

```yaml
name: Chart Testing

on:
  pull_request:
    branches:
      - main

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: Install Helm
      run: |
        curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
    - name: Install ct
      run: |
        curl -LO https://github.com/helm/chart-testing/releases/download/v3.0.0/chart-testing_3.0.0_linux_amd64.tar.gz
        tar -xvf chart-testing_3.0.0_linux_amd64.tar.gz
        sudo mv ct /usr/local/bin/
    - name: Run ct lint
      run: ct lint --charts ./my-chart
```

---

## 4. Keuntungan Testing dengan `ct`
- **Otomatisasi Testing**: Memastikan chart berfungsi sebelum di-deploy.
- **Linting Mendalam**: Mendeteksi kesalahan umum dan pelanggaran best practices.
- **Kompatibilitas**: Memastikan chart kompatibel dengan berbagai versi Kubernetes.

---

## Kesimpulan:
Pada **Chapter 10**, kita telah mempelajari penggunaan **ct** untuk melakukan testing pada Helm chart dan mengintegrasikannya ke pipeline CI/CD.
