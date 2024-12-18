### Modul 4: Session Management, Cookies, dan Flask Extensions

#### 4.1 Session Management di Flask

**Session** digunakan untuk menyimpan data pengguna secara sementara antara permintaan HTTP yang berbeda. Data ini disimpan di sisi server dan dikaitkan dengan ID sesi yang disimpan di browser pengguna sebagai cookie. Flask secara otomatis mengelola sesi ini, dan kita dapat menggunakan objek `session` untuk menyimpan dan mengakses data sesi.

**Pengaturan Sesi di Flask:**
1. Flask menggunakan **secret key** untuk mengenkripsi data sesi agar aman.
2. Untuk mengaktifkan sesi, kita perlu mengatur `SECRET_KEY` pada aplikasi Flask.

**Contoh Penggunaan Sesi di Flask:**
```python
from flask import Flask, session, redirect, url_for, request

app = Flask(__name__)
app.config['SECRET_KEY'] = 'mysecretkey'  # Kunci rahasia untuk enkripsi sesi

@app.route('/')
def index():
    # Memeriksa apakah variabel sesi 'username' ada
    if 'username' in session:
        return f"Logged in as {session['username']}"
    return "You are not logged in!"

@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        session['username'] = request.form['username']  # Menyimpan data sesi
        return redirect(url_for('index'))
    return '''
        <form method="post">
            Username: <input type="text" name="username">
            <input type="submit" value="Login">
        </form>
    '''

@app.route('/logout')
def logout():
    session.pop('username', None)  # Menghapus data sesi
    return redirect(url_for('index'))

if __name__ == '__main__':
    app.run(debug=True)
```

**Penjelasan:**
- Pada rute `/`, kita memeriksa apakah `username` ada dalam sesi. Jika ada, kita menampilkan nama pengguna yang sedang login. Jika tidak, kita menampilkan pesan bahwa pengguna tidak login.
- Pada rute `/login`, kita menerima input dari pengguna dan menyimpan nama pengguna dalam sesi menggunakan `session['username']`.
- Pada rute `/logout`, kita menghapus data sesi `username` dan mengarahkan pengguna kembali ke halaman utama.

#### 4.2 Cookies di Flask

**Cookies** adalah cara untuk menyimpan data di sisi klien (browser). Di Flask, kita bisa menggunakan objek `response` untuk mengatur cookies dan mengaksesnya melalui objek `request`.

**Contoh Menggunakan Cookies di Flask:**
```python
from flask import Flask, request, make_response

app = Flask(__name__)

@app.route('/')
def index():
    # Mengambil nilai cookie "username" jika ada
    username = request.cookies.get('username')
    if username:
        return f"Hello, {username}!"
    return "Hello, Guest!"

@app.route('/set_cookie/<username>')
def set_cookie(username):
    resp = make_response(f"Cookie set for {username}")
    resp.set_cookie('username', username)  # Menyimpan username dalam cookie
    return resp

@app.route('/delete_cookie')
def delete_cookie():
    resp = make_response("Cookie has been deleted")
    resp.delete_cookie('username')  # Menghapus cookie
    return resp

if __name__ == '__main__':
    app.run(debug=True)
```

**Penjelasan:**
- Pada rute `/`, kita mencoba mengambil cookie `username` dan menampilkannya jika ada.
- Pada rute `/set_cookie/<username>`, kita menyimpan `username` dalam cookie menggunakan `set_cookie()`.
- Pada rute `/delete_cookie`, kita menghapus cookie dengan `delete_cookie()`.

#### 4.3 Flask Extensions

Flask memiliki banyak ekstensi yang bisa membantu dalam memperluas fungsionalitas aplikasi. Beberapa ekstensi yang sering digunakan meliputi Flask-WTF (untuk form handling), Flask-SQLAlchemy (untuk integrasi database), Flask-Login (untuk manajemen pengguna), dan banyak lagi.

Pada modul ini, kita akan melihat bagaimana menggunakan Flask-SQLAlchemy dan Flask-Login sebagai contoh.

##### 4.3.1 Flask-SQLAlchemy

**Flask-SQLAlchemy** adalah ekstensi untuk Flask yang menyediakan integrasi yang mudah dengan database SQL (seperti SQLite, PostgreSQL, MySQL, dll.) melalui SQLAlchemy ORM (Object Relational Mapping).

**Instalasi Flask-SQLAlchemy:**
```bash
pip install flask-sqlalchemy
```

**Contoh Menggunakan Flask-SQLAlchemy:**
```python
from flask import Flask, render_template
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///users.db'  # Menggunakan SQLite sebagai database
db = SQLAlchemy(app)

# Mendefinisikan model User
class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(120), unique=True, nullable=False)
    email = db.Column(db.String(120), unique=True, nullable=False)

    def __repr__(self):
        return f'<User {self.username}>'

# Membuat database dan tabel
@app.before_first_request
def create_tables():
    db.create_all()

@app.route('/')
def index():
    # Mengambil semua pengguna dari database
    users = User.query.all()
    return render_template('index.html', users=users)

if __name__ == '__main__':
    app.run(debug=True)
```

**Penjelasan:**
- Kita mendefinisikan model `User` yang berisi kolom `id`, `username`, dan `email`.
- `db.create_all()` digunakan untuk membuat tabel di database jika belum ada.
- `User.query.all()` digunakan untuk mengambil semua pengguna dari database.

##### 4.3.2 Flask-Login

**Flask-Login** adalah ekstensi yang menyediakan cara yang mudah untuk menangani login dan manajemen sesi pengguna.

**Instalasi Flask-Login:**
```bash
pip install flask-login
```

**Contoh Menggunakan Flask-Login:**
```python
from flask import Flask, render_template, redirect, url_for, request
from flask_login import LoginManager, UserMixin, login_user, login_required, logout_user, current_user

app = Flask(__name__)
app.config['SECRET_KEY'] = 'mysecretkey'
login_manager = LoginManager()
login_manager.init_app(app)

# Menggunakan UserMixin untuk memudahkan pembuatan model User
class User(UserMixin):
    def __init__(self, id):
        self.id = id

users = {'user1': {'password': 'mypassword'}}

@login_manager.user_loader
def load_user(user_id):
    return User(user_id)

@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        user_id = request.form['username']
        password = request.form['password']
        if user_id in users and users[user_id]['password'] == password:
            user = User(user_id)
            login_user(user)
            return redirect(url_for('protected'))
    return '''
        <form method="post">
            Username: <input type="text" name="username"><br>
            Password: <input type="password" name="password"><br>
            <input type="submit" value="Login">
        </form>
    '''

@app.route('/protected')
@login_required
def protected():
    return f'Logged in as {current_user.id}'

@app.route('/logout')
def logout():
    logout_user()
    return redirect(url_for('login'))

if __name__ == '__main__':
    app.run(debug=True)
```

**Penjelasan:**
- `UserMixin` menyediakan implementasi dasar untuk model pengguna.
- Fungsi `user_loader` digunakan untuk memuat pengguna berdasarkan ID.
- `login_required` digunakan untuk melindungi rute sehingga hanya pengguna yang sudah login yang dapat mengaksesnya.

#### 4.4 Kesimpulan Modul 4

Pada modul ini, kita telah:
- Mempelajari cara menggunakan **session management** dan **cookies** di Flask untuk menyimpan data sementara.
- Mengenal **Flask Extensions**, seperti Flask-SQLAlchemy untuk database dan Flask-Login untuk manajemen sesi pengguna.

[Back to main](https://github.com
rohmanurnr/learn-flask/tree/main)