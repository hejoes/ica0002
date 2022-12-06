# Backup SLA

## Coverage:

This backup is meant to cover MySQL (located on hejoes-1 and hejoes-2) and InfluxDB (hejoes-3). Other services (nginx, agama, grafana, bind) can be recoved with Ansible and therefore in this backup they are not covered.

## RPO

Backups are made automatically every day at 20:00-20:30(20:00-20:15 MySQL, 20:15-20:30 InfluxDB)UTC(server time). Acceptable data loss: 1hour in 30days. Reqoveries and their verification are responsibility of an sysadmin.

## Retention and versioning

Every saturday full backups are created, every other day only incremental. Backups are stored for 2weeks but can vary if the IT team considers necessary to store them longer.

## Usability checks

We will verify backups in our testing-environment once a week - on thursdays.

## Restoration criteria

When service (DB server) files have been lost or service stops working and can't be fixed without data restoration.

## RTO

Depends on the data loss. Usually takes 1hour. Backup and it's documentation is compiled by the sysadmin. The backup documentation should describe step-by-step on how to restore the system so that the staff outside the IT could also enable the backup when needed. If high automation is achieved, may take less than 30minutes.
