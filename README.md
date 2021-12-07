# lern-mysql
lern-mysql

## Step 1 Download
````
wget https://raw.githubusercontent.com/wachira90/lern-mysql/main/mysqlsampledatabase.sql
````

## Step 2  unzip the file to the C:\temp

## Step 3 Connect to the MySQL server

````
> mysql -u root -p
Enter password: ********

````
## Step 4 Use the source command to load data into the MySQL Server:

````
mysql> source c:\temp\mysqlsampledatabase.sql
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
````
CREATE DATABASE `testdb` CHARACTER SET 'utf8' COLLATE 'utf8_general_ci';
````
