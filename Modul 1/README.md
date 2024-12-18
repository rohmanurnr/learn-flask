### Modul 1: Memulai dengan Flask

#### 1.1 Apa itu Flask?

Flask adalah framework web minimalis dan ringan untuk Python. Flask memungkinkan kita untuk membuat aplikasi web dengan cepat dan mudah. Flask tidak mengatur banyak hal secara otomatis, yang memberi kita kebebasan untuk menyesuaikan sesuai kebutuhan proyek.

#### 1.2 Instalasi Flask

Untuk menginstal Flask, kita harus terlebih dahulu mengatur lingkungan Python dan menginstal Flask menggunakan **pip**.

**Langkah 1: Membuat Virtual Environment**
1. Di terminal, navigasi ke folder proyekmu.
2. Jalankan perintah berikut untuk membuat virtual environment:
   ```
   python -m venv venv
   ```
3. Aktifkan virtual environment:
   - **Windows**:
     ```
     venv\Scripts\activate
     ```
   - **Mac/Linux**:
     ```
     source venv/bin/activate
     ```

**Langkah 2: Instal Flask**
Dengan virtual environment aktif, instal Flask menggunakan pip:
```
pip install flask
```

#### 1.3 Struktur Proyek Flask

Untuk membuat aplikasi Flask, kita akan mengikuti struktur direktori dasar seperti berikut:

```
my_flask_app/
│
├── app.py          # File utama aplikasi
├── templates/      # Folder untuk template HTML
├── static/         # Folder untuk file statis (CSS, JS, gambar)
```

- **`app.py`**: Tempat logika aplikasi Flask ditulis.
- **`templates/`**: Tempat untuk file HTML.
- **`static/`**: Tempat untuk file CSS, JavaScript, atau gambar yang digunakan aplikasi.

#### 1.4 Membuat Aplikasi Flask Pertama

Sekarang kita akan membuat aplikasi Flask pertama kita.

1. **Buat file `app.py`** dengan isi berikut:
   ```python
   from flask import Flask

   app = Flask(__name__)

   @app.route('/')
   def home():
       return "Hello, World!"

   if __name__ == '__main__':
       app.run(debug=True)
   ```

2. **Penjelasan Kode**:
   - `Flask(__name__)`: Membuat objek Flask. `__name__` memberitahu Flask di mana mencari file aplikasi.
   - `@app.route('/')`: Mendefinisikan route untuk halaman utama (URL root `/`).
   - `home()`: Fungsi yang dijalankan ketika pengunjung mengakses root URL.
   - `app.run(debug=True)`: Menjalankan server dalam mode debug untuk memudahkan pengembangan.

3. **Menjalankan Aplikasi**:
   Jalankan aplikasi dengan perintah:
   ```
   python app.py
   ```

   Buka browser dan kunjungi `http://127.0.0.1:5000/`. Kamu akan melihat teks "Hello, World!" di layar.

#### 1.5 Menambahkan Template HTML

Sekarang kita akan menggunakan template HTML untuk menampilkan halaman yang lebih menarik.

1. Buat folder `templates` di dalam proyekmu, lalu buat file `index.html` di dalamnya dengan isi berikut:
   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>Flask App</title>
   </head>
   <body>
       <h1>Welcome to My Flask App!</h1>
       <p>This is a simple web page served by Flask.</p>
   </body>
   </html>
   ```

2. Ubah kode di `app.py` untuk merender template HTML:
   ```python
   from flask import Flask, render_template

   app = Flask(__name__)

   @app.route('/')
   def home():
       return render_template('index.html')

   if __name__ == '__main__':
       app.run(debug=True)
   ```

3. Sekarang, jika kamu mengunjungi `http://127.0.0.1:5000/`, kamu akan melihat halaman HTML yang lebih menarik daripada sebelumnya.

#### 1.6 Kesimpulan Modul 1
Di modul ini, kita telah:
- Mengenal Flask dan bagaimana menginstalnya.
- Membuat aplikasi Flask pertama yang menampilkan "Hello, World!".
- Menggunakan template HTML untuk merender halaman web.

[Back to main](https://github.com/rohmanurnr/learn-flask/tree/main)