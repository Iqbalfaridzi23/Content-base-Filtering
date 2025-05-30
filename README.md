# Laporan Proyek Machine Learning - Iqbal Alfaridzi Balman
## Domain Proyek

Dalam era digital saat ini, jumlah film yang tersedia secara online meningkat secara signifikan, yang menyebabkan kesulitan bagi pengguna dalam menemukan film sesuai preferensi mereka. Menurut Harper dan Konstan (2015), tantangan dalam menyaring dan menyajikan konten yang relevan telah menjadi salah satu fokus utama dalam pengembangan sistem rekomendasi film. Sistem rekomendasi hadir sebagai solusi efektif untuk mengatasi masalah ini dengan memanfaatkan informasi pengguna dan karakteristik item (film) guna memberikan saran yang dipersonalisasi.

Proyek ini bertujuan untuk membangun sistem rekomendasi film berbasis konten (Content-Based Filtering), yang dapat memberikan rekomendasi film berdasarkan kemiripan konten, seperti tag deskriptif dari film tersebut. Dataset yang digunakan adalah MovieLens 20M, salah satu dataset paling populer dalam penelitian sistem rekomendasi, yang disediakan oleh GroupLens Research (Harper & Konstan, 2015). Dataset ini tersedia secara publik melalui platform Kaggle dan mencakup berbagai metadata film serta skor relevansi terhadap tag deskriptif.

Referensi :
- "F. Maxwell Harper and Joseph A. Konstan. "The MovieLens Datasets: History and Context." ACM Transactions on Interactive Intelligent Systems (TiiS) 5, 4 (2015)."
- Dataset: Kaggle - MovieLens 20M Dataset

## Business Understanding

### Problem Statements

- Bagaimana cara memberikan rekomendasi film kepada pengguna baru tanpa data interaksi historis?
- Bagaimana sistem dapat mengenali kesamaan antar film meskipun belum pernah diberi rating oleh pengguna yang sama?

### Goals

- Membangun sistem rekomendasi film yang mampu memberikan rekomendasi hanya berdasarkan konten film.
- Menyediakan fungsi rekomendasi film yang dapat menerima input berupa judul film dan menghasilkan daftar film serupa secara konten.

### Solution statements
- Menggunakan representasi konten film dari tag relevansi (genome_scores) dan menghitung kemiripan menggunakan TF-IDF + cosine similarity.
- Memanfaatkan informasi genre atau metadata lain sebagai fitur tambahan untuk memperkaya representasi konten film.

## Data Understanding
Dataset yang digunakan berasal dari Kaggle - MovieLens(https://www.kaggle.com/datasets/grouplens/movielens-20m-dataset) yang terdiri dari beberapa file utama:
- movie.csv: Metadata film (movieId, title, genres)
- genome_tags.csv: Kumpulan tag yang digunakan
- genome_scores.csv: Skor relevansi (0–1) antara film dan tag

Variabel penting:
- movieId: ID unik untuk setiap film
- title: Judul film
- tag: Tag deskriptif
- relevance: Nilai yang menunjukkan seberapa kuat hubungan antara tag dan film


## Data Preparation
Pada tahap ini dilakukan beberapa proses persiapan data sebelum dimasukkan ke dalam model:
- Cek Missing Value
- Menggabungkan data genome_scores.csv dengan genome_tags.csv
- Menyusun 20 tag paling relevan untuk setiap film
- Membuat satu representasi teks (gabungan tag) untuk setiap film
- Melakukan vektorisasi teks menggunakan TfidfVectorizer dari scikit-learn

## Modeling
Model rekomendasi dibuat berdasarkan cosine similarity antar film berdasarkan representasi tag-nya.

Contoh fungsi rekomendasi:

![image](https://github.com/user-attachments/assets/dd71ecc9-fd14-4a15-be25-11681a08bce9)

Output:
- Sistem akan mengembalikan 5 film dengan konten paling mirip berdasarkan tag.


## Evaluation
Evaluasi sistem rekomendasi dilakukan untuk mengukur seberapa relevan rekomendasi yang dihasilkan berdasarkan kesamaan genre antara film input dan film yang direkomendasikan. Mengingat pendekatan yang digunakan adalah Content-Based Filtering, maka evaluasi dilakukan dengan mengukur genre overlap antar film.

### Metodologi Evaluasi
Metode yang digunakan adalah Precision@K, yaitu proporsi film yang direkomendasikan (top-K) yang memiliki kemiripan genre dengan film input. Langkah-langkah evaluasi:
- Representasi genre dari setiap film diubah menjadi himpunan (set) untuk memudahkan perbandingan.
- Dipilih 100 sampel film secara acak dari dataset sebagai data uji.
- Untuk setiap film:
    - Diambil 5 rekomendasi teratas (top-5) dari sistem.
    - Dihitung berapa banyak rekomendasi yang memiliki genre yang sama dengan film input.
    - Precision dihitung sebagai rasio jumlah film yang mirip secara genre terhadap jumlah top-K (dalam hal ini K=5).
- Precision dari seluruh sampel dirata-ratakan untuk mendapatkan nilai rata-rata (average precision).

### Hasil Evaluasi
Hasil evaluasi dengan 100 sampel film acak dan top-5 rekomendasi menghasilkan nilai:
  - Average Precision@5 (berdasarkan kemiripan genre): 0.8528
    
![image](https://github.com/user-attachments/assets/7d807aa7-a3ed-4da9-adb5-d8a4c47cd825)

Artinya, rata-rata sekitar 85% dari film yang direkomendasikan memiliki kesamaan genre dengan film input, yang menunjukkan bahwa sistem rekomendasi cukup akurat dalam menyarankan film-film serupa.

### Contoh Rekomendasi
Berikut contoh hasil rekomendasi dari sistem:

![image](https://github.com/user-attachments/assets/b120ae50-7029-46f5-bbe9-5d651156682a)

