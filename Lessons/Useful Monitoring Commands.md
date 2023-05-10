# Useful Monitoring Commands


htop
glances

##### Check auth log

```bash
cat /var/log/auth.log
```

##### Check last log

```bash
# Show listing of last logged in users
# This read the /var/log/wtmp
last -aiF
```

```bash
This will show the last failed logins
lastb -adF
```

##### Check last login of user

```bash
lastlog -u $USER
```

##### Advance who

```bash
#install whowatch
```