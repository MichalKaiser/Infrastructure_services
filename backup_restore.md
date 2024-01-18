# Restore mysql

```ruby
ansible-playbook infra.yaml
```

### As backup user

#### Download backup from server
```ruby
sudo runuser -u backup -- duplicity --no-encryption restore rsync://MichalKaiser@backup.cybera.mk/mysql /home/backup/restore/mysql`
```

### as root user:
```ruby
mysql agama < /home/backup/restore/mysql/agama.sql`
```

# Restore influxDB telegraf database

#### Download backup as backup user from backup server
```ruby
sudo runuser -u backup --  duplicity --no-encryption restore rsync://MichalKaiser@backup.cybera.mk/influxdb /home/backup/restore/influxdb
```

### As root user stop telegraf service, delete telegraf database, restore and restart telegraf service

### stop telegraf service
```ruby
service telegraf stop`  
```

### delete telegraf database
```ruby
influx -execute 'DROP DATABASE telegraf'`  
```

### restore influxdb database
```ruby
influxd restore -portable -database telegraf /home/backup/restore/influxdb`  
```

### start telegraf service
```ruby
service telegraf start`  
```

### Play infra.yaml again
```ruby
ansible-playbook infra.yaml
```