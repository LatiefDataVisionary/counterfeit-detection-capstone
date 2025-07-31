# AI-Powered Counterfeit Detection and Risk Summarization

**Capstone Project untuk Student Development Initiative (IBM & Hacktiv8)**
**Oleh: [NAMA LENGKAP ANDA]**

---

### **1. Ringkasan Proyek (Project Overview)**

Proyek ini bertujuan untuk mengatasi masalah krusial dalam industri e-commerce: peredaran produk palsu. Dengan memanfaatkan data transaksional dan atribut produk, proyek ini membangun sebuah sistem cerdas untuk:
1.  **Mengklasifikasikan (Classification):** Mengidentifikasi dan memprediksi produk yang berpotensi palsu dengan akurasi tinggi menggunakan berbagai model machine learning.
2.  **Meringkas (Summarization):** Menggunakan model bahasa (Large Language Model) dari IBM Granite untuk secara otomatis menghasilkan ringkasan risiko dalam bahasa manusia, membantu tim moderator mengambil keputusan yang cepat dan tepat.

---

### **2. Sumber Data (Raw Dataset)**

*   **Nama Dataset:** Counterfeit Product Detection Dataset
*   **Sumber:** [Kaggle](https://www.kaggle.com/datasets/aimlveera/counterfeit-product-detection-dataset)
*   **Deskripsi:** Dataset ini berisi data terstruktur yang mensimulasikan listingan produk di platform e-commerce. Mencakup fitur-fitur seperti harga, rating penjual, kategori, asal pengiriman, dan metrik lainnya. Kolom target adalah `is_counterfeit`.
*   File dataset mentah dapat ditemukan di direktori: `/data/raw/`

---

### **3. Alur Analisis & Metodologi**

Proyek ini dibagi menjadi empat fase utama:

1.  **Pembersihan & Feature Engineering:** Standardisasi data (misalnya nama brand) dan menciptakan fitur baru seperti rasio pembelian-terhadap-tayangan dan perbandingan harga dengan rata-rata kategori untuk memperkaya model.
2.  **Analisis Data Eksploratif (EDA):** Menggali insight awal melalui visualisasi untuk memahami karakteristik produk palsu, seperti korelasi dengan harga, rating penjual, dan negara asal pengiriman.
3.  **Pemodelan & Perbandingan:** Melatih dan mengevaluasi 6 model klasifikasi yang berbeda (Logistic Regression, KNN, SVM, Random Forest, XGBoost, LightGBM) untuk menemukan model dengan performa terbaik dalam memprediksi produk palsu.
4.  **Integrasi IBM Granite:** Menggunakan model "juara" untuk melakukan prediksi. Jika produk terdeteksi palsu, data terstrukturnya akan dikirim ke IBM Granite untuk menghasilkan ringkasan risiko yang dinamis dan mudah dibaca.

---

### **4. Insight & Temuan Utama (Insight & Findings)**

*   **Indikator Terkuat:** Berdasarkan analisis *feature importance* dari model XGBoost, tiga prediktor terkuat untuk produk palsu adalah:
    1.  `[Contoh Insight 1: Misal: Harga yang jauh di bawah rata-rata kategori]`
    2.  `[Contoh Insight 2: Misal: Rating penjual yang sangat rendah dikombinasikan dengan waktu pengiriman yang lama]`
    3.  `[Contoh Insight 3: Misal: Produk yang dikirim dari negara 'X' memiliki probabilitas palsu 70% lebih tinggi]`
*   **Performa Model:** Model XGBoost mencapai performa terbaik dengan **F1-Score sebesar [Misal: 0.95]**, menunjukkan keseimbangan yang sangat baik antara presisi dan recall dalam mendeteksi produk palsu.

---

### **5. Penjelasan Dukungan AI (AI Support Explanation)**

AI dimanfaatkan dalam dua pilar utama proyek ini sesuai dengan objektif Capstone Project:

*   **AI untuk Klasifikasi:** Berbagai model *Machine Learning*, khususnya **XGBoost**, digunakan untuk mempelajari pola dari data historis dan secara otomatis mengklasifikasikan listingan baru sebagai "Asli" atau "Palsu".
*   **AI untuk Summarization (IBM Granite):** Kami menggunakan LLM **IBM Granite** untuk tugas yang lebih canggih daripada sekadar meringkas teks. AI ini menerima **data terstruktur** (angka dan kategori) sebagai input dan menghasilkan **ringkasan naratif (penjelasan)** sebagai output. Ini mendemonstrasikan kemampuan AI untuk *reasoning* dan menjelaskan data kompleks dalam bahasa yang mudah dimengerti, yang merupakan aplikasi LLM yang sangat kuat untuk lingkungan bisnis.

---

### **6. Cara Menjalankan Proyek**

1.  *Clone* repositori ini.
2.  Pastikan Anda memiliki Python 3.x.
3.  Instal semua dependensi yang dibutuhkan:
    ```bash
    pip install -r requirements.txt
    ```
4.  Buka dan jalankan notebook di `/notebooks/Counterfeit_Detection_Analysis.ipynb` menggunakan Google Colab atau Jupyter Notebook.
