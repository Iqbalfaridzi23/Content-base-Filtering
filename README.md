# Laporan Proyek Machine Learning - Iqbal Alfaridzi Balman
## Domain Proyek

Dalam era digital saat ini, jumlah film yang tersedia secara online semakin meningkat drastis. Hal ini menyulitkan pengguna untuk menemukan film yang sesuai dengan preferensi mereka. Sistem rekomendasi hadir sebagai solusi yang efektif dalam menyaring dan menyajikan konten yang relevan bagi pengguna.

Proyek ini bertujuan untuk membangun sistem rekomendasi film berbasis konten (Content-Based Filtering) yang dapat memberikan rekomendasi film berdasarkan kemiripan konten, seperti tag deskriptif dari film tersebut. Dataset yang digunakan adalah MovieLens, salah satu dataset paling populer untuk eksperimen sistem rekomendasi.

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
Dalam pendekatan Content-Based Filtering, sistem merekomendasikan item (film) yang mirip secara konten dengan item yang disukai pengguna.
Untuk itu, evaluasi dilakukan secara kualitatif, dengan cara:
- Memilih beberapa judul film dari dataset.
- Melihat hasil rekomendasi top-5 untuk masing-masing film.
- Menilai apakah film-film yang direkomendasikan memiliki kemiripan konten (genre, tema, atau tag) dengan film input.

![image](https://github.com/user-attachments/assets/b120ae50-7029-46f5-bbe9-5d651156682a)

