แน่นอน! นี่คือ **คำแนะนำทีละขั้นตอน** สำหรับการสร้างและใช้งาน **Stored Procedure** ใน **MySQL 8** เป็นภาษาไทย

---

### **ขั้นตอนที่ 1: สร้างฐานข้อมูลและตารางตัวอย่าง**
ก่อนสร้าง **Stored Procedure** เราจะสร้างฐานข้อมูลและตารางตัวอย่างก่อน

```sql
CREATE DATABASE mydb;
USE mydb;

CREATE TABLE employees (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    salary DECIMAL(10,2)
);
```

---

### **ขั้นตอนที่ 2: สร้าง Stored Procedure อย่างง่าย**
เราจะสร้าง **Stored Procedure** เพื่อ **เพิ่มพนักงานใหม่** ลงในตาราง `employees`

```sql
DELIMITER //

CREATE PROCEDURE AddEmployee(
    IN emp_name VARCHAR(100),
    IN emp_salary DECIMAL(10,2)
)
BEGIN
    INSERT INTO employees (name, salary) VALUES (emp_name, emp_salary);
END //

DELIMITER ;
```

🔹 `DELIMITER //` เปลี่ยนตัวแบ่งคำสั่ง เพื่อไม่ให้ MySQL ดำเนินการคำสั่งก่อนเวลา  
🔹 `IN emp_name` และ `IN emp_salary` คือ **พารามิเตอร์ขาเข้า (Input Parameters)**  
🔹 `INSERT INTO` ใช้เพิ่มข้อมูลลงในตาราง  
🔹 `DELIMITER ;` คืนค่าตัวแบ่งเป็นค่าเริ่มต้น  

---

### **ขั้นตอนที่ 3: เรียกใช้งาน Stored Procedure**
เรียกใช้งาน **Stored Procedure** เพื่อเพิ่มข้อมูลพนักงาน

```sql
CALL AddEmployee('John Doe', 50000.00);
```

---

### **ขั้นตอนที่ 4: สร้าง Stored Procedure เพื่อดึงข้อมูล**
เราจะสร้าง **Stored Procedure** เพื่อดึง **รายชื่อพนักงานทั้งหมด**

```sql
DELIMITER //

CREATE PROCEDURE GetEmployees()
BEGIN
    SELECT * FROM employees;
END //

DELIMITER ;
```

เรียกใช้งาน:

```sql
CALL GetEmployees();
```

---

### **ขั้นตอนที่ 5: สร้าง Stored Procedure พร้อมพารามิเตอร์ขาออก (Output Parameter)**
Stored Procedure นี้จะ **คืนค่าจำนวนพนักงานทั้งหมด**

```sql
DELIMITER //

CREATE PROCEDURE GetEmployeeCount(OUT emp_count INT)
BEGIN
    SELECT COUNT(*) INTO emp_count FROM employees;
END //

DELIMITER ;
```

เรียกใช้งาน:

```sql
CALL GetEmployeeCount(@count);
SELECT @count;
```

---

### **ขั้นตอนที่ 6: สร้าง Stored Procedure ที่มีพารามิเตอร์ทั้งขาเข้าและขาออก**
Stored Procedure นี้จะ **คืนค่าเงินเดือนของพนักงาน** โดยใช้ชื่อเป็นตัวระบุ

```sql
DELIMITER //

CREATE PROCEDURE GetSalary(
    IN emp_name VARCHAR(100),
    OUT emp_salary DECIMAL(10,2)
)
BEGIN
    SELECT salary INTO emp_salary FROM employees WHERE name = emp_name LIMIT 1;
END //

DELIMITER ;
```

เรียกใช้งาน:

```sql
CALL GetSalary('John Doe', @salary);
SELECT @salary;
```

---

### **ขั้นตอนที่ 7: แก้ไข Stored Procedure ที่มีอยู่**
หากต้องการแก้ไข **Stored Procedure** ให้ลบอันเดิมก่อน แล้วสร้างใหม่

```sql
DROP PROCEDURE IF EXISTS AddEmployee;
```

---

### **ขั้นตอนที่ 8: ลบ Stored Procedure**
หากต้องการ **ลบ** Stored Procedure:

```sql
DROP PROCEDURE IF EXISTS GetEmployees;
```

---

### **ขั้นตอนที่ 9: ดูรายการ Stored Procedure ทั้งหมด**
ใช้คำสั่งนี้เพื่อตรวจสอบ Stored Procedure ที่มีอยู่ในฐานข้อมูล `mydb`

```sql
SHOW PROCEDURE STATUS WHERE Db = 'mydb';
```

---

### **ขั้นตอนที่ 10: ใช้ Stored Procedure ร่วมกับ Transaction**
Stored Procedure นี้จะ **เพิ่มพนักงานสองคน** และ **Rollback** หากเกิดข้อผิดพลาด

```sql
DELIMITER //

CREATE PROCEDURE AddTwoEmployees(
    IN emp1_name VARCHAR(100), IN emp1_salary DECIMAL(10,2),
    IN emp2_name VARCHAR(100), IN emp2_salary DECIMAL(10,2)
)
BEGIN
    DECLARE EXIT HANDLER FOR SQLEXCEPTION
    BEGIN
        ROLLBACK;
    END;
    
    START TRANSACTION;
    
    INSERT INTO employees (name, salary) VALUES (emp1_name, emp1_salary);
    INSERT INTO employees (name, salary) VALUES (emp2_name, emp2_salary);
    
    COMMIT;
END //

DELIMITER ;
```

เรียกใช้งาน:

```sql
CALL AddTwoEmployees('Alice', 60000, 'Bob', 55000);
```

---

### **สรุป**
Stored Procedure ใน **MySQL 8** เป็นเครื่องมือที่ทรงพลังสำหรับจัดการตรรกะของฐานข้อมูลได้อย่างมีประสิทธิภาพ  
คุณได้เรียนรู้เกี่ยวกับ:
✅ การสร้าง **Stored Procedure**  
✅ การใช้ **พารามิเตอร์ขาเข้าและขาออก**  
✅ การดึงข้อมูลจากฐานข้อมูล  
✅ การแก้ไขและลบ **Stored Procedure**  
✅ การใช้ **Transaction** ภายใน **Stored Procedure**  

หากต้องการข้อมูลเพิ่มเติม บอกได้เลยนะครับ! 🚀

Sure! Here’s a **step-by-step guide** on creating and using **stored procedures** in **MySQL 8**.

---

### **Step 1: Create a Sample Database and Table**
Before creating a stored procedure, let's set up a database and a sample table.

```sql
CREATE DATABASE mydb;
USE mydb;

CREATE TABLE employees (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    salary DECIMAL(10,2)
);
```

---

### **Step 2: Create a Simple Stored Procedure**
Let’s create a stored procedure to **insert a new employee** into the `employees` table.

```sql
DELIMITER //

CREATE PROCEDURE AddEmployee(
    IN emp_name VARCHAR(100),
    IN emp_salary DECIMAL(10,2)
)
BEGIN
    INSERT INTO employees (name, salary) VALUES (emp_name, emp_salary);
END //

DELIMITER ;
```
- `DELIMITER //` changes the delimiter to `//` so MySQL does not execute statements prematurely.
- `IN emp_name` and `IN emp_salary` are **input parameters**.
- `INSERT INTO` inserts data into the table.
- `DELIMITER ;` restores the default delimiter.

---

### **Step 3: Call the Stored Procedure**
Now, let's call the procedure to insert a record:

```sql
CALL AddEmployee('John Doe', 50000.00);
```

---

### **Step 4: Create a Stored Procedure to Fetch Data**
Let’s create a stored procedure to **retrieve all employees**.

```sql
DELIMITER //

CREATE PROCEDURE GetEmployees()
BEGIN
    SELECT * FROM employees;
END //

DELIMITER ;
```

To execute this procedure:

```sql
CALL GetEmployees();
```

---

### **Step 5: Create a Stored Procedure with an Output Parameter**
This procedure will **return the total number of employees**.

```sql
DELIMITER //

CREATE PROCEDURE GetEmployeeCount(OUT emp_count INT)
BEGIN
    SELECT COUNT(*) INTO emp_count FROM employees;
END //

DELIMITER ;
```

To call this procedure:

```sql
CALL GetEmployeeCount(@count);
SELECT @count;
```

---

### **Step 6: Create a Stored Procedure with an Input and Output Parameter**
Let’s create a procedure that **retrieves an employee’s salary** by their name.

```sql
DELIMITER //

CREATE PROCEDURE GetSalary(
    IN emp_name VARCHAR(100),
    OUT emp_salary DECIMAL(10,2)
)
BEGIN
    SELECT salary INTO emp_salary FROM employees WHERE name = emp_name LIMIT 1;
END //

DELIMITER ;
```

To execute this procedure:

```sql
CALL GetSalary('John Doe', @salary);
SELECT @salary;
```

---

### **Step 7: Modify an Existing Stored Procedure**
If you need to modify a stored procedure, first **drop** it and then recreate it.

```sql
DROP PROCEDURE IF EXISTS AddEmployee;
```

---

### **Step 8: Delete a Stored Procedure**
If you want to **delete** a stored procedure:

```sql
DROP PROCEDURE IF EXISTS GetEmployees;
```

---

### **Step 9: List All Stored Procedures**
To see all stored procedures in your database:

```sql
SHOW PROCEDURE STATUS WHERE Db = 'mydb';
```

---

### **Step 10: Use a Stored Procedure Inside a Transaction**
This procedure inserts two employees and rolls back if an error occurs.

```sql
DELIMITER //

CREATE PROCEDURE AddTwoEmployees(
    IN emp1_name VARCHAR(100), IN emp1_salary DECIMAL(10,2),
    IN emp2_name VARCHAR(100), IN emp2_salary DECIMAL(10,2)
)
BEGIN
    DECLARE EXIT HANDLER FOR SQLEXCEPTION
    BEGIN
        ROLLBACK;
    END;
    
    START TRANSACTION;
    
    INSERT INTO employees (name, salary) VALUES (emp1_name, emp1_salary);
    INSERT INTO employees (name, salary) VALUES (emp2_name, emp2_salary);
    
    COMMIT;
END //

DELIMITER ;
```

Call the procedure:

```sql
CALL AddTwoEmployees('Alice', 60000, 'Bob', 55000);
```

---

### **Conclusion**
Stored procedures in MySQL 8 are powerful tools for managing database logic efficiently. You’ve now learned how to:
✅ Create stored procedures  
✅ Use input/output parameters  
✅ Fetch data using stored procedures  
✅ Modify, delete, and list procedures  
✅ Use transactions inside a stored procedure  

Would you like help with anything else? 🚀
