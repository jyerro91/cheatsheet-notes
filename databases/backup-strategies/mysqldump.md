# mysqldump
---

Backing up databases in [mysql](databases/mysql.md) using mysqldump

#### tl;dr
1. Use the --single-transaction
2. If using utf8mb4, usually it's a good idea to set the --default-character-set=utf8mb4 flag
3. You can gzip the output on the fly


#### Efficient database backup using single-transaction
```bash
mysqldump -u USER -p --single-transaction \
    --default-character-set=utf8mb4 \
    some_database | gzip > some_database.sql.gz
```

##### Transactions
Mysqldump can slow down your database instance because it:
1. Runs queries to export the data
2. Can cause table locking

Additionally, on databases that are in-use, the export can bc inconsistent. For example, if a authors table is dumped out, but then a new Author is created along with a new blog post article BEFORE the articles table is dumped, then mysqldump will export data with inconsistent data. It will have a blog post article related to an author that does not exist in that dump.

To get around this, use `--single-transaction` to export the data within a transaction, which will keep your data consistent.

This also reduces locks on the database tables/rows since the transaction won't block other queries from reading the datbase during the dump.


#### Compressing output
You can use `gzip` to compress the output on the fly.

You can also install the pigz command to get a faster implementation of `gzip` on multi-core machines.

To use it, just pipe the mysqldump outout through `gzip` or `pigz`:

#### Backing up in [[DigitalOcean]] Databases

```conf
#!/usr/bin/env bash

BACKUP_DATE=$(date +"%m.%d.%y-%H.%M.%S")
mysqldump --single-transaction --defaults-extra-file=/home/ubuntu/.my.cnf employees | pigz | aws s3 cp - s3://scaling-laravel-backup/$BACKUP_DATE/employees.sql.gz
```

aws s3 cp index.html s3://imphspacedb01/database_backup/ --endpoint=https://sgp1.digitaloceanspaces.com

#### Copying files to DO Spaces
aws s3 cp <sourcefile/path> s3://imphspacedb01/database_backup/ --endpoint=https://sgp1.digitaloceanspaces.com
