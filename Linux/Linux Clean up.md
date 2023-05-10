# Linux Clean up

## 1. Remove old kernel headers and unnecessary packages

```bash
sudo apt autoremove
sudo apt autoclean
sudo apt clean
```

## 2. Clear old systemd journal logs

You won’t realize but the logs can take up a considerable amount of distance. Most Linux distributions use systemd these days and systemd stores the logs in /var/log/journal.

> You can use the du command to check the size of the /var/log/journal directory:

```bash
du -sh /var/log/journal/
```

> Now, don’t go on and delete the log directory. There is better ways to handle these logs. For example, you can clear all the logs older than ten days in this manner:

```bash
# Check disk usage of logs
journalctl --disk-usage
```


```bash
sudo journalctl --vacuum-time=10d
sudo journalctl --vacuum-size=100M
```

## 3. Use Docker? Remove unused images and containers

```bash
docker rmi images
```

## 4. Check the biggest directories in your system and see if they are supposed to be that big

```bash
du -h . --max-depth=1 | sort -n -r | head -n 10
```

install log rotate for maintain log files

<https://itsfoss.com/free-up-space-ubuntu-linux/>