Coverage: This backup is meant to cover MySQL and InfluxDB. Other services (nginx, agama, grafana, bind) can be recoved with Ansible and therefore in this backup they are not covered. MYSQL at the moment is in remote2 and InfluxDB is in remote1 host

RPO: Backups are made automatically every day at 04:00-04:50(04:00-04:25 MySQL, 04:25-04:50 Influx)EET. Acceptable data loss: 1hour in 30days.

Retention and versioning: Every sunday full backups are created, every other day only incremental. Backups are stored for 2weeks but can vary if the IT team considers necessary to store them longer.

Usability checks: We will verify backups in our testing-environment once a week - on thursdays.

Restoration criteria: When service (DB server) files have been lost or service stops working and can't be fixed without data restoration.

RTO: Depends on the data loss. Usually takes 1hour. If we can achieve high automation, takes less than 30minutes.
