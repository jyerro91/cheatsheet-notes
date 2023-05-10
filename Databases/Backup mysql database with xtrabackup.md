# Backup mysql database with xtrabackup

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

#### Backup
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