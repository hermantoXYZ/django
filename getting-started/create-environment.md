---
icon: seedling
---

# Create Environment

Membuat **environment (VIRTUAL ENVIROMENT)** untuk mengisolasi proyek dalam pengembangan aplikasi, terutama saat menggunakan bahasa seperti **Python**. Berikut adalah cara membuat environment untuk berbagai kebutuhan:

***

#### **1. Python Virtual Environment**

**Langkah-Langkah:**

1. **Pastikan Python Terinstal**:
   *   Cek versi Python:

       ```bash
       python --version
       ```

       atau

       ```bash
       python3 --version
       ```
2. **Instal `virtualenv` (Opsional)**:
   *   Jika belum terinstal:

       ```bash
       pip install virtualenv
       ```
3. **Buat Virtual Environment**:
   *   Jalankan perintah berikut di direktori proyek Anda:

       ```bash
       python -m venv env
       ```

       * Ganti `env_name` dengan nama environment Anda.
4. **Aktifkan Virtual Environment**:
   *   **Windows**:

       ```bash
       .\env\Scripts\activate
       ```
   *   **Linux/Mac**:

       ```bash
       source env/bin/activate
       ```
5. **Install Dependencies**:
   *   Setelah aktivasi, install package atau library:

       ```bash
       pip install package_name
       ```
6.  **Keluar dari Virtual Environment**:

    ```bash
    deactivate
    ```

***

#### **2. Node.js Environment**

**Langkah-Langkah:**

1. **Pastikan Node.js Terinstal**:
   *   Cek versi Node.js:

       ```bash
       node -v
       ```

       dan npm:

       ```bash
       npm -v
       ```
2. **Inisialisasi Proyek Node.js**:
   *   Di direktori proyek Anda:

       ```bash
       npm init -y
       ```

       * Ini akan membuat file `package.json`.
3. **Instal Dependencies**:
   *   Install package dengan npm:

       ```bash
       npm install package_name
       ```

***

#### **3. Docker Environment**

**Langkah-Langkah:**

1. **Instal Docker**:
   *   Pastikan Docker sudah terinstal dan berjalan:

       ```bash
       docker --version
       ```
2. **Buat Dockerfile**:
   *   Contoh sederhana `Dockerfile`:

       ```dockerfile
       FROM python:3.9-slim
       WORKDIR /app
       COPY . .
       RUN pip install -r requirements.txt
       CMD ["python", "app.py"]
       ```
3.  **Bangun Docker Image**:

    ```bash
    docker build -t my_environment .
    ```
4.  **Jalankan Docker Container**:

    ```bash
    docker run -p 5000:5000 my_environment
    ```

***

#### **4. Anaconda Environment**

**Langkah-Langkah:**

1. **Pastikan Anaconda Terinstal**:
   *   Cek instalasi Anaconda:

       ```bash
       conda --version
       ```
2.  **Buat Environment Baru**:

    ```bash
    conda create --name env_name python=3.9
    ```

    * Ganti `env_name` dengan nama environment Anda.
3.  **Aktifkan Environment**:

    ```bash
    conda activate env_name
    ```
4.  **Install Dependencies**:

    ```bash
    conda install package_name
    ```
5.  **Keluar dari Environment**:

    ```bash
    conda deactivate
    ```

####
