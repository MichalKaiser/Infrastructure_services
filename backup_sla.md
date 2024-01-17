## Coverage
mysql database - backed up daily.  
influxdb log database - backed up daily.  
ansible repository - covers applications.

## RPO
Maximum amount of data that can be lost is one day. Past this is unacceptable data loss.

## Versioning and retention

### MySQL: 
Full backups are done daily at 22:30 UTC and incremental backups are done at 45th minute every 6 hours.

### InfluxDB: 
Full backups are done every sunday at 23:20 UTC and incremental backups are done at 23:35 UTC on every day-of-week from Monday through Saturday.


## Versioning and retention
Backup versions are stored 24 hours before they are replaced with a new backups.

## Usability checks
The whole backup system is automated, therefore it can be considered usable.
To verify the backup usability try to restore the backup by following the restoration rules in the backup_restore.md file.

## RTO
To not cause business damage - backups should be restored within 1 hour and 12 minutes (allowed downtime). Provided service must be availiable at least 95% in a year. 

## Make a backup

### MySql
`sudo -u backup -- mysqldump agama > /home/backup/mysql/agama.sql`
`sudo -u backup -- duplicity --no-encryption full /home/backup/mysql/ rsync://MichalKaiser@backup.cybera.mk/mysql`

### InfluxDB
`Sudo -u backup rm -rf /home/backup/influxdb/*; sudo -u influxd backup -portable -database telegraf /home/backup/influxdb`
`Sudo -u backup duplicity --no-encryption full /home/backup/influxdb/ rsync://MichalKaiser@backup.cybera.mk/influxdb`
