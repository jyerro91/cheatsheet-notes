# How to get folder/file size

- The command starts with `sudo` because most of the files and directories inside the /var directory are owned by the root user and are not readable by the regular users. If you omit `sudo` the `du` command will print “du: cannot read directory”.
- `s` - Display only the total size of the specified directory, do not display file size totals for subdirectories.
- `h` - Print sizes in a human-readable format (h).
- `/var` - The path to the directory you want to get the size.

```bash
sudo du -shc /var/*
```


# Find oldest file in a directory

```bash
cd /path/to/dir
ls -lt | tail -1
```

# Find newest file in a directory

```bash
cd /path/to/dir
ls -ltr | tail -1
```