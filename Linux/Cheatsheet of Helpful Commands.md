# Cheatsheet of Helpful Commands

## SED Command
```
sed -i "s/^BROADCAST_DRIVER.*$/BROADCAST_DRIVER=cache/g" .env  
```


## 1. redo last command but as root
```
sudo !!
```
## 2. open an editor to run a command
```
ctrl+x+e
```
## 3. create a super fast ram disk
```
mkdir -p /mnt/ram
mount -t tmpfs tmpfs /mnt/ram -o size=8192M
```
## 4. don't add command to history (note the leading space)
 ```
 ls -l
```
## 5. fix a really long command that you messed up
```
fc
```
## 6. tunnel with ssh (local port 3337 -> remote host's 127.0.0.1 on port 6379)
```
ssh -L 3337:127.0.0.1:6379 root@emkc.org -N
```
## 7. quickly create folders
```
mkdir -p folder/{sub1,sub2}/{sub1,sub2,sub3}
```
## 8. intercept stdout and log to file
```
cat file | tee -a log | cat > /dev/null
```
## 9. cutting and pasting text in the command line
```bash
ctrl-k, ctrl-u, ctrl-w, ctrl-y
```
## bonus: exit terminal but leave all processes running
```
disown -a && exit
```

## 10. Kill ssh session running in the background

```
# 9999 must be replaced with desired port
ps -lef | grep ssh | grep "9999" | awk "{print \$4}" | xargs kill
```

## 11. Execute SSH Tunneling from bastion to private server
```
ssh -L 5901:127.0.0.1:5901 -N -f user@remote.host
# The -f option tells the ssh command to run in the background and -N not to execute a remote command. We are using localhost because the VNC and the SSH server are running on the same host.

```