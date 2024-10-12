 # **Data Encryption & Decryption for Secure Dataset Management**

## **Overview**

This project provides a secure method for encrypting and decrypting sensitive company datasets using Python, `pandas`, and the `cryptography` library. The project focuses on protecting sensitive information by encrypting it with a unique encryption key and enabling only authorized users to decrypt the data. The project also logs key actions for auditing purposes.

### **Main Features:**
- **Encryption**: Encrypts sensitive data from CSV files.
- **Decryption**: Only authorized users can decrypt the data using the encryption key.
- **Access Control**: Role-based access for data decryption.
- **Logging**: Tracks encryption and decryption actions for auditing.
- **Chunk Processing**: Handles large datasets efficiently by encrypting and decrypting in chunks.

---

## **Table of Contents**
- [Installation](#installation)
- [Usage](#usage)
- [Encrypting Data](#encrypting-data)
- [Decrypting Data](#decrypting-data)
- [Access Control](#access-control)
- [Audit Logs](#audit-logs)
- [Project Structure](#project-structure)
- [Contributing](#contributing)
- [License](#license)

---

## **Installation**

### **1. Clone the Repository**
```bash
git clone https://github.com/your-username/DataEncryptionDecryption.git
cd DataEncryptionDecryption
```

### **2. Set Up a Virtual Environment (Optional but Recommended)**
```bash
python -m venv venv
source venv/bin/activate  # On Windows, use: venv\Scripts\activate
```

### **3. Install Required Libraries**
```bash
pip install -r requirements.txt
```

### **4. Prepare Your Dataset**
Make sure you have a dataset in CSV format named `Practice.csv` in the same directory. You can replace `Practice.csv` with any other file as required, but make sure to adjust the code if you do.

---

## **Usage**

### **1. Encrypting Data**

To encrypt your CSV file:

```bash
python DataEncryption&Decryption.py
```

The script will:
1. **Load your dataset**.
2. **Check for an existing encryption key**. If it doesn’t exist, it will generate a new key and store it in `encryption_key.key`.
3. **Encrypt the dataset** and save it to a new file called `encrypted_company_data.csv`.
4. **Log the encryption event** in `access.log`.

### **2. Decrypting Data**

To decrypt the encrypted data, run the following command:

```bash
python DataDecryption.py
```

The decryption script will:
1. **Load the encrypted dataset** (`encrypted_company_data.csv`).
2. **Check user authorization** for decryption.
3. **Use the encryption key** stored in `encryption_key.key` and `secret.key` to decrypt the data.
4. **Log the decryption event**.

#### **Access Control**: 
Only authorized users (like "admin") can decrypt the data. You can simulate access control by modifying the user role in the code.

---

## **Access Control**

The code uses **Role-Based Access Control (RBAC)** to ensure only certain users can decrypt the data. In the code, we simulate this by using a predefined list of authorized users (`admin`, `security_manager`). If an unauthorized user tries to decrypt the data, they will receive an **Access Denied** message, and the event will be logged.

---

## **Audit Logs**

All encryption and decryption activities are logged in the `access.log` file. The logs capture the following:
- **Timestamp** of when the data was loaded, encrypted, or decrypted.
- **User details** for decryption attempts (whether successful or denied).

This ensures full transparency and accountability in handling sensitive datasets.

---

## **Project Structure**

```bash
.
├── DataEncryption&Decryption.py    # Main encryption script
├── DataDecryption.py               # Decryption script
├── encryption_key.key              # Encryption key file (generated)
├── secret.key                      # Secret key for added security (generated)
├── Practice.csv                    # Example dataset
├── encrypted_company_data.csv       # Encrypted output file
├── access.log                      # Log file for auditing
├── requirements.txt                # List of required packages
└── README.md                       # Project README file
```

---

## **Contributing**

Contributions are welcome! Please create an issue or submit a pull request with improvements or new features.

---

## **License**

This project is licensed under the MIT License.

---

## **Encrypting Data: Key Code Snippet**

```python
# Encrypt the dataset
def encrypt_data(data, encryption_key):
    fernet = Fernet(encryption_key)
    return data.apply(lambda column: column.map(lambda x: fernet.encrypt(str(x).encode()).decode()))
```

- **What It Does**: Encrypts each column of the dataset using the `Fernet` encryption key. Every piece of data in the dataset is transformed into a non-readable format for security.

---

## **Decrypting Data: Key Code Snippet**

```python
# Decrypt the dataset with access control
def decrypt_data(encrypted_data, encryption_key, user):
    if not has_access(user):
        raise PermissionError("Access Denied. You do not have permission to decrypt this data.")
    fernet = Fernet(encryption_key)
    return encrypted_data.apply(lambda column: column.map(lambda x: fernet.decrypt(x.encode()).decode()))
```

- **What It Does**: Decrypts the encrypted data, but only if the user has permission. It uses the same `Fernet` key that was used for encryption. Access control is applied to ensure only authorized users can decrypt the data.

### **Important Points**:
- **Encryption Key**: Stored in `encryption_key.key`, it is essential for decrypting the data.
- **Access Control**: The decryption process checks if the user has permission using the `has_access()` function. Only users with roles like "admin" can decrypt the dataset.
- **Secret Key**: For an additional layer of security, a secret key (`secret.key`) is generated and used along with the encryption key.

---
