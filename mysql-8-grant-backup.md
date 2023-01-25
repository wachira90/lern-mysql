# mysql 8 grant backup

One of the easiest ways I've found to export users is using Percona's tool pt-show-grants. The Percona tool kit is free, easy to install, and easy to use, with lots of documentation. It's an easy way to show all users, or specific users. It lists all of their grants and outputs in SQL format. I'll give an example of how I would show all grants for test_user:

````
shell> pt-show-grants --only test_user
````

Example output of that command:

````
GRANT USAGE ON *.* TO 'test_user'@'%' IDENTIFIED BY PASSWORD '*06406C868B12689643D7E55E8EB2FE82B4A6F5F4';
GRANT ALTER, INSERT, LOCK TABLES, SELECT, UPDATE ON `test`.* TO 'test_user'@'%';
````

I usually rederict the output into a file so I can edit what I need, or load it into mysql.
Alternatively, if you don't want to use the Percona tool and want to do a dump of all users, you could use mysqldump in this fashion:

````
shell> mysqldump mysql --tables user db > users.sql
````

Note: --flush-privileges won't work with this, as the entire db isn't being dumped. this means you need to run it manually.

````
shell> mysql -e "FLUSH PRIVILEGES"
````
