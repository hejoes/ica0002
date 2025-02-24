# Restore Mysqll or InfluxDB:

## Install and configure infrastructure with Ansible:

ansible-playbook infra.yaml

## To see automatic full and incremental backup schedule, refer to backup_sla.md or open crontab (on hejoes-2) using:

`nano /etc/cron.d/mysql-backup`
To change crontab schedule, open crontab guru for reference: https://crontab.guru/

## Restore databases:

Login as root user: `sudo su`

- Restore Agama Mysql data from previous backup:
  Login to MYSQL master (not read only) (default is hejoes-2, if not, check grafana mysql dashboard to see which machine is mysql master) with root user:

  1.  `rm -rf /home/backup/restore/mysql/*`
  2.  `sudo -u backup duplicity --no-encryption restore rsync://hejoes@backup.nexify.it//home/hejoes/mysql /home/backup/restore/mysql/`
  3.  `mysql agama < /home/backup/restore/mysql/agama.sql`

- Restore InfluxDB telegraf data from previous backup:
  Login to server where InfludDB is located (hejoes-3)

  Change to user root and type:

  1.  `service telegraf stop`
  2.  `influx -execute 'DROP DATABASE telegraf'`
  3.  `sudo -u backup duplicity --no-encryption restore rsync://hejoes@backup.nexify.it//home/hejoes/influxdb/ /home/backup/restore/influxdb/`
  4.  `influxd restore -portable /home/backup/restore/influxdb/`
  5.  `service telegraf start`

## Checking if backup was successful:

- Make sure that any of the previous restore commands didn't give errors
- Check (as root) that MYSQL machine tables have restored: `mysql -e 'SHOW DATABASES'`
- Check (as root) that InfluxDb tables have been restored: `influx -execute 'show databases'`
