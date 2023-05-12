# xtrabackup
---
Backup and Restore databases in [mysql](databases/mysql.md) using xtrabackup.

## Backup
---

It backs up MySQL's data files themself, instead of creating a file of queries like mysqldump does.

This has some advantages:

1. It only run a few initial queries against the database to get some information, otherwise the load on the server is just file I/O - it doesn't slow down MySQL
2. It can make consistent backups, usually even with MyISAM table types
3. Restoring from xtrabackup can be faster than mysqldump
4. When used with binary log files enabled, it can aid with creating point in time restorations

#### Install

We can install Percona's Xtrabackup with their `apt` repository for Ubuntu/Debian servers:

```bash
wget https://repo.percona.com/apt/percona-release_0.1-4.$(lsb_release -sc)_all.deb
sudo dpkg -i percona-release_0.1-4.$(lsb_release -sc)_all.deb
sudo apt-get update
sudo apt-get install -y percona-xtrabackup-24
```

#### Backup command
```bash
# Backup mysql from its data directory, likely /var/lib/mysql
# Place backups in /data/backups
sudo xtrabackup -u USER -p --backup --target-dir=/data/backups/
```

This will create a backup and place it in the local drive at `/data/backup`. Note that you'll need enough disk space free to hold the mysql data and the backup(s) to do this.

You can compress that backup as well when complete. Here we'll "tar" and "gzip" the backup saved to file `/data/backups/xtrabackup-backup`:

```bash
cd /data/backups

# Gzip whatever backup directory "xtrabackup"
# created in the /data/backups directory
tar -cj xtrabackup-backup.tar.gz xtrabackup-backup
```

#### Extra Configuration

We can place additional xtrabackup configuration in local user's config file `~/.my.cnf`. In this example, we set the username and password so we don't need use the `-u` and `-p` flags when we run te command, just like we can do for `mysqldump`.

```bash
; file ~/.my.cnf
[xtrabackup]
user=user
pass=secret
```

## Restore 
---

Restoring a xtrabackup backup involves replacing your mysql data directory. This is usually found at `/var/lib/mysql`.

To restore, there are three steps:

1. Get the backup
2. Prepare the backup
3. Restore the backup

#### Get the backup

First, download the backup files (if they are noton the server). We'll need to uncompress them if the backup was compressed, and place them on the server where the restoration will take place.

Here we assume the backup was `tar`ed and `gzip`'ed:

```bash
tar -xf xtrabackup-backup.tar.gz
```

#### Prepare the Backup

Next, we'll prepare the backup for restoration.

This can be done on any server, so if you have a utility server (hopefully with the same version of MySQL), that might be a good place to prepare it.

Here's what preparing the backup (at location /data/backups/my-backup) looks like:

```bash
xtrabackup --prepare --target-dir=/data/backups/my-backup
```
> Preparing the backup tells Xtrabackup to change the backup so that MySQL does not think it's corrupt. More information on that here.

#### Restore the Backup

Lastly, on the same server you are restoring to, assuming the files are on the server, you can replace the /var/lib/mysql directory with the prepared backup files.

```bash
# Move current mysql data directory out of
# the way if it exists and has files in it
cd /var/lib
sudo mv mysql mysql-pre-restore
sudo mkdir mysql

# Restore the prepared backup to the data directory
sudo xtrabackup --copy-back --target-dir=/data/backups/my-backup

# Ensure files are owned by user/group `mysql`
sudo chown -R mysql:mysql /var/lib/mysql
```

> You don't need to use the xtrabackup --copy-back method, you can actually just copy the prepared files into /var/lib/mysql yourself! (Or whatever file path your MySQL data files are saved in).

Once you move the old data directory out of the way (if it exists and has files inside of it), you can create an empty one, and then have xtrabackup use the --copy-back command to copy the restore back into place.

Make sure the restored files are owned by user/group mysql when you're done.

Then you can restart mysql and be on your way:

```bash
sudo service mysql restart
```