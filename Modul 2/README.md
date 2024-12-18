## **Modul 2: Handling Files in Flask**

### **2.1 Apa itu File Handling di Flask?**

**File handling** di Flask merujuk pada proses menerima dan memproses file yang diupload pengguna melalui form HTML. Flask menyediakan fasilitas untuk menangani file dengan mudah, termasuk pengunggahan file dan validasi jenis file.

Pada aplikasi web, pengguna sering kali perlu mengirimkan file (misalnya, file CSV untuk analisis data) kepada server. Flask memberikan API sederhana untuk menangani file ini menggunakan objek `request`.

### **2.2 Instalasi dan Persiapan**

Sebelum kita mulai menangani file di Flask, pastikan Anda sudah menginstal Flask dan mempersiapkan aplikasi seperti yang telah kita lakukan pada modul sebelumnya.

Untuk memproses file CSV atau Excel di Flask, kita juga membutuhkan beberapa pustaka tambahan:

- **Werkzeug**: Flask sudah dilengkapi dengan `Werkzeug`, yang digunakan untuk menangani file upload.
- **Pandas**: Untuk memproses file CSV atau Excel, kita akan menggunakan `Pandas`, yang memudahkan pembacaan dan manipulasi data.
  
Install Pandas dengan:
```bash
pip install pandas
```

### **2.3 Membuat Form untuk Mengunggah File**

Langkah pertama adalah membuat form HTML untuk memungkinkan pengguna mengunggah file. Berikut adalah contoh form HTML yang menerima file:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Upload File</title>
</head>
<body>
    <h1>Upload File CSV</h1>
    <form action="/upload" method="POST" enctype="multipart/form-data">
        <label for="file">Pilih file CSV:</label>
        <input type="file" name="file" id="file" accept=".csv,.xlsx">
        <button type="submit">Upload</button>
    </form>
</body>
</html>
```

Form di atas mengirimkan file ke route Flask menggunakan metode POST. Attribut `enctype="multipart/form-data"` diperlukan untuk mengunggah file.

### **2.4 Menangani File Upload di Flask**

Di Flask, kita dapat menggunakan objek `request.files` untuk mengakses file yang diunggah. Berikut adalah cara menangani file yang diunggah dari form di atas:

1. **Membuat Route untuk Mengunggah File**
   
   Di dalam file `app.py`, buat route untuk menangani form dan file upload:
   
   ```python
   from flask import Flask, request, render_template
   import os

   app = Flask(__name__)

   # Tentukan direktori untuk menyimpan file yang diupload
   UPLOAD_FOLDER = 'uploads'
   app.config['UPLOAD_FOLDER'] = UPLOAD_FOLDER
   app.config['ALLOWED_EXTENSIONS'] = {'csv', 'xlsx'}

   # Fungsi untuk memeriksa apakah ekstensi file diizinkan
   def allowed_file(filename):
       return '.' in filename and filename.rsplit('.', 1)[1].lower() in app.config['ALLOWED_EXTENSIONS']

   @app.route('/')
   def upload_form():
       return render_template('upload.html')  # Menampilkan form upload

   @app.route('/upload', methods=['POST'])
   def upload_file():
       if 'file' not in request.files:
           return 'No file part'
       file = request.files['file']
       if file.filename == '':
           return 'No selected file'
       if file and allowed_file(file.filename):
           # Menyimpan file ke folder uploads
           filename = os.path.join(app.config['UPLOAD_FOLDER'], file.filename)
           file.save(filename)
           return f'File uploaded successfully: {filename}'
       else:
           return 'Invalid file type. Only CSV and Excel files are allowed.'
   
   if __name__ == "__main__":
       app.run(debug=True)
   ```

2. **Menyimpan File yang Diupload**

   Di atas, kita menyimpan file yang diupload ke dalam folder `uploads` (pastikan folder ini ada di direktori yang sama dengan aplikasi Flask Anda). Nama file disusun menggunakan `os.path.join()` untuk membuat path lengkap.

3. **Validasi Ekstensi File**

   Fungsi `allowed_file()` digunakan untuk memastikan bahwa hanya file dengan ekstensi yang diperbolehkan (CSV atau Excel) yang dapat diunggah.

### **2.5 Menampilkan Pesan Setelah Mengunggah File**

Setelah file berhasil diunggah, Anda bisa menampilkan pesan yang mengonfirmasi bahwa file telah berhasil disimpan. Anda juga bisa melanjutkan dengan memproses file tersebut, misalnya, membaca isi file CSV menggunakan Pandas.

### **2.6 Membaca File CSV dengan Pandas**

Jika file yang diunggah adalah file CSV, Anda bisa menggunakan Pandas untuk membaca dan memprosesnya. Berikut adalah contoh cara membaca file CSV setelah diunggah:

```python
import pandas as pd

@app.route('/upload', methods=['POST'])
def upload_file():
    if 'file' not in request.files:
        return 'No file part'
    file = request.files['file']
    if file.filename == '':
        return 'No selected file'
    if file and allowed_file(file.filename):
        filename = os.path.join(app.config['UPLOAD_FOLDER'], file.filename)
        file.save(filename)
        
        # Membaca file CSV menggunakan Pandas
        if filename.endswith('.csv'):
            data = pd.read_csv(filename)
            return f'File uploaded successfully. Data head:\n{data.head()}'
        else:
            return 'File type is not CSV'
```

Setelah file disimpan, kita menggunakan `pd.read_csv()` untuk membaca file dan menampilkan beberapa baris pertama dari data.

### **2.7 Menangani File Excel**

Untuk file Excel, Anda bisa menggunakan `pd.read_excel()` untuk membaca data dari file `.xlsx`:

```python
if filename.endswith('.xlsx'):
    data = pd.read_excel(filename)
    return f'File uploaded successfully. Data head:\n{data.head()}'
```

---

### **Kesimpulan**

Pada modul ini, kita telah membahas:
- Cara membuat form HTML untuk mengunggah file.
- Menangani file yang diupload menggunakan Flask dengan objek `request.files`.
- Menyimpan file di server menggunakan Python dan Flask.
- Membaca dan memproses file CSV atau Excel dengan Pandas.
