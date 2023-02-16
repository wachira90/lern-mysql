# shell script backup restore

## nano database_backup.sh

````
#!/bin/bash
DB_NAMES=( "authen" "permission" "storage" "ap" "dp" "esaraban" "ess" "mss" "notification" "pa" "py" "rm" "sequencer" "ta" "tr" "wf" )
USERDB="root"
PASSDB="xxxx"
D=$(date '+%Y-%m-%d')
if [ ! -d $D ]; then
  mkdir -p $D
fi
for DB_NAME in "${DB_NAMES[@]}"
do
    mysqldump -u ${USERDB} -p${PASSDB} ${DB_NAME} > $D/${DB_NAME}.sql
done

````

## nano database_backup.sh

````
#!/bin/bash
#Set the databases name
DB_NAMES=( "authen" "permission" "storage" "ap" "dp" "esaraban" "ess" "mss" "notification" "pa" "py" "rm" "sequencer" "ta" "tr" "wf" )
USERDB="root"
PASSDB="xxxx"
for DB_NAME in "${DB_NAMES[@]}"
do
    mysql -u ${USERDB} -p${PASSDB} $DB_NAME < backup1/${DB_NAME}.sql
done

````
