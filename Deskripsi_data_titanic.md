# Deskripsi Variabel Baru di Data Titanic
Tiga variabel baru: `family_size`, `is_alone`, and `title`.

---

### 1. `family_size`

* **Deskripsi**: Variabel ini menunjukkan jumlah total anggota keluarga seorang penumpang di kapal, ditambah dengan penumpang itu sendiri.

* **Metode Pembuatan**: Variabel ini dibuat dengan menjumlahkan nilai dari kolom `sibsp` (jumlah saudara kandung/pasangan) dan `parch` (jumlah orang tua/anak), lalu menambahkan 1 untuk menghitung penumpang itu sendiri.

    ```python
    titanic['family_size'] = titanic['sibsp'] + titanic['parch'] + 1
    ```

---

### 2. `is_alone` (Bepergian Sendiri)

* **Deskripsi**: Ini adalah variabel biner yang menunjukkan apakah seorang penumpang bepergian sendiri atau bersama keluarga. Nilai `1` berarti penumpang tersebut sendirian, dan `0` berarti mereka bersama setidaknya satu anggota keluarga.

* **Metode Pembuatan**: Variabel ini diturunkan langsung dari fitur `family_size`. Jika `family_size` bernilai 1, maka `is_alone` diatur menjadi 1; jika tidak, nilainya adalah 0.

    ```python
    titanic['is_alone'] = 0
    titanic.loc[titanic['family_size'] == 1, 'is_alone'] = 1
    ```

---
