---
title: Tips and Tricks in Linux
date: 2023-05-12
tags: [awk, linux, cli, snippet, sudo, folder-size, file-size, cleanup, sed, mkdir]
---

# Tips and Tricks in Linux
---


## Contents
---
- [[#Get Folder File Size]]
- [[#Linux Clean up]]


#### [SED](sed) Command
```
sed -i "s/^BROADCAST_DRIVER.*$/BROADCAST_DRIVER=cache/g" .env  
```


#### 1. redo last command but as root
```bash
sudo !!
```

#### 2. open an editor to run a command
```bash
ctrl+x+e
```

#### 3. create a super fast ram disk
```bash
mkdir -p /mnt/ram
mount -t tmpfs tmpfs /mnt/ram -o size=8192M
```

#### 4. don't add command to history (note the leading space)
 ```bash
 ls -l
```

#### 5. fix a really long command that you messed up
```bash
fc
```

#### 6. tunnel with [ssh](ssh) (local port 3337 -> remote host's 127.0.0.1 on port 6379)
```bash
ssh -L 3337:127.0.0.1:6379 root@emkc.org -N
```

#### 7. Quickly create folders
```bash
mkdir -p folder/{sub1,sub2}/{sub1,sub2,sub3}
```

#### 8. intercept stdout and log to file
```bash
cat file | tee -a log | cat > /dev/null
```

#### 9. cutting and pasting text in the command line
```bash
ctrl-k, ctrl-u, ctrl-w, ctrl-y
```

####  bonus: exit terminal but leave all processes running
```bash
disown -a && exit
```

#### 10. Kill [ssh](ssh) session running in the background

```bash
# 9999 must be replaced with desired port
ps -lef | grep ssh | grep "9999" | awk "{print \$4}" | xargs kill
```

#### 11. Execute [SSH](ssh) Tunneling from bastion to private server
```bash
ssh -L 5901:127.0.0.1:5901 -N -f user@remote.host
# The -f option tells the ssh command to run in the background and -N not to execute a remote command. We are using localhost because the VNC and the SSH server are running on the same host.

```


## Get Folder File Size
---

- The command starts with [`sudo`](sudo) because most of the files and directories inside the /var directory are owned by the root user and are not readable by the regular users. If you omit `sudo` the `du` command will print “du: cannot read directory”.
- `s` - Display only the total size of the specified directory, do not display file size totals for subdirectories.
- `h` - Print sizes in a human-readable format (h).
- `/var` - The path to the directory you want to get the size.

```bash
sudo du -shc /var/*
```


#### Find oldest file in a directory

```bash
cd /path/to/dir
ls -lt | tail -1
```

#### Find newest file in a directory

```bash
cd /path/to/dir
ls -ltr | tail -1
```



## Linux Clean up
---


#### 1. Remove old kernel headers and unnecessary packages

```bash
sudo apt autoremove
sudo apt autoclean
sudo apt clean
```

#### 2. Clear old systemd journal logs

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

#### 3. Use Docker? Remove unused images and containers

```bash
docker rmi images
```

#### 4. Check the biggest directories in your system and see if they are supposed to be that big

```bash
du -h . --max-depth=1 | sort -n -r | head -n 10
```

install log rotate for maintain log files

<https://itsfoss.com/free-up-space-ubuntu-linux/>