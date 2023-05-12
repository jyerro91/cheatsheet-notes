---
title: Defining Backup Strategies
date: 2023-05-12
tags: [backup-strategies, mysql, sql, databases]
---

# Defining Backup Strategies
---

## Table of contents
---

- [[#Guide Questions]]
- [[#Backup Levels]]
- [[#Cold vs Hot Backup]]
- [[#Physical vs Logical]]
- [[#Complete vs Partial]]
- [[#Full vs Incremental]]

---


#### Guide Questions
- How the backup is taken?
- How often?
- Where the backup is stored?
- For how much time?
- HOw much time the backup can take?
- How much space the backup can take?
- How much time the restore can take?

#### Backup Levels
- Latest backup stored locally, uncompressed
- 7 Daily backups in S3/spaces


#### Cold vs Hot Backup

##### Cold - Stop [MySQL](databases/mysql.md) and copy data
- Downtime
- Faster

##### Hot - Take backups while MySQL is Running
- Slow Down
- More complex

#### Physical vs Logical

##### Physical - Set of files and directory
- Usually takes more space (indexes are included)
- Doing it while the server is runing requires tools

##### Logical - SQL statements to recreate a dataset
- Can only be HOT
- Can potentially be restored on any MySQL version
- Takes more time to take backup and to restore it
- Inconsistent OR long Transaction

#### Complete vs Partial

##### Complete - All Data

##### Partial - A selected part of data
- One or more databases
- One or more tables from a database
- Result of a SELECT (only Logical backup)

#### Full vs Incremental

##### Full - A backup of data at a certain point in time
- A full backup can take a long time
- Suppose you  take it every 24hrs. In case of disaster, you could lose up to 24hrs of changes

##### Incremental
- Taking an incremental backup normally takes much less time
- It includes the changes that happened after the last full backup
- If there is a hole between the full backup and the oldet change we have, our incremental backups are useless