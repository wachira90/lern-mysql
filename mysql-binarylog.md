ใน MySQL 8 "Binary Log" หรือ "binlog" คือไฟล์ที่ใช้บันทึกกิจกรรมต่างๆ ที่เกิดขึ้นในฐานข้อมูล เช่น:

- การเปลี่ยนแปลงข้อมูล (INSERT, UPDATE, DELETE)
- การสร้างหรือลบตาราง/ดาต้าเบส
- การเปลี่ยนแปลงโครงสร้างตาราง
- คำสั่ง DDL (Data Definition Language)

### จุดประสงค์หลักของ Binary Log:

1. **Replication** (การทำสำเนาฐานข้อมูล):
   - ใช้สำหรับส่งข้อมูลจาก **Primary Server (Master)** ไปยัง **Replica (Slave)** เพื่อทำให้ฐานข้อมูลทั้งสองฝั่งเหมือนกัน

2. **Point-in-Time Recovery (PITR)**:
   - ใช้สำหรับกู้ข้อมูลย้อนกลับไปยังจุดเวลาหนึ่ง (เช่น หลังจาก restore จาก backup แล้ว replay binlog เพื่อกลับไปยังจุดก่อนระบบล่ม)

---

### การเปิดใช้งาน Binary Log:
ต้องเปิดในไฟล์ config (`my.cnf` หรือ `my.ini`):

```ini
[mysqld]
log-bin=mysql-bin
server-id=1
```

---

### ประเภทของ Binlog Formats (3 แบบหลัก):

1. **STATEMENT** – บันทึก SQL statement (ค่า default ในบางเวอร์ชันเก่า)
2. **ROW** – บันทึกค่าที่เปลี่ยนแปลงในแต่ละ row
3. **MIXED** – ใช้ทั้งสองแบบผสมกัน

เช่น:

```ini
binlog_format = ROW
```

---

### คำสั่งที่ใช้ดู Binary Log:

- ดูรายการไฟล์ binlog:

  ```sql
  SHOW BINARY LOGS;
  ```

- ดูไฟล์ binlog ที่กำลังใช้:

  ```sql
  SHOW MASTER STATUS;
  ```

- อ่านเนื้อหาใน binlog:

  ```bash
  mysqlbinlog mysql-bin.000001
  ```

---

อยากใช้ binlog ไปทำอะไรต่อเหรอ? replication, recovery หรือแค่อยากรู้ระบบ log ของ MySQL เฉยๆ?

### =======================

เยี่ยมเลย! การใช้ **Binary Log** สำหรับ **Replication** และ **Recovery** เป็นเรื่องสำคัญในการบริหารจัดการฐานข้อมูล MySQL โดยเฉพาะในระบบที่ต้องการความเสถียรและความปลอดภัยของข้อมูลสูง เดี๋ยวอธิบายแยกเป็น 2 ส่วนให้เข้าใจง่าย ๆ:

---

## 🌀 **1. Replication (การจำลองข้อมูล)**

### คืออะไร?
Replication คือการคัดลอกข้อมูลจาก **Primary (Master)** ไปยัง **Replica (Slave)** แบบอัตโนมัติ เพื่อใช้:

- กระจายโหลดการอ่าน (Read Scaling)
- ทำ High Availability
- ใช้สำรองข้อมูลในกรณีที่เครื่องหลักมีปัญหา

---

### ขั้นตอนสั้น ๆ:

#### 1. เปิด Binary Log บน Master:
```ini
[mysqld]
server-id=1
log-bin=mysql-bin
```

#### 2. สร้าง user สำหรับ replication:
```sql
CREATE USER 'repl'@'%' IDENTIFIED BY 'password';
GRANT REPLICATION SLAVE ON *.* TO 'repl'@'%';
```

#### 3. ตรวจสอบสถานะ binlog:
```sql
SHOW MASTER STATUS;
```
- จะได้ `File` กับ `Position` เอาไว้ใช้ตอนตั้งค่าบน Slave

#### 4. ตั้งค่า Replica (Slave):
```ini
[mysqld]
server-id=2
```

จากนั้นใน MySQL (ฝั่ง Replica):

```sql
CHANGE MASTER TO 
  MASTER_HOST='ip_master',
  MASTER_USER='repl',
  MASTER_PASSWORD='password',
  MASTER_LOG_FILE='mysql-bin.000001',
  MASTER_LOG_POS=12345;

START SLAVE;
```

#### 5. ตรวจสอบสถานะ:
```sql
SHOW SLAVE STATUS\G
```

---

## 🔁 **2. Point-in-Time Recovery (การกู้ข้อมูลตามช่วงเวลา)**

### คืออะไร?
ใช้เมื่อคุณต้องการ **ย้อนข้อมูลกลับไปยังเวลาที่ต้องการ** เช่น:

- กู้จากการเผลอลบข้อมูล
- Restore ข้อมูลหลัง crash

---

### ขั้นตอน:

#### 1. เตรียม Full Backup (เช่นจาก `mysqldump`):
```bash
mysqldump -u root -p --all-databases --master-data > full-backup.sql
```
- `--master-data` จะบันทึกตำแหน่ง binlog ไว้ด้วย

#### 2. Restore จาก backup:
```bash
mysql -u root -p < full-backup.sql
```

#### 3. ใช้ `mysqlbinlog` เพื่อ replay binlog:
```bash
mysqlbinlog --start-datetime="2025-03-30 12:00:00" \
            --stop-datetime="2025-03-30 12:30:00" \
            /var/lib/mysql/mysql-bin.000001 | mysql -u root -p
```

- คุณสามารถเลือกช่วงเวลา หรือ ใช้ `--start-position` / `--stop-position` ก็ได้

---

ถ้าสนใจ เดี๋ยวสรุปเป็น Flow Chart หรือ config ไฟล์ตัวอย่างให้ได้นะ  
หรือจะให้ช่วยเขียนสคริปต์ auto setup เลยก็ได้ 😄

อยากโฟกัสที่ Replication หรือ Recovery ก่อนดี?
