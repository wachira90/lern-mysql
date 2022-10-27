# user management

mysql 8 change plugin mysql_native_password

## create user

```CREATE USER 'adminpin'@'%' IDENTIFIED WITH mysql_native_password BY 'password';```

## edit user plugin

```ALTER USER 'adminpin'@'%' IDENTIFIED WITH mysql_native_password BY 'password';```

## grant

```GRANT ALL PRIVILEGES ON *.* TO 'adminpin'@'%' WITH GRANT OPTION;```

## flush

```FLUSH PRIVILEGES;```


## for option

```GRANT CREATE, ALTER, DROP, INSERT, UPDATE, DELETE, SELECT, REFERENCES, RELOAD on *.* TO 'adminpin'@'%' WITH GRANT OPTION;```

