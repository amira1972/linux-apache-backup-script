# linux-apache-backup-script

mkdir /backup/apache
chmod 700 /backup/apache
nano /backup/apache_script.sh

################## the script

#!/bin/bash
date=$(date +%F)
apache_data="/var/www/html"
backup_data="/backup/apache"
log="/var/log/backup.log"
backupFile="$backup_data/ApacheBackup_$date.tar.gz"
echo "Backup started" >> $log

if [ -d "$apache_data" ]
then
     echo "source folder exists" >> $log
else
     echo "source folder  not found" >> $log
    exit
fi
 tar -czf $backupFile $apache_data

if [ -f "$backupFile" ]
then 
   echo "apache backup file created">> $log
else
   echo "apache backup file not created" >> $log
   exit
fi 

echo "finished" >> $log
################################# 


chmod +x /backup/apache_script.sh
/backup/apache_script.sh
cat /var/log/backup.log

crontab -e   ====>        5 * * * * /backup/apache_script.sh
crontab -l

ls -lh /backup
