# Export Data

To export the database in MySQL Workbench, follow these steps:

<figure><img src="../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

#### **Step 1: Select Tables to Export**

1. In the **"Tables to Export"** section, check the database schema you want to export from the list on the left.
2. On the right side, select specific tables you want to export. If you want to export all tables, click **"Select Tables"**.

#### **Step 2: Choose Objects to Export (Optional)**

* If you want to export **stored procedures and functions**, check the box **"Dump Stored Procedures and Functions"**.
* If you want to export **events**, check **"Dump Events"**.
* If you need **triggers**, check **"Dump Triggers"**.

#### **Step 3: Configure Export Options**

* You can export to either:
  * **Dump Project Folder:** Exports each table into a separate file.
  * **Self-Contained File (Recommended):** Exports everything into a single SQL file.\
    ✅ Since you already selected this option, you will get a single file.
* You can check:
  * **"Create Dump in a Single Transaction"** (Useful for consistency).
  * **"Include Create Schema"** (Recommended if restoring to a new database).

#### **Step 4: Start the Export Process**

1. Click **"Start Export"**.
2. Wait for the export to complete. The output log at the bottom will display progress.
3. Once completed, the exported SQL file will be available in the specified location:\
   `C:\Users\LEGION\Documents\dumps\db.sql`

Now, you can use this file to restore your database in MySQL Workbench or any MySQL server. Let me know if you need further help! 😊
