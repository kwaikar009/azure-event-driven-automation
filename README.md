# Automated Business Process on Azure (Event-Driven)

![Azure](https://img.shields.io/badge/Azure-Managed%20Services-0089D6?logo=microsoft-azure&logoColor=white)
![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?logo=python&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-Azure%20Database-4479A1?logo=mysql&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green)
![Status](https://img.shields.io/badge/Status-Active-brightgreen)

A custom Python program running on an Azure Ubuntu VM converts uploaded text files to CSV, stores them in Blob Storage, and pushes structured data into a MySQL database — automating an end-to-end business data pipeline.

---

## Overview

This project automates a multi-step business data pipeline on Azure. A custom Python program running on an **Azure Ubuntu VM** accepts a text file, converts it to a structured **CSV**, uploads the CSV to **Azure Blob Storage**, and inserts the parsed data into **Azure Database for MySQL** — eliminating manual data handling end to end.

---

## Architecture

```
  +---------+    File Upload    +----------------------+
  |  Users  | --------------->  |   Azure VM (Ubuntu)   |
  +---------+                  |   Python Program      |
                               +----------+-----------+
                                          |
                          +--------------+----------------+
                          |                               |
               +------------------+          +---------------------+
               |  Azure Blob      |          |  Azure Database     |
               |  Storage         |          |  for MySQL          |
               |  (CSV output)    |          |  (structured data)  |
               +------------------+          +---------------------+
```

### Architecture Implementation

| Step | Description |
|------|-------------|
| 1 | Upload the custom program and provided text file to a VM created using Ubuntu |
| 2 | Create a MySQL server using Azure Database service |
| 3 | Create a database inside the MySQL server created above |
| 4 | Running the custom program converts the text file into a CSV, uploads it to Blob Storage, and sends the data to the MySQL server |

---

## Features

- Automated text-to-CSV conversion from raw input files
- Azure Blob Storage integration for cloud-based CSV storage
- Azure Database for MySQL integration for structured data persistence
- Hosted on an Azure Ubuntu VM for a controlled execution environment
- Modular Python codebase using the Azure SDK
- End-to-end pipeline requiring no manual intervention after initial file upload

---

## Tech Stack

| Component      | Technology                        |
|----------------|-----------------------------------|
| Cloud Platform | Microsoft Azure                   |
| Language       | Python 3.10+                      |
| Compute        | Azure Virtual Machine (Ubuntu)    |
| Storage        | Azure Blob Storage                |
| Database       | Azure Database for MySQL          |
| SDK            | azure-storage-blob                |
| DB Connector   | mysql-connector-python            |

---

## Project Structure

```
azure-event-driven-automation/
├── process.py       # Main script — runs directly on the VM
├── docproc-invoice.txt           # Sample input file
├── architecture.png      # Architecture diagram
├── screenshots/          # Project screenshots
├── .gitignore
├── requirements.txt      # Dependency reference list
└── README.md
```

---

## Setup and Installation

### Prerequisites

- Active Azure Subscription
- Azure Ubuntu VM (provisioned and SSH-accessible via a `.pem` key file)
- Azure Blob Storage container created with a valid connection string
- Azure Database for MySQL server and database provisioned
- Local machine with `ssh` and `scp` available

---

### Step 1 — Transfer Files to the VM

Use `scp` to copy both the invoice file and the Python script to the Ubuntu VM. Run the command once for each file, replacing the placeholders with your actual `.pem` file path, filename, and VM public IP address.

```bash
scp -i <pem-file> <file-to-copy> ubuntu@<public-IP-of-VM>:/home/ubuntu
```

Example:

```bash
scp -i my-key.pem docproc-invoice.txt       ubuntu@20.10.5.100:/home/ubuntu
scp -i my-key.pem process.py   ubuntu@20.10.5.100:/home/ubuntu
```

---

### Step 3 — SSH into the VM and Set Up the Environment

Connect to the VM via SSH:

```bash
ssh -i <pem-file> ubuntu@<public-IP-of-VM>
```

Once connected, run the following commands to install all required dependencies:

```bash
sudo apt update
sudo apt install python3
sudo apt install python3-pip
sudo pip3 install pandas
sudo pip3 install azure-storage-blob
sudo pip3 install mysql-connector-python
sudo apt install mysql-client-core-8.0
```

---

### Step 4 — Run the Program

Navigate to the home directory and execute the Python script:

```bash
cd /home/ubuntu
python3 process.py
```

The script will convert the input text file to CSV, upload the CSV to Azure Blob Storage, and insert the parsed data into the MySQL database.

---

## Script Configuration

Before deploying, open `process.py` and update the following lines with your Azure credentials:

| Line | Value |
|------|-------|
| 9    | Azure Database for MySQL — server hostname |
| 10   | MySQL admin username |
| 11   | MySQL admin password |
| 15   | Azure Blob Storage account connection string |

**Note:** Do not commit `process.py` after adding credentials. It is listed in `.gitignore` for this reason.

---

## Input File Format

The program expects a pipe-delimited text file with a header row on line 1:

```
name|age|city
Alice|30|Dallas
Bob|25|Austin
```

---


## Screenshots

### VM Creation
![VM Creation](screenshots/VM_Creation.png)

### Storage Account Creation
![Storage Account Creation](screenshots/Storage_account_creation.png)

### MySQL Database Creation
![MySQL Database Creation](screenshots/Mysql_Database_creation.png)

### Database Table Creation
![Database Table Creation](screenshots/Database_Table_creation.png)

### Successful Execution of process.py
![Successful Process.py Running](screenshots/Successful_Process_py_running.png)

### CSV File Uploaded in Blob Storage Container
![CSV File Uploaded](screenshots/CSV_file_uploaded_in_storage_blob_container.png)

### Retrieving Records from Database Table
![Retrieving Table Records](screenshots/Retrieving_table_records_from_database_table.png)

---

## Running Tests

```bash
pytest tests/ -v
```

---

## Impact

- Automated previously manual data entry and file handling workflows
- Improved system reliability through a structured, repeatable pipeline
- Delivered hands-on experience with Azure VM, Blob Storage, and Azure Database for MySQL in a production environment

---

## License

This project is licensed under the [MIT License](LICENSE).

---

## Author

**Your Name**  
[LinkedIn](https://linkedin.com/in/yourprofile) | [GitHub](https://github.com/yourusername)
