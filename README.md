# AI-Powered Counterfeit Detection and Risk Summarization

**Capstone Project untuk Student Development Initiative (IBM & Hacktiv8)**
**Oleh: [NAMA LENGKAP ANDA]**

* * *

### **1. Ringkasan Proyek (Project Overview)**

Proyek ini dibangun untuk mengatasi tantangan signifikan dalam ekosistem e-commerce: proliferasi produk palsu. Dengan menganalisis data transaksi dan atribut produk secara cermat, proyek ini menghadirkan solusi cerdas yang menggabungkan kekuatan Machine Learning tradisional dengan kemampuan Large Language Model (LLM) modern untuk:

1.  **Klasifikasi Produk:** Mengembangkan model prediktif yang akurat untuk mengidentifikasi listingan produk yang berpotensi palsu. Ini memungkinkan platform e-commerce untuk secara proaktif menandai atau menghapus produk berisiko sebelum mencapai konsumen.
2.  **Ringkasan Risiko Cerdas:** Memanfaatkan model bahasa besar (LLM) **IBM Granite** untuk mengubah data terstruktur (statistik, kategori, dll.) menjadi narasi ringkas yang menjelaskan alasan di balik prediksi "palsu". Ini memberikan konteks yang kaya bagi tim *Trust & Safety* untuk mempercepat investigasi dan pengambilan keputusan.

Sistem ini bertujuan untuk meningkatkan kepercayaan konsumen, melindungi reputasi penjual asli, dan menciptakan lingkungan belanja online yang lebih aman.

* * *

### **2. Sumber Data (Raw Dataset)**

*   **Nama Dataset:** Counterfeit Product Detection Dataset
*   **Sumber:** [Kaggle](https://www.kaggle.com/datasets/aimlveera/counterfeit-product-detection-dataset)
*   **Deskripsi:** Dataset ini adalah simulasi data listingan produk dari platform e-commerce, terdiri dari 5000 baris data dengan 27 kolom yang mencakup berbagai atribut seperti harga produk, rating dan ulasan penjual, jumlah gambar, panjang deskripsi, waktu pengiriman, asal pengiriman, metrik interaksi pembeli (views, purchases, wishlist adds), informasi penjual (usia domain, kelengkapan kontak, kebijakan pengembalian, metode pembayaran), serta indikator lain yang terkait dengan potensi pemalsuan (kesalahan ejaan, pola pembayaran aneh, ketidaksesuaian lokasi IP, sertifikasi, garansi, pesanan massal). Kolom target (`is_counterfeit`) adalah variabel boolean yang menandai apakah produk tersebut asli (False) atau palsu (True).
*   File dataset mentah asli dapat ditemukan di direktori: `/data/raw/`

* * *

### **3. Alur Analisis & Metodologi**

Proyek ini mengikuti alur kerja (pipeline) data science yang sistematis, terbagi menjadi fase-fase berikut:

1.  **Fase 1: Pembersihan Data & Feature Engineering:**
    *   Melakukan standardisasi data kategorikal, khususnya nama brand, untuk memastikan konsistensi.
    *   Menangani potensi outlier pada fitur-fitur numerik menggunakan metode capping berbasis IQR.
    *   Mengkonversi kolom tanggal (`listing_date`) ke format datetime dan membuat fitur turunan seperti `listing_age_days`.
    *   Menciptakan fitur-fitur baru yang relevan seperti `purchase_to_view_ratio` dan indikator harga relatif terhadap rata-rata kategori (`is_price_below_avg`).
    *   Memeriksa missing values dan duplikat data untuk memastikan kualitas dataset.

2.  **Fase 2: Analisis Data Eksploratif (EDA):**
    *   Menganalisis distribusi kelas target (`is_counterfeit`) dan mengidentifikasi adanya ketidakseimbangan kelas.
    *   Memvisualisasikan hubungan antara fitur-fitur kunci (harga, rating penjual, asal pengiriman, dll.) dengan status keaslian produk menggunakan boxplot, histogram, dan bar chart.
    *   Mengeksplorasi korelasi antar fitur numerik melalui heatmap.
    *   Menganalisis hubungan fitur kategorikal/boolean dengan target melalui crosstab dan visualisasi proporsi untuk mengungkap pola diskriminatif.

3.  **Fase 3: Pemodelan & Perbandingan Model (Classification):**
    *   Mempersiapkan data untuk pemodelan termasuk pemisahan fitur (X) dan target (y).
    *   Menerapkan preprocessing (StandardScaler untuk numerik, OneHotEncoder untuk kategorikal/boolean) menggunakan ColumnTransformer.
    *   Membagi data menjadi training dan testing set dengan stratifikasi untuk mempertahankan distribusi kelas.
    *   Membangun dan melatih enam model klasifikasi populer: **Logistic Regression, K-Nearest Neighbors (KNN), Support Vector Machine (SVM), Random Forest, XGBoost, dan LightGBM.**
    *   Mengevaluasi performa setiap model pada data uji menggunakan metrik komprehensif seperti Akurasi, Presisi, Recall, F1-Score, Confusion Matrix, ROC AUC, Balanced Accuracy, dan Precision-Recall AUC.
    *   Melakukan Cross-Validation untuk mendapatkan estimasi performa model yang lebih robust.
    *   Melakukan tuning hyperparameter (menggunakan GridSearchCV) pada model Random Forest untuk mengoptimalkan performa.
    *   Menganalisis *Feature Importance* dari model terbaik (Random Forest) untuk mengidentifikasi prediktor paling berpengaruh.
    *   Menyimpan model terbaik menggunakan `joblib` untuk penggunaan di masa depan.

4.  **Fase 4: Integrasi IBM Granite (Summarization Cerdas):**
    *   Mengintegrasikan model bahasa besar IBM Granite (via Replicate dan Langchain) ke dalam alur kerja.
    *   Mengembangkan prompt template yang memberikan konteks proyek kepada LLM.
    *   Membuat fungsi query untuk mengirim data terstruktur produk yang terdeteksi palsu ke IBM Granite.
    *   IBM Granite bertugas untuk melakukan *reasoning* berdasarkan data input dan menghasilkan **ringkasan naratif yang menjelaskan mengapa produk tersebut dianggap berisiko palsu**, alih-alih hanya meringkas teks deskripsi produk. Ini merupakan demonstrasi aplikasi LLM yang canggih untuk analisis dan penjelasan data terstruktur.

* * *

### **4. Insight & Temuan Utama (Key Insights & Findings)**

Analisis data dan hasil pemodelan mengungkap beberapa temuan kunci yang sangat diskriminatif dalam membedakan produk asli dan palsu pada dataset ini:

*   **Indikator Terkuat dari Feature Importance:** Berdasarkan analisis *feature importance* dari model Random Forest terbaik (atau model berbasis tree lainnya yang menunjukkan performa tinggi), fitur-fitur yang paling berkontribusi dalam prediksi adalah:
    1.  **`product_images`**: Jumlah gambar produk yang sedikit (terutama hanya 1 atau 2) sangat terkait dengan produk palsu.
    2.  **`domain_age_days`**: Penjual produk palsu cenderung memiliki domain atau akun yang baru dibuat (usia domain rendah).
    3.  **`description_length`**: Deskripsi produk palsu umumnya jauh lebih pendek dan kurang detail.
    4.  **`shipping_time_days`**: Waktu pengiriman yang sangat lama adalah indikator kuat produk palsu.
    5.  **`payment_methods_count`**: Jumlah metode pembayaran yang ditawarkan oleh penjual palsu cenderung terbatas.
    6.  **`seller_reviews` & `seller_rating`**: Penjual produk palsu hampir selalu memiliki jumlah ulasan yang sangat sedikit dan rating yang rendah.
    7.  **`contact_info_complete`**: Penjual asli memiliki informasi kontak yang lengkap, sementara penjual palsu cenderung tidak.
    8.  **`price` & `is_price_below_avg`**: Harga produk palsu secara signifikan lebih rendah dan seringkali berada di bawah ambang batas tertentu dari harga rata-rata di kategorinya.
    9.  **`spelling_errors`**: Deskripsi produk palsu cenderung memiliki lebih banyak kesalahan ejaan.

*   **Performa Model yang Sempurna:** Seluruh model klasifikasi yang diuji, termasuk Logistic Regression, KNN, SVM, Random Forest, XGBoost, dan LightGBM, mencapai **akurasi, presisi, recall, F1-score, AUC ROC, Balanced Accuracy, dan Precision-Recall AUC sebesar 1.0000 (100%)** pada data uji. Hasil ini dikonfirmasi melalui cross-validation yang juga menunjukkan skor sempurna. Performa yang luar biasa ini menunjukkan bahwa fitur-fitur dalam dataset ini sangat kuat dan diskriminatif, memungkinkan pemisahan kelas yang hampir sempurna.

*   **Ketidakseimbangan Kelas:** Dataset menunjukkan adanya ketidakseimbangan kelas, dengan sekitar 70.6% produk asli dan 29.4% produk palsu. Meskipun model mencapai performa sempurna pada dataset ini (yang mungkin mengindikasikan fitur yang sangat jelas atau potensi data leakage dalam pembuatan dataset simulasi), dalam skenario dunia nyata dengan dataset yang lebih kompleks dan tidak seimbang, fokus pada metrik seperti F1-score, Recall, dan Precision-Recall AUC akan sangat krusial untuk memastikan model efektif dalam mendeteksi kelas minoritas (produk palsu).

* * *

### **5. Penjelasan Dukungan AI (AI Support Explanation)**

Penerapan AI dalam proyek ini melampaui klasifikasi standar untuk mendemonstrasikan pemanfaatan LLM secara inovatif:

*   **AI for Classification:** Berbagai algoritma *Machine Learning* (Logistic Regression, KNN, SVM, Random Forest, XGBoost, LightGBM) dilatih untuk memprediksi status keaslian produk (`is_counterfeit`). Model **Random Forest** (setelah tuning hyperparameter) dipilih sebagai model "juara" karena performanya yang konsisten dan kemampuannya menyediakan *feature importance* yang informatif, meskipun semua model menunjukkan performa sempurna pada dataset ini.
*   **AI for Summarization (IBM Granite):** Model bahasa besar **IBM Granite** (diakses melalui API Replicate dan diatur dengan Langchain) digunakan untuk tugas *summarization* yang unik. Alih-alih meringkas teks naratif, Granite menerima input berupa **data terstruktur** (nilai numerik dan kategorikal dari fitur-fitur produk yang terdeteksi palsu). Berdasarkan input data tersebut, Granite menghasilkan **ringkasan naratif dalam bahasa alami** yang menjelaskan **mengapa** produk tersebut dicurigai palsu, menyoroti faktor-faktor risiko utama (misalnya, "harga terlalu rendah", "rating penjual buruk", "waktu pengiriman lama"). Ini menunjukkan kemampuan LLM untuk melakukan *reasoning* atas data kuantitatif/kategorikal dan mengkomunikasikan insight tersebut secara efektif, yang merupakan aplikasi powerful untuk menjelaskan hasil model ML kepada pengguna non-teknis (misalnya, tim operasional atau *fraud analyst*).

* * *

### **6. Cara Menjalankan Proyek**

Untuk mereproduksi analisis dan hasil dalam proyek ini:

1.  *Clone* repositori ini.
2.  Pastikan Anda memiliki Python 3.x.
3.  Instal semua dependensi yang dibutuhkan:
    ```bash
    pip install -r requirements.txt
    ```
4.  Buka dan jalankan notebook di `/notebooks/notebook.ipynb` menggunakan Google Colab atau Jupyter Notebook.
