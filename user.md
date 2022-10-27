# MYSQL User Management

mysql 8 change plugin mysql_native_password

## Create user

```CREATE USER 'adminpin'@'%' IDENTIFIED WITH mysql_native_password BY 'password';```

## Edit user plugin

```ALTER USER 'adminpin'@'%' IDENTIFIED WITH mysql_native_password BY 'password';```

## Grant

```GRANT ALL PRIVILEGES ON *.* TO 'adminpin'@'%' WITH GRANT OPTION;```

## Flush

```FLUSH PRIVILEGES;```


## Option

```GRANT CREATE, ALTER, DROP, INSERT, UPDATE, DELETE, SELECT, REFERENCES, RELOAD on *.* TO 'adminpin'@'%' WITH GRANT OPTION;```

