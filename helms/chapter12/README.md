
# Chapter 12: Security di Helm Charts 🔒

Pada chapter ini, kita akan membahas bagaimana menjaga **keamanan** dalam Helm chart, termasuk mengelola secrets dengan aman dan menghindari kebocoran data sensitif.

---

## 1. Menggunakan Helm Secrets
Untuk mengelola secrets dengan aman di Helm, kamu bisa menggunakan **helm-secrets**. Plugin ini memungkinkan kamu untuk mengenkripsi secrets dan menyimpannya secara aman di chart.

### Instalasi:
```bash
helm plugin install https://github.com/jkroepke/helm-secrets
```

### Menggunakan Helm Secrets:
Kamu bisa menyimpan file secrets sebagai file terenkripsi:
```bash
helm secrets enc secrets.yaml
```

Saat melakukan install atau upgrade, Helm akan mendekripsi secrets secara otomatis:
```bash
helm secrets install my-release ./my-chart
```

---

## 2. Menghindari Secrets di Values.yaml
Hindari menyimpan data sensitif seperti passwords atau API keys di `values.yaml`. Gunakan tools seperti **Helm Secrets** atau sistem manajemen secrets dari cloud provider (misalnya AWS Secrets Manager atau Google Secret Manager) untuk menjaga keamanan.

---

## Kesimpulan:
Pada **Chapter 12**, kita telah belajar bagaimana menjaga keamanan dalam Helm chart dengan menggunakan plugin **Helm Secrets** dan tips mengelola secrets dengan aman.
