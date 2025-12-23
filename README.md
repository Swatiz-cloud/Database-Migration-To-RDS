# 📦 Database Migration from Localhost to AWS RDS (MySQL)

## 📌 Project Overview

This project demonstrates a complete database migration from a local MariaDB/MySQL server to AWS RDS MySQL using command-line tools.
The migration is performed using an EC2 instance as an intermediate migration host, following real-world enterprise practices.

---

## 🛠️ Technology Stack

- AWS EC2
- AWS RDS (MySQL)
- MySQL
- MariaDB
- Ubuntu Linux

---

## 🏗️ Architecture Diagram – Database Migration

**Flow:** Localhost → EC2 → AWS RDS

Local MariaDB/MySQL  
        ↓ (mysqldump)  
AWS EC2 (Migration Host)  
        ↓ (restore)  
AWS RDS MySQL  

---

## 🔍 Architecture Explanation

1. **Local MariaDB Server**
   - Hosts source database `myclass`
   - Tables: `students`, `staff`, `course`
   - Backup created using `mysqldump`

2. **AWS EC2 Instance**
   - Secure migration bridge
   - Stores SQL dump file
   - Connects to RDS over port 3306

3. **AWS RDS MySQL**
   - Managed database service
   - Target database `Fortune`
   - Data restored from SQL dump

---

## 🚀 Step 1: Install MariaDB on Localhost

```bash
sudo apt update
sudo apt install mariadb-server -y
sudo mysql
```

---

## 🔐 Step 2: Create Database and Tables

```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY 'Pass@123';
CREATE DATABASE myclass;
USE myclass;
```

### Tables

```sql
CREATE TABLE students (
  id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(100),
  email VARCHAR(50),
  contact BIGINT,
  address VARCHAR(100)
);

CREATE TABLE staff (
  staff_id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(100),
  email VARCHAR(50),
  contact BIGINT,
  department VARCHAR(50)
);

CREATE TABLE course (
  c_id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(100),
  duration VARCHAR(50),
  fees FLOAT(10)
);
```

---

## 🧪 Step 3: Insert Sample Data

```sql
INSERT INTO students VALUES
(NULL,'Sanchita','sanch@gmail.com',9878865645,'Pune'),
(NULL,'Vaibhav','vaibhav@gmail.com',8765877432,'Kurla');

INSERT INTO staff VALUES
(NULL,'Yogita','yogi@gmail.com',9878865645,'HR'),
(NULL,'Pranay','Pranay@gmail.com',8765877432,'Admin');

INSERT INTO course VALUES
(NULL,'MCA','6 months',50000),
(NULL,'DADS','7 months',60000);
```

---

## 📤 Step 4: Take Database Backup

```bash
mysqldump -u root -p myclass > mybackup.sql
```

---

## ☁️ Step 5: Connect to AWS RDS from EC2

```bash
sudo mysql -u admin -p -h myrds.cgviiougcvqs.us-east-1.rds.amazonaws.com
```

---

## 🆕 Step 6: Create Database on RDS

```sql
CREATE DATABASE Fortune;
USE Fortune;
```

---

## 📥 Step 7: Restore Backup to RDS

```bash
sudo mysql -u admin -p -h myrds.cgviiougcvqs.us-east-1.rds.amazonaws.com Fortune < mybackup.sql
```

---

## ✅ Step 8: Verify Migration

```sql
SHOW TABLES;
SELECT * FROM students;
SELECT * FROM staff;
SELECT * FROM course;
```

---

## 🔐 Security Best Practices

- Allow port 3306 only from EC2 Security Group
- Avoid root user in production
- Disable public access to RDS
- Use AWS Secrets Manager for credentials

---

## 🎯 Key Learnings

- MySQL/MariaDB administration
- Database backup and restore
- Secure EC2 to RDS connectivity
- Real-world cloud database migration

---

## 🏁 Conclusion

This project showcases a production-style database migration suitable for AWS Cloud Engineer, DevOps Engineer, and Database Administrator roles.
