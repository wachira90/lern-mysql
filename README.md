# lern-mysql
lern-mysql

## Step 1 Download

### Windows

````
cd C:\temp
wget https://raw.githubusercontent.com/wachira90/lern-mysql/main/mysqlsampledatabase.sql
````

### Linux

````
cd /tmp
wget https://raw.githubusercontent.com/wachira90/lern-mysql/main/mysqlsampledatabase.sql
````

## Step 2  recheck file exist 

```C:\temp\mysqlsampledatabase.sql```
and
```/tmp/mysqlsampledatabase.sql```

## Step 3 Connect to the MySQL server

````
> mysql -u root -p
Enter password: ********

````
## Step 4 Use the source command to load data into the MySQL Server:

### Windows

````
mysql> source c:\temp\mysqlsampledatabase.sql
````

### Linux

````
mysql> source /tmp/mysqlsampledatabase.sql
````

## Step 5 Use the SHOW DATABASES command to list all databases in the current server:

````
mysql> show databases;

+--------------------+
| Database           |
+--------------------+
| classicmodels      |
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
````

## create db

```sql
CREATE DATABASE `testdb` CHARACTER SET 'utf8' COLLATE 'utf8_general_ci';
```


## shell script backup 

```bash
#!/bin/bash
# Set variables
DB_HOST=localhost
DB_USER=root
DB_PASS=password
DB_NAME=mydatabase
BACKUP_DIR=/path/to/backup/directory
DATE=$(date +%F)

# Create backup directory if it doesn't exist
mkdir -p $BACKUP_DIR

# Create backup filename
BACKUP_FILE=$BACKUP_DIR/$DB_NAME-$DATE.sql.gz

# Dump database and gzip output
mysqldump -h $DB_HOST -u $DB_USER -p$DB_PASS $DB_NAME | gzip > $BACKUP_FILE

# Print success message
echo "Backup of database $DB_NAME completed successfully!"
```

