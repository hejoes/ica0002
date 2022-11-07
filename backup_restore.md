#Restore Mysql or InfluxDB:

##Install and configure infrastructure with Ansible:
ansible-playbook infra.yaml

##To see automatic full and incremental backup schedule, refer to backup_sla.md or open crontab (on hejoes-2) using:
```nano /etc/cron.d/mysql-backup```
To change crontab schedule, open crontab guru for reference: https://crontab.guru/


##Restore databases:
Login as root user: ```sudo su```

* Restore Agama Mysql data from previous backup:
  Login to server, where MSQL is located (hejoes-2). Change to user root and type:
    ```rm -rf /home/backup/restore/mysql/*```
    ```sudo -u backup duplicity --no-encryption restore rsync://hejoes@backup.nexify.it//$HOME/mysql /home/backup/restore/mysql/```
    ```mysql agama < /home/backup/restore/mysql/agama.sql```

* Restore InlufDB telegraf data from previous backup:
  Login to server where InfludDB is located (hejoes-2). Change to user root and type:
    ```systemctl stop telegraf```
    ```influx -execute 'DROP DATABASE telegraf'```
    ```sudo -u backup duplicity --no-encryption restore rsync://hejoes@backup.nexify.it//$HOME/influxdb/ /home/backup/restore/influxdb/```
    ```influxd restore -portable -database telegraf /home/backup/restore/influxdb/```
    ```systemctl start telegraf``

##Checking if backup was successful:
- Make sure that any of the previous restore commands didn't give errors
- Check (as root) that MYSQL machine tables have restored: ```mysql -e 'SHOW DATABASES'```
- Check (as root) that InfluxDb tables have been restored: ```influx -execute 'show databases'``` 

