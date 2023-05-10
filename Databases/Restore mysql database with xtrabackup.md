# Restore mysql database with xtrabackup

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