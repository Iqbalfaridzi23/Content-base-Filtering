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
- movie.csv: Metadata film (movieId, title, genres), Jumlah baris dan kolom: 27278 baris × 3 kolom, Kolom: `movieId`, `title`, `genres`, dan Missing value: Tidak ditemukan missing value (berdasarkan `genome_tags.isnull().sum()`) di notebook
- genome_tags.csv: Kumpulan tag yang digunakan, Jumlah baris dan kolom: 1128 baris × 2 kolom, Kolom: `tagId`, `tag`, Missing value: Tidak ditemukan missing value (berdasarkan `genome_tags.isnull().sum()`)
- genome_scores.csv: Skor relevansi (0–1) antara film dan tag, Jumlah baris dan kolom: 11709768 baris × 3 kolom, Kolom: `movieId`, `tagId`, `relevance`, Missing value: Tidak ditemukan missing value (berdasarkan `genome_scores.isnull().sum()`)

Variabel penting:
- movieId: ID unik untuk setiap film (digunakan sebagai penghubung antara file movies.csv, genome_scores.csv, dan ratings.csv)
- title: Judul film beserta tahun rilis
- genres : Daftar genre film dalam format string yang dipisahkan oleh karakter '|', misalnya 'Action|Adventure|Sci-Fi'. Jika tidak diketahui, genre bernilai '(no genres listed)'.
- tag: Kata atau frasa deskriptif yang merepresentasikan aspek dari film, digunakan untuk menilai keterkaitan dengan suatu konten.
- tagId: ID unik dari sebuah tag, digunakan untuk menghubungkan antara file genome_tags.csv dan genome_scores.csv.
- relevance: Nilai numerik antara 0 sampai 1 yang menunjukkan seberapa relevan suatu tag dengan film tertentu (semakin tinggi nilainya, semakin relevan).


## Data Preparation
Pada tahap ini dilakukan beberapa proses persiapan data sebelum dimasukkan ke dalam model:
- Cek Missing Value
- Menggabungkan data genome_scores.csv dengan genome_tags.csv
- Menyusun 20 tag paling relevan untuk setiap film
- Membuat satu representasi teks (gabungan tag) untuk setiap film
- Melakukan vektorisasi teks menggunakan TfidfVectorizer dari scikit-learn
- Membuat kolom genre_set untuk keperluan evaluasi

## Modeling
Model rekomendasi dibuat berdasarkan cosine similarity antar film berdasarkan representasi tag-nya.

Contoh fungsi rekomendasi:

![image](https://github.com/user-attachments/assets/ea77349c-f29a-4119-b9d0-9da225634f27)


Output:
- Sistem akan mengembalikan 5 film dengan konten paling mirip berdasarkan tag.

### Cosine Similarity
Cosine Similarity adalah metode yang digunakan untuk mengukur kemiripan antara dua vektor dalam ruang berdimensi tinggi, berdasarkan sudut di antara mereka.
Dalam konteks sistem rekomendasi berbasis konten (Content-Based Filtering), cosine similarity digunakan untuk menghitung seberapa mirip suatu film dengan film lainnya berdasarkan fitur yang sudah diekstrak (misalnya: genre atau tag relevance). Semakin besar nilai cosine similarity (mendekati 1), semakin mirip dua item tersebut.


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
  - Average Precision@5 (berdasarkan kemiripan genre): 0.8520
    
![image](https://github.com/user-attachments/assets/7d807aa7-a3ed-4da9-adb5-d8a4c47cd825)

Artinya, rata-rata sekitar 85% dari film yang direkomendasikan memiliki kesamaan genre dengan film input, yang menunjukkan bahwa sistem rekomendasi cukup akurat dalam menyarankan film-film serupa.

### Contoh Rekomendasi
Berikut contoh hasil rekomendasi dari sistem:

![image](https://github.com/user-attachments/assets/b120ae50-7029-46f5-bbe9-5d651156682a)

