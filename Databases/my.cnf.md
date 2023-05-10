#.my.cnf
```conf
#.my.cnf
[mysqldump]
default-character-set=utf8mb4
single-transaction

[clientdoprod]
host = db-mysql-sgp1-45396-do-user-2773464-0.db.ondigitalocean.com
user = doadmin
password = w7lanxnfzq8bja8q
port = 25060

[clientawsdev]
host = imgbsdevdb01.ctqgnm8dfdyn.ap-southeast-1.rds.amazonaws.com
user = admin
password = xUwghwqvzOFsfNFjoYzg
port = 3306

[clientawsstag]
host = imgbsstagdb01.ctqgnm8dfdyn.ap-southeast-1.rds.amazonaws.com
user = admin
password = laueJxdPpjUgLyweQQn2
port = 3306

[clientawsprod]
host = imgbsproddb01.ctqgnm8dfdyn.ap-southeast-1.rds.amazonaws.com
user = admin
password = Gz6jFRLV4ImexJAl0d0y
port = 3306
```

To use:

```bash
mysql --defaults-group-suffix=doprod
mysqldump --defaults-group-suffix=doprod --set-gtid-purged=OFF
```
> #--set-gtid-purged=OFF is used in when dumping mysql databases in digitalcoean