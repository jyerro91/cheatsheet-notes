# Offsite backup of mysql database

```conf
#!/usr/bin/env bash

BACKUP_DATE=$(date +"%m.%d.%y-%H.%M.%S")
mysqldump --single-transaction --defaults-extra-file=/home/ubuntu/.my.cnf employees | pigz | aws s3 cp - s3://scaling-laravel-backup/$BACKUP_DATE/employees.sql.gz
```

aws s3 cp index.html s3://imphspacedb01/database_backup/ --endpoint=https://sgp1.digitaloceanspaces.com

#### Copying files to DO Spaces
aws s3 cp <sourcefile/path> s3://imphspacedb01/database_backup/ --endpoint=https://sgp1.digitaloceanspaces.com