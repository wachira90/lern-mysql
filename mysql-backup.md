# mysql backup

## To create a backup of all MySQL server databases, run the following command:
````
mysqldump --user root --password  --all-databases > all-databases.sql

mysqldump -u root -p classicmodels > classicmodels.sql
````

## To recover data, use the following command:
````
mysql --user root --password mysql < all-databases.sql

mysql -u root -p pro_kk < pro_kk.sql

````

## Often you need to backup not the entire server, but a specific database. To dump a specific database, use the name of the database instead of the –all-database parameter.
````
mysql --user root --password [db_name] < [db_name].sql

````
