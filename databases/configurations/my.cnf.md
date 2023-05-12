# .my.cnf
---
This is the main configuration to customize connection and other related to [mysql](databases/mysql.md). To use this create a .my.cnf your use home folder.

```conf
#.my.cnf
[mysqldump]
default-character-set=utf8mb4
single-transaction

[clientdoprod]
host = ****.ondigitalocean.com
user = doadmin
password = ****
port = 25060

[clientawsdev]
host = ****.rds.amazonaws.com
user = admin
password = ****
port = 3306

[clientawsstag]
host = ****.rds.amazonaws.com
user = admin
password = ****
port = 3306

[clientawsprod]
host = ****.rds.amazonaws.com
user = admin
password = ****
port = 3306
```

To use:

```bash
mysql --defaults-group-suffix=doprod
mysqldump --defaults-group-suffix=doprod --set-gtid-purged=OFF
```
> #--set-gtid-purged=OFF is used in when dumping mysql databases in digitalcoean