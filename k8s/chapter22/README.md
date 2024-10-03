
# Vertical Pod Autoscaler (VPA) di Kubernetes 📉📈

**Vertical Pod Autoscaler (VPA)** di Kubernetes adalah fitur yang secara otomatis menyesuaikan permintaan sumber daya (CPU dan memory) dari pod berdasarkan penggunaan aktual. Jika pod memerlukan lebih banyak sumber daya untuk menangani beban kerja, VPA akan mengalokasikannya, atau sebaliknya, jika pod menggunakan terlalu banyak sumber daya, VPA dapat mengurangi jumlah yang dialokasikan.

## Apa Itu Vertical Pod Autoscaler (VPA)? 🤔

**Vertical Pod Autoscaler** adalah resource yang memungkinkan Kubernetes untuk mengatur secara dinamis penggunaan **CPU** dan **memory** dari pod. Berbeda dengan **Horizontal Pod Autoscaler (HPA)** yang menambah atau mengurangi jumlah pod, VPA menyesuaikan jumlah sumber daya yang digunakan oleh pod secara vertikal, yaitu menambah atau mengurangi resource seperti CPU dan memory yang dialokasikan ke pod.

### Kapan Menggunakan VPA?
1. Aplikasi dengan beban kerja yang tidak tetap, tetapi memerlukan penyesuaian sumber daya secara otomatis.
2. Aplikasi yang tidak cocok untuk scaling horizontal, tetapi perlu menyesuaikan alokasi resource internal.
3. Pengoptimalan pemakaian sumber daya di lingkungan cluster dengan resource terbatas.

---

## Cara Kerja Vertical Pod Autoscaler 🔄

VPA secara berkala memonitor metrik penggunaan CPU dan memory dari pod, dan berdasarkan pola penggunaan tersebut, VPA merekomendasikan perubahan resource limit yang lebih sesuai. Jika pod memerlukan lebih banyak CPU atau memory, VPA akan meningkatkan resource limit, atau sebaliknya.

---

## Membuat Vertical Pod Autoscaler 🛠️

Berikut adalah contoh YAML file untuk membuat VPA yang secara otomatis menyesuaikan **CPU** dan **memory** pod.

### Contoh YAML VPA:
```yaml
apiVersion: autoscaling.k8s.io/v1
kind: VerticalPodAutoscaler
metadata:
  name: nginx-vpa
spec:
  targetRef:
    apiVersion: "apps/v1"
    kind:       Deployment
    name:       nginx-deployment
  updatePolicy:
    updateMode: "Auto"
```

### Penjelasan:
- **targetRef**: Menunjukkan resource mana yang akan diatur oleh VPA, dalam hal ini Deployment bernama **nginx-deployment**.
- **updatePolicy**: Menentukan bagaimana VPA akan menerapkan perubahan. Mode **Auto** berarti VPA akan secara otomatis memperbarui pod berdasarkan rekomendasi.

---

## Mode Pengoperasian VPA 🔄

VPA memiliki beberapa mode operasi yang berbeda:

1. **Auto**: VPA secara otomatis menerapkan perubahan resource tanpa campur tangan manual.
2. **Off**: VPA hanya memberikan rekomendasi, tetapi tidak melakukan perubahan otomatis.
3. **Initial**: VPA hanya memberikan rekomendasi saat pod pertama kali dijalankan, tetapi tidak mengubah resource selama runtime.

Untuk mengubah mode pengoperasian, kamu bisa mengedit bagian **updatePolicy**.

---

## Melihat Rekomendasi VPA 📊

Setelah VPA berjalan, kamu bisa melihat rekomendasi penggunaan CPU dan memory yang disarankan oleh VPA menggunakan perintah:

```bash
kubectl describe vpa nginx-vpa
```

### Output Contoh:
```bash
TargetRef:  Deployment/nginx-deployment
Update Mode: Auto
Recommendations:
  Container: nginx
    Target:
      Cpu: 200m
      Memory: 100Mi
```

Ini menunjukkan rekomendasi penggunaan sumber daya yang lebih sesuai berdasarkan observasi VPA terhadap beban kerja pod.

---

## Menghapus Vertical Pod Autoscaler 🗑️

Jika kamu ingin menghapus VPA, kamu bisa menggunakan perintah berikut:

```bash
kubectl delete vpa <vpa-name>
```

Ini akan menghapus Vertical Pod Autoscaler dari cluster.

---

## Kapan Memilih VPA vs HPA? ⚖️

- **VPA** cocok untuk aplikasi yang lebih sensitif terhadap perubahan sumber daya internal (CPU dan memory) tetapi tidak memerlukan scaling jumlah pod.
- **HPA** cocok untuk aplikasi yang membutuhkan penyesuaian jumlah pod berdasarkan beban kerja.
- Keduanya bisa digunakan bersama, tetapi penting untuk menyesuaikan resource dan konfigurasi yang optimal agar keduanya bekerja dengan baik.
