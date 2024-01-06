## Coverage
Backup coverage is mysql database - backed up daily.  
influxdb log database - backed up daily.  
ansible repository - covers applications.

## RPO
Maximum amount of data that can be lost is one day. Past this is unacceptable data loss.

## Versioning and retention

### MySQL: 
Full backups are done daily at 23:10 UTC and incremental backups are done at 23:10 UTC on every day-of-week from Monday through Saturday.

### InfluxDB: 
Full backups are done daily at 22:10 UTC and incremental backups are done at 22:10 UTC on every day-of-week from Monday through Saturday.


## Versioning and retention
Versioning and retention describe how many backup versions are stored and for how long.

Backup versions are stored 24 hours before they are replaced with a new backups.

## Usability checks
Usability checks describe how is backup usability verified.

The whole backup system is automated, therefore it can be considered usable.
To verify the backup usability try to restore the backup by following the restoration rules in the backup_restore.md file.

## RTO
To not cause business damage - backups should be restored within 1 hour and 12 minutes (allowed downtime). Provided service must be availiable at least 95% in a year. 

## Make a backup

`sudo runuser -u backup -- mysqldump agama > /home/backup/mysql/agama.sql`
`sudo runuser -u backup -- duplicity --no-encryption full /home/backup/mysql/ rsync://redubr@backup.infra.rd/mysql`
