---
icon: seedling
---

# Requirements.txt

Untuk menginstal dependencies dari file `requirements.txt`, dapat mengikuti langkah-langkah berikut:

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

***

#### **Langkah-Langkah untuk Install Requirements**

1. **Pastikan berada di Virtual Environment**:
   *   Jika belum mengaktifkan virtual environment, aktifkan dengan:

       ```bash
       .\env\Scripts\activate  # Untuk Windows
       source env/bin/activate # Untuk Linux/Mac
       ```
2. **Pastikan File `requirements.txt` Ada**:
   *   Verifikasi apakah file `requirements.txt` ada di direktori proyek Anda:

       ```bash
       ls requirements.txt  # Untuk Linux/Mac
       dir requirements.txt # Untuk Windows
       ```
3. **Jalankan Perintah Install**:
   *   Jalankan perintah berikut untuk menginstal semua dependencies dari file `requirements.txt`:

       ```bash
       pip install -r requirements.txt
       ```
4. **Verifikasi Instalasi**:
   *   Setelah instalasi selesai, cek apakah semua modul berhasil diinstal:

       ```bash
       pip list
       ```

***

#### **Jika Error Terjadi**

1. **Error: `ModuleNotFoundError` atau Dependency Tidak Terinstal**
   *   Perbarui `pip` sebelum instalasi:

       ```bash
       pip install --upgrade pip
       ```
2. **Error: Versi Python atau Modul Tidak Kompatibel**
   *   Pastikan menggunakan versi Python yang benar dengan proyek . Cek versi Python:

       ```bash
       python --version
       ```
3. **Error: Tidak Ada File `requirements.txt`**
   *   Jika file `requirements.txt` tidak ditemukan, buat file ini secara manual dengan daftar dependencies:

       ```bash
       pip freeze > requirements.txt
       ```

***

#### **Contoh File `requirements.txt`**

File `requirements.txt` biasanya berisi daftar dependencies seperti ini:

```
Django==4.2.2
django-admin-interface==0.19.1
djangorestframework==3.14.0
Pillow==9.5.0
```

Jika Anda perlu bantuan untuk membuat file ini, jalankan:

```bash
pip freeze > requirements.txt
```
