## Backup Strategy and Documentation

### Coverage
- **MySQL Database**: Backed up every 6 hours daily, ensuring minimal data loss.
- **InfluxDB Log Database**: Backed up daily, with extended historical data coverage.
- **Ansible Repository**: Covers applications, ensuring configuration and codebase safety.

### Recovery Point Objective (RPO)
- **MySQL**: Maximum potential data loss is 6 hours, due to incremental backups every 6 hours.
- **InfluxDB**: Maximum potential data loss is 24 hours, with daily incremental backups.

### Versioning and Retention
- **MySQL**: 
  - Full backups daily at 22:30 UTC.
  - Incremental backups every 6 hours.
  - Retention of backups for 7 days, offering a week's worth of recovery options.
  - Total of 35 versions (7 full backups, 28 incremental backups).
- **InfluxDB**: 
  - Full backups every Sunday at 23:20 UTC.
  - Incremental backups daily at 23:35 UTC, Monday through Saturday.
  - Retention of backups for 30 days, providing extended recovery options for log data.
  - Total of 30 versions (4 full backups, 26 incremental backups).

### Usability Checks
- Regular testing of the backup restoration process is mandatory to ensure data integrity and recovery capabilities. For this resons is use od restoration tested every sunday for both backup - Mysql and InfluxDB.
- Follow the guidelines in the `backup_restore.md` file for detailed restoration procedures.

### Recovery Time Objective (RTO)
- Aim to restore backups within 6 hours to prevent significant business disruption.

    - Identifying the issue: 1 hour
    - Initiating the recovery process: 1 hour
    - Restoring data from backup: 2 hours
    - System checks and validation: 1 hour
    - Communication and coordination: 1 hour

### Backup Commands
- **MySQL**: 
  ```ruby
    sudo -u backup -- mysqldump agama > /home/backup/mysql/agama.sql
    ```
  ```ruby
    sudo -u backup -- duplicity --no-encryption full /home/backup/mysql/ rsync://MichalKaiser@backup.cybera.mk/mysql
    ```
- **InfluxDB**: 
  ```ruby
   sudo -u backup rm -rf /home/backup/influxdb/*; sudo -u influxd backup -portable -database telegraf /home/backup/influxdb`
  ```
  ```ruby
   sudo -u backup duplicity --no-encryption full /home/backup/influxdb/ rsync://MichalKaiser@backup.cybera.mk/influxdb`
   ```