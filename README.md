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
1. **Menangani Nilai yang Hilang (Missing Values)**
   - Memeriksa kolom Description dan Notes untuk nilai kosong atau NaN, kemudian menggantinya dengan string kosong (`""`), agar data tidak memiliki nilai kosong yang dapat menyebabkan masalah saat proses selanjutnya.

2. **Feature Engineering (Pengolahan Teks)**
   - **Stopwords Removal & Tokenization**:
     - Menghapus kata-kata umum yang tidak memberikan informasi penting (misalnya, "the", "and", "is") untuk menyaring kata-kata yang tidak signifikan.
     - Memecah teks menjadi kata atau token untuk memudahkan proses vektorisasi.
   - **TF-IDF (Term Frequency-Inverse Document Frequency)**:
     - Mengubah teks di kolom Description dan Notes menjadi vektor numerik menggunakan teknik TF-IDF.
     - `stop_words='english'` digunakan untuk menghapus kata-kata umum yang tidak memberikan informasi penting.
     - `max_features=500` membatasi jumlah kata yang dipertimbangkan dalam proses vektorisasi untuk menghindari overfitting dan meningkatkan efisiensi.
     - Hasilnya adalah 500 kolom vektor dari Description dan 500 kolom vektor dari Notes.

3. **Penggabungan Fitur**
   - Menggabungkan vektor hasil dari TF-IDF untuk Description dan Notes menjadi satu matriks fitur menggunakan `hstack()`, menghasilkan total 1000 kolom fitur gabungan.
   - Menambahkan kolom Perfume_ID untuk mempermudah referensi setiap parfum dalam proses rekomendasi, serta menyimpan fitur yang telah digabungkan ke dalam DataFrame agar lebih mudah diakses dan dimanipulasi.

### **Alasan Tahapan**
- **Menangani Nilai yang Hilang**: Memastikan data lengkap untuk menghindari kesalahan dalam pemodelan, sehingga setiap parfum memiliki deskripsi dan catatan wewangian yang lengkap.
- **Feature Engineering**: Mengubah teks menjadi representasi numerik yang signifikan bagi model, meningkatkan kemampuan model untuk memahami dan mengukur kesamaan antar deskripsi dan catatan wewangian.
- **Penggabungan Fitur**: Membuat representasi fitur yang kaya dan lengkap, yang penting untuk meningkatkan akurasi dalam menghitung kesamaan antar parfum, sehingga rekomendasi yang dihasilkan lebih relevan dan tepat sasaran.

---

## **Modeling and Results**

### **Pendekatan**
Model sistem rekomendasi yang digunakan adalah **Content-Based Filtering** dengan langkah-langkah sebagai berikut:
1. **Penghitungan Kesamaan Cosine (Cosine Similarity)**:
   - Mengukur kesamaan antara parfum berdasarkan deskripsi dan catatan wewangian mereka menggunakan Cosine Similarity.
   - Rumus: 
     $$\text{Cosine Similarity} = \cos(\theta) = \frac{A \cdot B}{\|A\| \|B\|}$$
   - Di mana A dot B adalah dot product dari vektor A dan B, ||A|| dan ||B|| adalah panjang dari vektor A dan B, dan theta adalah sudut antara dua vektor.
2. **Pembangunan Model Rekomendasi**:
   - Menggunakan matriks cosine similarity untuk memilih parfum dengan kesamaan tertinggi dan menampilkan 5 parfum teratas yang paling mirip dengan parfum yang dipilih pengguna.

### **Top-N Recommendation**
Berikut adalah contoh hasil rekomendasi top-5:

| **Nama Parfum**        | **Rekomendasi**                                             |
|------------------------|-------------------------------------------------------------|
| **Tihota Eau de Parfum**       | **Muskara Phero J Eau de Parfum**, **Vanille Noire du Mexique Eau de Toilette**, **Vanille Tonka Eau de Parfum**, **Vanille Absolu Eau de Parfum**, **Insulo Parfum Extrait**  |

### **Kelebihan dan Kekurangan**
**Kelebihan**:
- Rekomendasi yang relevan berdasarkan deskripsi dan catatan wewangian parfum.
- Tidak memerlukan data pengguna atau interaksi sebelumnya, cocok untuk pengguna baru (cold-start problem).

**Kekurangan**:
- Hanya mengandalkan informasi dari deskripsi dan catatan wewangian, tidak mempertimbangkan preferensi atau perilaku pengguna sebelumnya.
- Rekomendasi mungkin tidak cukup beragam jika deskripsi dan catatan wewangian parfum sangat mirip satu sama lain.

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
