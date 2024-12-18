### Modul 3: Form Handling, Form Validation, dan Menggunakan Template Jinja2

#### 3.1 Menggunakan Form di Flask

Formulir HTML memungkinkan pengguna untuk mengirimkan data ke server. Flask dapat menangani pengiriman data formulir menggunakan metode `POST`. Untuk membuat formulir, kita biasanya menggunakan HTML, dan untuk memproses data yang dikirimkan, kita akan menangani request di Flask.

**Contoh Membuat Formulir di Flask:**

1. Buat file `templates/form.html` yang berisi formulir HTML:
   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>Form Example</title>
   </head>
   <body>
       <h1>Submit Your Information</h1>
       <form action="/submit" method="POST">
           <label for="name">Name:</label>
           <input type="text" id="name" name="name" required><br><br>
           <label for="email">Email:</label>
           <input type="email" id="email" name="email" required><br><br>
           <button type="submit">Submit</button>
       </form>
   </body>
   </html>
   ```

2. Buat route di `app.py` untuk menampilkan dan menangani formulir:
   ```python
   from flask import Flask, render_template, request

   app = Flask(__name__)

   # Route untuk menampilkan formulir
   @app.route('/')
   def index():
       return render_template('form.html')

   # Route untuk menangani data yang dikirimkan melalui POST
   @app.route('/submit', methods=['POST'])
   def submit():
       name = request.form['name']
       email = request.form['email']
       return f"Name: {name}, Email: {email}"

   if __name__ == '__main__':
       app.run(debug=True)
   ```

**Penjelasan:**
- Pada rute `/`, kita menampilkan formulir HTML dengan menggunakan template.
- Ketika formulir disubmit, data akan dikirimkan menggunakan metode `POST` ke rute `/submit`, yang kemudian memproses data formulir.

#### 3.2 Validasi Formulir

Validasi formulir penting untuk memastikan bahwa data yang diterima dari pengguna sesuai dengan harapan kita (misalnya, memastikan email valid, nama tidak kosong, dll.).

**Contoh Validasi Sederhana:**
```python
from flask import Flask, render_template, request, redirect, url_for

app = Flask(__name__)

@app.route('/')
def index():
    return render_template('form.html')

@app.route('/submit', methods=['POST'])
def submit():
    name = request.form['name']
    email = request.form['email']

    # Validasi
    if not name or not email:
        return "Name and Email are required!", 400  # Menampilkan pesan kesalahan jika data kosong
    if '@' not in email:
        return "Invalid email address!", 400  # Memeriksa format email

    return f"Name: {name}, Email: {email}"

if __name__ == '__main__':
    app.run(debug=True)
```

**Penjelasan:**
- Validasi dilakukan untuk memastikan bahwa nama dan email tidak kosong, serta email memiliki format yang valid (mengandung karakter `@`).
- Jika data tidak valid, aplikasi akan menampilkan pesan kesalahan dan mengembalikan status HTTP 400.

#### 3.3 Menggunakan Flask-WTF untuk Validasi Lebih Lanjut

Flask-WTF adalah ekstensi Flask yang menyediakan cara mudah untuk menangani formulir, validasi, dan CSRF protection (Cross-Site Request Forgery). Flask-WTF menggunakan **WTForms**, yang menyederhanakan pembuatan dan validasi formulir.

**Instalasi Flask-WTF:**
```bash
pip install flask-wtf
```

**Contoh Penggunaan Flask-WTF:**
```python
from flask import Flask, render_template, redirect, url_for
from flask_wtf import FlaskForm
from wtforms import StringField, EmailField
from wtforms.validators import InputRequired, Email

app = Flask(__name__)
app.config['SECRET_KEY'] = 'mysecretkey'  # Untuk CSRF protection

# Definisikan Formulir dengan Flask-WTF
class ContactForm(FlaskForm):
    name = StringField('Name', validators=[InputRequired()])
    email = EmailField('Email', validators=[InputRequired(), Email()])

@app.route('/', methods=['GET', 'POST'])
def index():
    form = ContactForm()
    if form.validate_on_submit():
        name = form.name.data
        email = form.email.data
        return f"Name: {name}, Email: {email}"
    return render_template('form.html', form=form)

if __name__ == '__main__':
    app.run(debug=True)
```

**Penjelasan:**
- `FlaskForm` digunakan untuk membuat formulir yang akan divalidasi.
- `validate_on_submit()` memastikan bahwa formulir hanya diproses setelah dikirim dan valid.
- `InputRequired()` dan `Email()` adalah validator yang memastikan data formulir valid.

**Template `form.html`:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Form Example</title>
</head>
<body>
    <h1>Submit Your Information</h1>
    <form method="POST">
        {{ form.hidden_tag() }}  <!-- CSRF protection -->
        <label for="name">Name:</label>
        {{ form.name() }}<br><br>
        <label for="email">Email:</label>
        {{ form.email() }}<br><br>
        <button type="submit">Submit</button>
    </form>
</body>
</html>
```

#### 3.4 Menggunakan Template Jinja2

Flask menggunakan **Jinja2** sebagai engine template, yang memungkinkan kita untuk menambahkan logika dinamis dalam HTML. Ini termasuk kemampuan untuk menampilkan variabel, melakukan loop, dan kondisi.

**Contoh Penggunaan Template Jinja2:**

```python
from flask import Flask, render_template

app = Flask(__name__)

@app.route('/')
def index():
    user = {"name": "Alice", "email": "alice@example.com"}
    return render_template('user.html', user=user)

if __name__ == '__main__':
    app.run(debug=True)
```

**Template `user.html`:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>User Info</title>
</head>
<body>
    <h1>User Information</h1>
    <p>Name: {{ user.name }}</p>
    <p>Email: {{ user.email }}</p>
</body>
</html>
```

**Penjelasan:**
- Variabel `user` dikirim ke template dan diakses menggunakan sintaks `{{ user.name }}` dan `{{ user.email }}`.
- Jinja2 memungkinkan kita untuk menambahkan logika dinamis ke dalam template HTML.

#### 3.5 Kesimpulan Modul 3
Pada modul ini, kita telah:
- Belajar cara membuat dan menangani formulir di Flask.
- Mempelajari cara validasi data formulir, baik secara manual maupun menggunakan Flask-WTF.
- Menjelajahi template Jinja2 untuk menampilkan data dinamis dalam aplikasi.

[Back to main](https://github.com/rohmanurnr/learn-flask/tree/main)