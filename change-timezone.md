# change timezone

````
SELECT @@global.time_zone;

SELECT NOW();

SET GLOBAL time_zone = '+7:00';
````

## inconfig file 

````
[mysqld]
default-time-zone = "+7:00"
````

## test
````
sudo mysql –e "SELECT @@global.time_zone;"

sudo mysql –e "SELECT NOW();"
````
