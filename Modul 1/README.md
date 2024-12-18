# learn-flask

## **Modul 1: Flask Fundamentals**

### **1.1 Apa Itu Flask?**
**Flask** adalah sebuah web framework ringan (lightweight) yang digunakan untuk membangun aplikasi web dengan Python. Flask dirancang dengan filosofi minimalis, yaitu memberikan kebebasan kepada developer untuk memilih berbagai komponen aplikasi yang dibutuhkan, seperti template, database, dan autentikasi. Flask sering disebut sebagai **microframework**, karena memiliki sedikit dependensi dan fitur bawaan, namun sangat fleksibel untuk dikembangkan.

#### **Sejarah Flask**
Flask diciptakan oleh **Armin Ronacher**, seorang pengembang perangkat lunak asal Jerman, pada tahun 2010. Flask dikembangkan sebagai alternatif ringan untuk framework Python lainnya, seperti Django, yang lebih besar dan lebih terstruktur. Tujuan utama Flask adalah untuk memberikan framework minimalis dan mudah digunakan yang memungkinkan pengembang untuk menambahkan fungsionalitas sesuai kebutuhan.

Flask menggunakan WSGI (Web Server Gateway Interface) dan Jinja2 sebagai templating engine serta Werkzeug sebagai toolkit untuk menangani request HTTP.

### **1.2 Komponen Utama dalam Flask**

Beberapa komponen utama yang membentuk Flask adalah:

- **Flask Class**: Kelas utama untuk aplikasi Flask. Anda membuat instance dari kelas ini untuk menjalankan aplikasi.
- **Routing**: Menentukan URL dan menghubungkannya dengan fungsi Python yang menangani permintaan.
- **Request dan Response**: Flask memanfaatkan objek `request` untuk menangani data yang dikirimkan oleh pengguna (seperti data form), dan objek `response` untuk mengirimkan balasan ke pengguna.
- **Templates**: Menggunakan Jinja2 untuk merender HTML dinamis dengan data dari aplikasi.
- **Flask Extensions**: Ekstensi yang memungkinkan Anda menambah fungsionalitas ke aplikasi, seperti autentikasi pengguna, form handling, dan lain-lain.

### **1.3 Memulai dengan Flask**

#### **Instalasi Flask**
Untuk mulai menggunakan Flask, pertama-tama Anda harus menginstalnya. Anda bisa menggunakan `pip` untuk menginstalnya:

```bash
pip install flask
```

#### **Membuat Aplikasi Flask Pertama**

Setelah Flask terinstal, Anda bisa membuat aplikasi Flask pertama dengan langkah-langkah berikut:

1. **Buat file Python**  
   Buat file bernama `app.py` dan tambahkan kode berikut:

   ```python
   from flask import Flask

   # Membuat instance dari Flask
   app = Flask(__name__)

   # Mendefinisikan route (endpoint)
   @app.route('/')
   def hello():
       return "Hello, World!"

   if __name__ == "__main__":
       app.run(debug=True)  # Menjalankan aplikasi Flask
   ```

2. **Menjalankan Aplikasi**  
   Jalankan aplikasi dengan perintah:
   ```bash
   python app.py
   ```
   Setelah menjalankan aplikasi, buka browser dan akses `http://127.0.0.1:5000/` untuk melihat aplikasi Flask pertama Anda yang menampilkan teks "Hello, World!".

### **1.4 Routing di Flask**

Flask menggunakan **routing** untuk menentukan bagaimana permintaan HTTP ditangani. Setiap URL yang diakses di aplikasi Flask akan dipetakan ke fungsi tertentu menggunakan decorator `@app.route()`.

#### **Contoh Penggunaan Route**

1. **Route dengan URL sederhana**
   ```python
   @app.route('/')
   def home():
       return "Selamat datang di aplikasi Flask!"
   ```

2. **Route dengan URL dinamis**
   Anda bisa membuat URL dinamis dengan menggunakan parameter dalam URL. Misalnya:
   ```python
   @app.route('/greet/<name>')
   def greet(name):
       return f"Hello, {name}!"
   ```
   Dengan contoh di atas, jika Anda mengakses `http://127.0.0.1:5000/greet/John`, aplikasi akan menampilkan "Hello, John!".

### **1.5 HTTP Methods: GET dan POST**

Flask mendukung berbagai jenis metode HTTP, dua yang paling umum digunakan adalah **GET** dan **POST**.

- **GET**: Digunakan untuk mengambil data dari server. Biasanya digunakan untuk menampilkan informasi kepada pengguna.
- **POST**: Digunakan untuk mengirimkan data ke server, seperti saat mengirimkan data melalui form HTML.

#### **Contoh Penggunaan GET dan POST**

1. **GET Method**  
   Berikut adalah route yang menangani metode GET untuk menampilkan data:
   ```python
   @app.route('/greet', methods=['GET'])
   def greet():
       return "Greetings from Flask!"
   ```

2. **POST Method**  
   Untuk menangani data yang dikirim oleh form HTML, Anda bisa menggunakan metode POST:
   ```python
   @app.route('/submit', methods=['POST'])
   def submit():
       return 'Data telah disubmit!'
   ```

### **1.6 Rendering Templates dengan Jinja2**

Flask memungkinkan Anda untuk merender HTML menggunakan **Jinja2**, yang merupakan templating engine. Jinja2 memungkinkan Anda untuk membuat template HTML yang dinamis, yaitu template yang dapat menerima data dari aplikasi dan menampilkannya secara dinamis.

1. **Membuat Template HTML**

   Pertama, buat folder bernama `templates` di direktori yang sama dengan file `app.py`. Di dalam folder `templates`, buat file `index.html` dengan kode berikut:
   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>Flask Template</title>
   </head>
   <body>
       <h1>Hello, {{ name }}!</h1>
   </body>
   </html>
   ```

2. **Merender Template di Flask**
   Gunakan fungsi `render_template()` untuk merender template dan mengirimkan data ke dalam template.

   ```python
   from flask import render_template

   @app.route('/greet/<name>')
   def greet(name):
       return render_template('index.html', name=name)
   ```

   Ketika Anda mengakses `http://127.0.0.1:5000/greet/John`, template `index.html` akan dirender dan menampilkan "Hello, John!".

---

### **Kesimpulan**

Pada modul pertama ini, kita telah membahas:
- Apa itu Flask dan sejarahnya
- Cara menginstal dan membuat aplikasi Flask sederhana
- Penggunaan route dan HTTP methods
- Cara merender template HTML menggunakan Jinja2