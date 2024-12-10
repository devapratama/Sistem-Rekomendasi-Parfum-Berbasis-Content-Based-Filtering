# **Laporan Proyek Machine Learning - Sistem Rekomendasi Parfum Berbasis Content-Based Filtering**

## **Project Overview**

### **Latar Belakang**
Dalam industri parfum, pemilihan parfum yang tepat seringkali menjadi tantangan bagi konsumen, khususnya mereka yang tidak berpengalaman. Banyaknya pilihan yang tersedia dan kurangnya informasi yang jelas menyebabkan konsumen kesulitan membuat keputusan. Untuk mengatasi tantangan ini, proyek ini bertujuan untuk membangun sistem rekomendasi parfum menggunakan **Content-Based Filtering**. Sistem ini diharapkan dapat memberikan rekomendasi personal berdasarkan karakteristik produk parfum.

### **Pentingnya Proyek**
Proyek ini penting untuk membantu konsumen menemukan parfum yang sesuai dengan preferensi mereka tanpa harus melalui proses pencarian manual yang memakan waktu. Sistem ini dapat menjadi solusi yang efisien dan personal, serta mampu meningkatkan kepuasan konsumen.

---

## **Business Understanding**

### **Problem Statements**
1. Bagaimana memberikan rekomendasi parfum yang relevan berdasarkan karakteristik parfum yang ada?
2. Bagaimana mengukur kesamaan antar parfum secara efektif untuk meningkatkan relevansi rekomendasi?

### **Goals**
1. Mengembangkan sistem rekomendasi parfum berbasis **Content-Based Filtering**.
2. Menganalisis kesamaan antar parfum berdasarkan deskripsi produk dan catatan wewangian.

### **Solution Approach**
Pendekatan yang digunakan untuk mencapai tujuan adalah:
1. Menggunakan **TF-IDF Vectorizer** untuk mengolah deskripsi parfum dan catatan wewangian menjadi representasi numerik.
2. Menghitung kesamaan antar parfum menggunakan **Cosine Similarity**.

---

## **Data Understanding**

### **Dataset**
Dataset yang digunakan berisi informasi mengenai 2.191 parfum dari berbagai merek. Dataset ini terdiri dari lima kolom utama:
- **Name**: Nama parfum.
- **Brand**: Merek atau perusahaan pembuat parfum.
- **Description**: Deskripsi parfum, termasuk karakteristik aroma.
- **Notes**: Daftar catatan wewangian (misalnya, citrus, musk, lavender).
- **Image URL**: URL gambar parfum.

**Kondisi Data**:
- Dataset  memiliki 80 data yang hilang untuk kolom Notes sehingga perlu ditangani dengan mengisi data menggunakan string kosong.
- **Description** dan **Notes** menjadi fokus utama untuk proses pemodelan.

**Sumber Data**:
Dataset dapat diakses melalui tautan berikut: https://www.kaggle.com/datasets/nandini1999/perfume-recommendation-dataset/data

---

## **Data Preparation**

### **Tahapan Data Preparation**
1. **Preprocessing Teks**: Tokenisasi dan penghapusan stop words.
2. **Representasi Data**: Menggunakan **TF-IDF Vectorizer** untuk mengubah data teks menjadi representasi numerik.
3. **Penggabungan Fitur**: Menggabungkan kolom **Description** dan **Notes** untuk menghasilkan satu fitur teks yang lebih informatif.

### **Alasan Tahapan**
- Preprocessing diperlukan untuk mengurangi kompleksitas data teks dan menghilangkan informasi yang tidak relevan.
- Representasi numerik menggunakan TF-IDF memastikan bahwa frekuensi dan pentingnya setiap kata diperhatikan.

---

## **Modeling and Results**

### **Pendekatan**
Model sistem rekomendasi yang digunakan adalah **Content-Based Filtering** dengan langkah-langkah sebagai berikut:
1. **TF-IDF Vectorizer** digunakan untuk mengekstrak fitur numerik dari teks.
2. **Cosine Similarity** digunakan untuk menghitung kemiripan antara parfum.
3. Sistem memberikan **top-N recommendation** berdasarkan parfum yang memiliki kemiripan tertinggi.

### **Top-N Recommendation**
Berikut adalah contoh hasil rekomendasi top-5:

| **Nama Parfum**        | **Rekomendasi**                                             |
|------------------------|-------------------------------------------------------------|
| **Tihota Eau de Parfum**       | **Muskara Phero J Eau de Parfum**, **Vanille Noire du Mexique Eau de Toilette**, **Vanille Tonka Eau de Parfum**, **Vanille Absolu Eau de Parfum**, **Insulo Parfum Extrait**  |

### **Kelebihan dan Kekurangan**
**Kelebihan**:
- Tidak membutuhkan data historis interaksi pengguna.
- Efektif untuk memberikan rekomendasi personal berdasarkan karakteristik produk.

**Kekurangan**:
- Tidak dapat menangani cold start untuk parfum baru tanpa deskripsi lengkap.
- Rentan terhadap data teks yang tidak terstruktur dengan baik.

---

## **Evaluation**

### **Metrik Evaluasi**
Evaluasi dilakukan menggunakan metrik **Precision at K** untuk mengukur relevansi rekomendasi.

**Formula Precision at K**
Precision at K dihitung dengan rumus:

**Precision@K** = (Jumlah rekomendasi relevan di Top K) / K

Dimana:
- "Jumlah rekomendasi relevan di Top K" adalah jumlah rekomendasi yang relevan dari total K rekomendasi yang diberikan.
- K adalah jumlah rekomendasi yang ditampilkan (misalnya 5, 10, atau 20).

**Hasil Evaluasi**:
- **K=5**: Precision = 1.00 (68 kali dari 200 sampel), menunjukkan bahwa sekitar 34% dari parfum mendapatkan rekomendasi yang sepenuhnya relevan.
- **K=10**: Precision = 1.00 (30 kali dari 200 sampel), tetapi hanya sekitar 15% sampel yang mendapatkan rekomendasi sepenuhnya relevan.
- **K=20**: Precision = 1.00 (11 kali dari 200 sampel), menandakan hanya sekitar 5% sampel yang relevan.

### **Kesimpulan**
- **K=5** memberikan hasil evaluasi terbaik dibandingkan dengan K yang lebih besar.
- Sistem ini cukup baik untuk memberikan rekomendasi personal meskipun memiliki keterbatasan pada cold start.
