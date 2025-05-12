---
icon: database
---

# Akses Database & Create New Database

Akses ke database tergantung pada jenis database yang digunakan dan bagaimana konfigurasi sistemnya. Untuk mengakses database melalui terminal, langkahnya akan tergantung pada jenis database yang Anda gunakan. Berikut adalah panduan umum untuk database populer:

## MySQL/MariaDB

Berikut adalah panduan untuk mengakses database sekaligus membuat database baru di berbagai sistem database menggunakan terminal:

***

### **1. MySQL/MariaDB**

#### **Akses Database**

1.  **Login ke MySQL:**

    ```bash
    mysql -u username -p 
    #or
    #Aktifkan Plugin mysql_native_password
    mysql -u root -p
    ```

    * Ganti `username` dengan nama pengguna database Anda.
    * Masukkan password saat diminta.
2.  **Lihat Daftar Database:**

    ```sql
    SHOW DATABASES;
    ```

#### **Membuat Database Baru**

1. **Buat Database:**

```sql
CREATE DATABASE database_name;
```

* Ganti `database_name` dengan nama yang Anda inginkan untuk database.

1.  **Gunakan Database:**

    ```sql
    USE database_name;
    ```
2.  **Buat Tabel di Database:**

    ```sql
    CREATE TABLE table_name (
        id INT AUTO_INCREMENT PRIMARY KEY,
        name VARCHAR(100),
        created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
    );
    ```
3.  **Insert Data ke Tabel:**

    ```sql
    INSERT INTO table_name (name) VALUES ('Sample Name');
    ```
4.  **Tampilkan Data dari Tabel:**

    ```sql
    SELECT * FROM table_name;
    ```
5.  **Keluar dari MySQL:**

    ```bash
    EXIT;
    ```

***

### **2. PostgreSQL**

#### **Akses Database**

1.  **Login ke PostgreSQL:**

    ```bash
    psql -U username -d postgres
    ```

    * Ganti `username` dengan nama pengguna PostgreSQL Anda.
2.  **Lihat Daftar Database:**

    ```sql
    \l
    ```

#### **Membuat Database Baru**

1.  **Buat Database:**

    ```sql
    CREATE DATABASE database_name;
    ```
2.  **Gunakan Database:**

    ```sql
    \c database_name
    ```
3.  **Buat Tabel di Database:**

    ```sql
    CREATE TABLE table_name (
        id SERIAL PRIMARY KEY,
        name VARCHAR(100),
        created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
    );
    ```
4.  **Insert Data ke Tabel:**

    ```sql
    INSERT INTO table_name (name) VALUES ('Sample Name');
    ```
5.  **Tampilkan Data dari Tabel:**

    ```sql
    SELECT * FROM table_name;
    ```
6.  **Keluar dari PostgreSQL:**

    ```bash
    \q
    ```

***

### **3. MongoDB**

#### **Akses Database**

1.  **Login ke MongoDB:**

    ```bash
    mongo
    ```
2.  **Lihat Daftar Database:**

    ```bash
    show dbs
    ```

#### **Membuat Database Baru**

1.  **Pilih Database (Otomatis Membuat jika Belum Ada):**

    ```bash
    use database_name
    ```
2.  **Buat Koleksi Baru dan Masukkan Data:**

    ```bash
    db.collection_name.insertOne({ name: "Sample Name", created_at: new Date() });
    ```
3.  **Tampilkan Data:**

    ```bash
    db.collection_name.find();
    ```
4.  **Keluar dari MongoDB:**

    ```bash
    exit
    ```

***

### **4. SQLite**

#### **Akses Database**

1.  **Buka SQLite Database:**

    ```bash
    sqlite3 database_name.db
    ```

#### **Membuat Database Baru**

1.  **Buat Tabel Baru:**

    ```sql
    CREATE TABLE table_name (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        name TEXT,
        created_at DATETIME DEFAULT CURRENT_TIMESTAMP
    );
    ```
2.  **Insert Data ke Tabel:**

    ```sql
    INSERT INTO table_name (name) VALUES ('Sample Name');
    ```
3.  **Tampilkan Data dari Tabel:**

    ```sql
    SELECT * FROM table_name;
    ```
4.  **Keluar dari SQLite:**

    ```bash
    .exit
    ```

***

#### Tekninsnya berikut ini:

