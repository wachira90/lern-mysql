# GRANT DATABASE TO USER 

## CREATE USER

```sql
CREATE USER 'wachira'@'%' IDENTIFIED WITH caching_sha2_password BY 'example1234';
ALTER USER 'wachira'@'%' REQUIRE NONE WITH MAX_QUERIES_PER_HOUR 0 MAX_CONNECTIONS_PER_HOUR 0 MAX_UPDATES_PER_HOUR 0 MAX_USER_CONNECTIONS 0;
```

## REVOKE ALL DATABASE

```sql
REVOKE ALL PRIVILEGES ON *.* FROM 'wachira'@'%';
```

## GRANT DATABASE TO USER

```sql
GRANT ALL PRIVILEGES ON `database-restaurant`.* TO 'wachira'@'%';
GRANT ALL PRIVILEGES ON `database-prod`.* TO 'wachira'@'%';
GRANT ALL PRIVILEGES ON `database-admin`.* TO 'wachira'@'%';
```

## FLUSH

```sql
FLUSH PRIVILEGES;
```
