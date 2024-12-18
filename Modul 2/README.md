## Modul 2: Routing, HTTP Request & Response, dan URL Parameters

#### 2.1 Routing dan HTTP Methods

Flask memungkinkan kita untuk mendefinisikan rute aplikasi menggunakan dekorator `@app.route()`. Selain itu, kita juga dapat menentukan metode HTTP yang digunakan untuk suatu rute, seperti `GET`, `POST`, `PUT`, atau `DELETE`.

**Contoh Routing dengan Metode HTTP:**
```python
from flask import Flask

app = Flask(__name__)

# Route dengan metode GET (default)
@app.route('/')
def home():
    return "Hello, this is the Home page!"

# Route dengan metode POST
@app.route('/submit', methods=['POST'])
def submit():
    return "Form submitted successfully!"

if __name__ == '__main__':
    app.run(debug=True)
```

**Penjelasan:**
- Rute `/` akan menangani permintaan dengan metode `GET` secara default.
- Rute `/submit` hanya akan menangani permintaan dengan metode `POST`.

#### 2.2 HTTP Request

Flask menyediakan objek `request` untuk mengakses data yang dikirimkan oleh klien melalui HTTP request. Data tersebut bisa berupa data formulir (form), data JSON, file, atau parameter URL.

**Contoh Mengakses Data Formulir (POST):**
```python
from flask import Flask, request

app = Flask(__name__)

@app.route('/submit', methods=['POST'])
def submit():
    name = request.form['name']
    email = request.form['email']
    return f"Name: {name}, Email: {email}"

if __name__ == '__main__':
    app.run(debug=True)
```

**Penjelasan:**
- `request.form` digunakan untuk mengambil data dari formulir HTML yang dikirimkan dengan metode `POST`.

#### 2.3 URL Parameters

Kadang-kadang kita perlu menangani data yang dikirimkan melalui URL. Flask memungkinkan kita untuk menangani parameter URL menggunakan sintaks `<parameter>` di dalam rute.

**Contoh Menangani URL Parameter:**
```python
from flask import Flask

app = Flask(__name__)

@app.route('/greet/<name>')
def greet(name):
    return f"Hello, {name}!"

if __name__ == '__main__':
    app.run(debug=True)
```

**Penjelasan:**
- Pada rute `/greet/<name>`, `<name>` adalah parameter URL yang akan diteruskan ke fungsi `greet()`.
- Jika kamu mengunjungi `http://127.0.0.1:5000/greet/Alice`, maka outputnya adalah `Hello, Alice!`.

#### 2.4 Menggunakan Jenis Parameter URL yang Lain

Flask memungkinkan kita untuk menetapkan jenis parameter yang lebih spesifik, seperti integer, string, atau path.

**Contoh Parameter dengan Tipe Spesifik:**
```python
from flask import Flask

app = Flask(__name__)

# Integer parameter
@app.route('/user/<int:id>')
def user(id):
    return f"User ID is {id}"

# String parameter (default, tapi eksplisit)
@app.route('/product/<string:name>')
def product(name):
    return f"Product name is {name}"

# Path parameter (mengizinkan slashes di dalam parameter)
@app.route('/files/<path:filename>')
def files(filename):
    return f"File name is {filename}"

if __name__ == '__main__':
    app.run(debug=True)
```

**Penjelasan:**
- `<int:id>`: Parameter `id` harus berupa integer.
- `<string:name>`: Parameter `name` harus berupa string.
- `<path:filename>`: Parameter `filename` memungkinkan pengambilan path yang mengandung tanda garis miring `/`.

#### 2.5 HTTP Response

Flask juga memberikan kita kemampuan untuk mengontrol respons yang dikirimkan ke klien. Fungsi rute dapat mengembalikan string, objek `Response`, atau template HTML yang dirender.

**Contoh Menggunakan Response:**
```python
from flask import Flask, Response

app = Flask(__name__)

@app.route('/')
def home():
    return Response("Custom response text", status=200)

if __name__ == '__main__':
    app.run(debug=True)
```

**Penjelasan:**
- Di sini kita membuat respons kustom menggunakan objek `Response`, yang memungkinkan kita mengatur status HTTP secara eksplisit dan menentukan isi respons.

#### 2.6 Redirect dan URL For

Flask juga menyediakan dua fungsi penting untuk mengelola pengalihan (redirect) dan URL dinamis: `redirect()` dan `url_for()`.

**Contoh Redirect:**
```python
from flask import Flask, redirect, url_for

app = Flask(__name__)

@app.route('/')
def home():
    return redirect(url_for('greet', name='Alice'))

@app.route('/greet/<name>')
def greet(name):
    return f"Hello, {name}!"

if __name__ == '__main__':
    app.run(debug=True)
```

**Penjelasan:**
- Fungsi `redirect()` digunakan untuk mengarahkan pengguna ke rute lain.
- Fungsi `url_for()` digunakan untuk menghasilkan URL dinamis berdasarkan nama rute.

#### 2.7 Kesimpulan Modul 2
Pada modul ini, kita telah:
- Mempelajari routing dan metode HTTP (`GET`, `POST`).
- Mengakses data dari HTTP request, seperti data formulir.
- Menangani URL parameter menggunakan Flask.
- Membuat respons HTTP kustom.
- Menggunakan pengalihan dan URL dinamis.

### Homepage
[Back to main](https://github.com
rohmanurnr/learn-flask/tree/main)om/### Homepage
[Back to main](https://github.com
rohmanurnr/learn-flask/tree/main)