<div align="center">

# 🤖 AI-Powered Counterfeit Detection & Risk Summarization 🛡️

**Capstone Project untuk Student Development Initiative**
<br/>
<a href="https://www.ibm.com" target="_blank"><img src="https://img.shields.io/badge/Powered_by-IBM_Granite-0062FF?style=for-the-badge&logo=ibm" /></a>
<a href="https://www.hacktiv8.com" target="_blank"><img src="https://img.shields.io/badge/Partner-HACKTIV8-FF6F00?style=for-the-badge&logo=hacktiv8&logoColor=white" /></a>
<br/>
**Oleh: LATHIF RAMADHAN**

</div>

---

<div align="center">

![Python](https://img.shields.io/badge/Python-3.9%2B-blue?style=for-the-badge&logo=python&logoColor=white)
![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-0.24%2B-orange?style=for-the-badge&logo=scikit-learn)
![XGBoost](https://img.shields.io/badge/XGBoost-1.5%2B-blue?style=for-the-badge&logo=xgboost)
![LangChain](https://img.shields.io/badge/LangChain-Framework-lightgrey?style=for-the-badge&logo=langchain)
![Project Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)

</div>

### **Navigation**
*   [🎯 **Ringkasan & Tujuan Proyek**](#1-ringkasan-proyek-project-overview)
*   [📦 **Sumber Data (Dataset)**](#2-sumber-data-raw-dataset)
*   [⚙️ **Alur Analisis & Metodologi**](#3-alur-analisis--metodologi)
*   [💡 **Insight & Temuan Utama**](#4-insight--temuan-utama-key-insights--findings)
*   [🤖 **Peran Kunci AI: Klasifikasi & Penalaran**](#5-penjelasan-dukungan-ai-ai-support-explanation)
*   [🚀 **Cara Menjalankan Proyek**](#6-cara-menjalankan-proyek)

---

### **1. Ringkasan Proyek (Project Overview)**
Proyek ini dibangun untuk mengatasi tantangan signifikan dalam ekosistem e-commerce: proliferasi produk palsu. Dengan menganalisis data transaksi dan atribut produk secara cermat, proyek ini menghadirkan solusi cerdas yang menggabungkan kekuatan Machine Learning tradisional dengan kemampuan Large Language Model (LLM) modern untuk:

1.  **Klasifikasi Produk:** Mengembangkan model prediktif yang akurat untuk mengidentifikasi listingan produk yang berpotensi palsu. Ini memungkinkan platform e-commerce untuk secara proaktif menandai atau menghapus produk berisiko sebelum mencapai konsumen.
2.  **Ringkasan Risiko Cerdas:** Memanfaatkan model bahasa besar (LLM) **IBM Granite** untuk mengubah data terstruktur (statistik, kategori, dll.) menjadi narasi ringkas yang menjelaskan alasan di balik prediksi "palsu". Ini memberikan konteks yang kaya bagi tim *Trust & Safety* untuk mempercepat investigasi dan pengambilan keputusan.

Sistem ini bertujuan untuk meningkatkan kepercayaan konsumen, melindungi reputasi penjual asli, dan menciptakan lingkungan belanja online yang lebih aman.

### **2. Sumber Data (Raw Dataset)**
*   **Nama Dataset:** `Counterfeit Product Detection Dataset`
*   **Sumber:** [![Kaggle](https://img.shields.io/badge/Kaggle-Dataset-blue.svg?style=flat-square)](https://www.kaggle.com/datasets/aimlveera/counterfeit-product-detection-dataset)
*   **Deskripsi:** Dataset ini adalah simulasi data listingan produk dari platform e-commerce, terdiri dari **5000 baris** data dengan **27 kolom** yang mencakup berbagai atribut seperti harga produk, rating dan ulasan penjual, jumlah gambar, panjang deskripsi, waktu pengiriman, asal pengiriman, metrik interaksi pembeli, serta indikator lain yang terkait dengan potensi pemalsuan. Kolom target (`is_counterfeit`) adalah variabel boolean yang menandai apakah produk tersebut asli (False) atau palsu (True).
*   **Lokasi File Mentah:**
    ```bash
    /data/raw/Counterfeit_Product_Detection.csv
    ```

### **3. Alur Analisis & Metodologi**
Proyek ini dirancang mengikuti pipeline data science yang sistematis dan terstruktur:

**`Data Cleaning & Feature Engineering`** -> **`Exploratory Data Analysis (EDA)`** -> **`Model Training & Comparison`** -> **`IBM Granite Integration & Summarization`**

<details>
<summary><strong>Kilk di sini untuk melihat detail setiap fase...</strong></summary>

1.  **Fase 1: Pembersihan Data & Feature Engineering:**
    *   Melakukan standardisasi data kategorikal, khususnya nama `brand`.
    *   Menangani potensi outlier pada fitur numerik menggunakan metode *capping* berbasis IQR.
    *   Mengkonversi kolom `listing_date` ke format `datetime` dan membuat fitur turunan.
    *   Menciptakan fitur baru seperti `purchase_to_view_ratio` dan `is_price_below_avg`.
    *   Memastikan kualitas data dengan memeriksa *missing values* dan duplikat.

2.  **Fase 2: Analisis Data Eksploratif (EDA):**
    *   Menganalisis distribusi kelas target dan mengidentifikasi *class imbalance*.
    *   Memvisualisasikan hubungan fitur-kunci dengan status produk (asli/palsu).
    *   Mengeksplorasi korelasi antar fitur numerik melalui *heatmap*.
    *   Menganalisis fitur kategorikal vs. target menggunakan *crosstab* untuk mengungkap pola.

3.  **Fase 3: Pemodelan & Perbandingan Model (Classification):**
    *   Menerapkan *preprocessing pipeline* (`StandardScaler` untuk numerik, `OneHotEncoder` untuk kategorikal).
    *   Membagi data dengan stratifikasi.
    *   Membangun dan melatih 6 model: **Logistic Regression, KNN, SVM, Random Forest, XGBoost, dan LightGBM.**
    *   Mengevaluasi performa menggunakan metrik komprehensif (Akurasi, Presisi, Recall, F1-Score, ROC AUC, dll.).
    *   Melakukan *Cross-Validation* dan *tuning hyperparameter* (`GridSearchCV`).
    *   Menganalisis **Feature Importance** dari model terbaik.
    *   Menyimpan model final menggunakan `joblib`.

4.  **Fase 4: Integrasi IBM Granite (Summarization Cerdas):**
    *   Mengintegrasikan IBM Granite melalui API Replicate dan diatur oleh Langchain.
    *   Mengembangkan *prompt template* yang canggih.
    *   Mengirim data terstruktur produk yang terdeteksi palsu ke IBM Granite.
    *   LLM bertugas untuk melakukan **reasoning** dan menghasilkan ringkasan naratif tentang **mengapa** produk itu berisiko, bukan sekadar meringkas deskripsi.

</details>

### **4. Insight & Temuan Utama (Key Insights & Findings)**
> **Performa Model yang Sempurna:** Sebuah temuan menarik, seluruh model klasifikasi yang diuji berhasil mencapai **performa 100% (Skor 1.0)** pada semua metrik evaluasi di data uji. Hal ini menunjukkan bahwa fitur-fitur yang ada di dalam dataset ini sangat diskriminatif dan memiliki daya pemisah yang sangat kuat antara produk asli dan palsu. Dalam skenario dunia nyata, hasil seperti ini jarang terjadi dan menyoroti kualitas tinggi dari fitur yang direkayasa dalam dataset simulasi ini.

Berikut adalah faktor-faktor paling berpengaruh dalam mendeteksi produk palsu, diurutkan berdasarkan **Feature Importance** dari model Random Forest:
1.  **📸 `product_images`**: Jumlah gambar sedikit adalah tanda bahaya utama.
2.  **⏳ `domain_age_days`**: Akun/toko baru sangat mencurigakan.
3.  **📝 `description_length`**: Deskripsi pendek dan kurang detail.
4.  **🚚 `shipping_time_days`**: Waktu pengiriman yang tidak wajar (terlalu lama).
5.  **💳 `payment_methods_count`**: Opsi pembayaran yang terbatas.
6.  **⭐ `seller_reviews` & `seller_rating`**: Ulasan sedikit dan rating buruk.
7.  **📞 `contact_info_complete`**: Informasi kontak tidak lengkap.
8.  **💰 `price` & `is_price_below_avg`**: Harga terlalu murah untuk menjadi kenyataan.
9.  **✍️ `spelling_errors`**: Banyak kesalahan ejaan pada listingan.

### **5. Penjelasan Dukungan AI (AI Support Explanation)**

AI adalah tulang punggung proyek ini, diaplikasikan dalam dua peran yang berbeda namun saling melengkapi:

*   **🤖 AI for Classification:**
    Algoritma *Machine Learning* digunakan untuk tugas prediksi. Walaupun semua model (termasuk **XGBoost, LightGBM**, dll.) menunjukkan performa sempurna, model **Random Forest** (setelah *tuning*) dipilih sebagai model "juara" karena interpretasi *feature importance*-nya yang jelas dan performa yang solid.

*   **🧠 AI for Reasoning & Summarization (IBM Granite):**
    Ini adalah aplikasi AI yang paling inovatif dalam proyek ini. **IBM Granite** tidak digunakan untuk meringkas teks deskripsi. Sebaliknya, ia menerima **input data terstruktur** (angka, kategori, boolean) dan bertugas untuk **melakukan penalaran (reasoning)**. Hasilnya adalah **ringkasan naratif yang menjelaskan FAKTOR-FAKTOR RISIKO** dalam bahasa alami. Ini menunjukkan kemampuan LLM untuk menjelaskan "mengapa" di balik prediksi model ML, sebuah jembatan penting antara data science dan operasional bisnis.

### **6. Cara Menjalankan Proyek**
Untuk mereplikasi hasil analisis dalam proyek ini, ikuti langkah-langkah berikut:

1.  **Clone Repositori**
    ```bash
    git clone https://github.com/LatiefDataVisionary/counterfeit-detection-capstone.git
    cd counterfeit-detection-capstone
    ```
2.  **Buat Virtual Environment (Dianjurkan)**
    ```bash
    python -m venv venv
    source venv/bin/activate  # Untuk Windows: venv\Scripts\activate
    ```
3.  **Instal Dependensi**
    Pastikan Anda sudah memiliki file `requirements.txt` yang berisi semua library yang dibutuhkan.
    ```bash
    pip install -r requirements.txt
    ```
4.  **Jalankan Notebook**
    Buka dan jalankan file notebook yang ada di direktori `/notebooks/` menggunakan Google Colab atau Jupyter Notebook.
