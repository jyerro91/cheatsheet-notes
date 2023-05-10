# System Admin Basic

w - who
who - who less details
htop
top
netstat -tulpn - for viewing open ports and programs
- t: for TCP
- u: for UDP
- l: listening socket
- p: for program/processes attach to the socket
- n: 

## Pipes and Redirection

- STDIN == 0
- STDOUT == 1
- STDERR == 2

> This will send the error output to an `error.txt` file
```bash 
command 2> error.txt
```

less - paginates output using pipe
```bash
ps aux | less
```

## Filtering Output and Finding Things (&&, cut, sort, uniq, wc, grep)

- `&&`: if first command successfully run then execute the next command 
- `wc`: word count from file
- `grep`: filter or search any input
- `cut`: cutting input 
```bash
cat file | cut -d: -f1
-d: means delimeter
-f1 means first word
```

## User Account Management

```bash
tail /etc/passwd
```

`/etc/shadow`: stores user password information
`/etc/group`: stores user group information


```bash
useradd -m -d /home/testuser -s /bin/bash testuser
```

```bash
# delete user
userdel user
```

```bash
# set password to user
passwd user
```

```bash
# locks the user
usermod -L user
```

```bash
# unlocks the user
usermod -U user
```

## All about Process

#### Overview

`PROCESSES`: basically a program that runs

"has 2 parts" :
1. `address space` that it can use, where it can write to
2. `kernel data` structure keeping following information :

* `PID` : primary key for processes. PID are unique
* `parent id` : pid of process that started this one, if parent dies then child process is reparented to "init" process
* `UID` : tells which user owns the process (same for groupID)
* `EffectiveUserID` : (EUID) process spawned by user but shouldn't have same permissions as user (same for EgroupID)
* `Niceness` : if NI high then it's low priority and let's lower NI processes take resources

Life cycle of a process :
created by parent process by forking itself,
giving child that starts other programs/processes
death : exiting with return value given to parent process

case of init: (for ubuntu)
started at kernel boot, runs all startup scripts

#### Process Signals

Signals can be sent by the kernel when:
1. a process something REALLY BAD. (AKA Divide by Zero)
2. to notify a parent process of the death of a child process
3. to notify that a process is ready 

`Test Signals`: How processes communicate information about themselves to each other and the kernal (and how the kernal communicates back to processes)

- `SIGKILL (9)`: Kills a process without asking the process to stop (use it on a process that hasn't responded to a SIGTERM)
- `SIGTERM (15)`: Default when using `kill` command. Asks a process to stop/end


Most processes can be blocked/ignored/handled by...

- Handler processes inside of a function (you would use this if a process runs without a valid reason)


*What is the command that is used to signals to processes?*
```bash
kill
# (Reason its called kill: it automatically tries a 'kill 15' 
# if you don't specify anything)
```

*What does "kill -l" show you?*

> a list of the different processes & corresponding numbers

*If you are 'grep'ing for processes in 'ps', you will always see these two things:*

> 1. the process that has been launched looking for the process using your search term
> 2. the actual process


*Why can a 'kill 9' command be dangerous?*

> *kill 9 = Die immediately*, could cause problems if you have files open
Only use when a problem isn't behaving


*What does a 'control + C' key combination do?*

> An interrupt command
(aka halt/stop process)


*What does the 'killall' command do?*

> It kills all the processes started (specified by name)


*What would the command 'sudo pkill -u testusr' do?*

> It would kill all the processes initiated by the user: testusr
(don't do it on the user you are currently logged in as, funky things happen)


> pgrep & pkill

> More refined ways of searching and killing commands (respectively)

> (check more out with 'man 7 signal')   

#### State, Niceness, and How to Monitor Processes

*What is the proc filesystem?*
> Process Filesystem - virtual filesystem the linux kernel mounts to post information about its processes so that applications have easy access to them

*Every Process wants what?* **CPU time**

*What is responsible for: scheduling what process get CPU time when?*
> The kernel - Determines this based on what state the process is in.

*What are the 4 states a process can be in?*

> 1. Run-able - Eligible to be scheduled for CPU time, has all the info it needs
> 2. Sleeping - Waiting for something
> 3. Zombie - Finished with what it was doing, waiting to give back info it has come up with & be killed by kernel
> 4. Stopped - May have been in the middle of doing something BUT received stop signal and is now waiting for the continue signal to resume


*If you have a lot of Zombie processes causing an issue, what would be useful to check for?*
> Parent PID (PPID) the issue could be that the parent hasn't collected finished output of the processes and the children can't die

The most common tool/command to monitor processes is...
1. `top`
> (command: automatically sorts by % of cpu use, # of things running, states of processes, etc)
> Besides the 'top' command, what command can show you MORE detailed and granular info about processes, resource use and process states?
2. `htop` (command, you may need to install)
- nicer interface
- allows you to scroll
- can scroll to a process and send a signal to it directly
- more features, read man page

*What should be your first step in exploring any new program?*
> Look at the man page
> - familiarize yourself with the options available
> - see if there are any examples at the end

*What is Niceness (NI)?*
> A value that determines how 'nice' a process is to other processes (the priority of the process)
> With Niceness, the HIGHER the number...
> the LOWER the priority (the #s go from -20 [SUPER HIGH] to 19 [lowest priority])
> - Ex:
> Low priority command: nice - n 15 /backup/not/important/at/all/task

Niceness and Customer facing systems: with anything like this, you want to be careful not to....
...run things that will run things that will grind everything else to a halt
(because you might want to set a lower niceness level)
- too great a level might make your system feel like it is freezing up
- generally, you don't want to set anything to the minimum niceness/highest priority b/c it will block other stuff

*How do you renice (change priority) of a task?*
```bash
# Syntax: 
renice (-20 to 20)  PID
```
(make sure that you have appropriate privileges)
- you can also change it with htop with F7 or F8

Example Problem: You are on a new system and it has slowed to a craw. 
What do you do?
1. Open up top (or htop)
2. What is taking up the MOST memory?
3. How much memory is free?
4. What is taking up the highest % of CPU usage?
5. Are there a lot of zombie processes (frozen parent)?

#### The /proc Filesystem

*What do you use 'ps' for?*

> listing processes


*The kernel automatically monitors the state of each process via a virtual file system called:*

> /proc filesystem (currently running/active processes)

*cwd*

> get path name of current working directory / show you where the process is operating from
(similar to pwd but without the trailing line terminator)

*What does 'fd' in 'ls fd' do?*

> List file descriptors for you

*What does 'map' in 'ls map' do?*

> List memory mapping info for you (if you wanna see what shared libraries it is using, more at 6:14)


The file 'cmd' will generally be what ______ the process is currently executing.

The 'cmdline' is generally how the process was ___________.


Remember: quite a bit of this stuff isn't really legible in its native form, it is meant to be used in conjunction with other programs like htop.
You start to appreciate the functionality that is built into linux/BASH commands here.

Command *strace*

Attach two processes & really see what they are doing
(in a way that is very difficult otherwise)
- this is a vital tool for sysadmins, you NEED this

--

Tell me if I missed anything. Have a great day and be safe.

## Filesystem and Absolute/Relative Pathnames

`Relative path` is the path relative to your current folder location

`Absolute path` is the full path direction of the directory or file. It works from anywhere.


## Filesystem Layout Overview

*ROOT ( / )*

Top of the filesytem tree

*What is **/bin**?*

Where your base operating commands are *
Unix = JUST base commands (ie: in freeBSD)
Linux = Links added here when you install programs

In LINUX - Base system put together from disparate sources
In UNIX - Tightly integrated system
(at least when it comes to /bin)

*What is **/boot?***

- Here you have kernel, kernal files, & boot loader (ie:grub)
- This Is Linux (its beautiful)

*What is **/dev**?*

All of your devices.
- HDD is here (under 'sd' moniker)
- CD-ROM, etc

*What is **/etc**?*

- system configuration
- all the extra stuff you have installed with your package manager

*What is **/home**?*

The home for all the users that are not root
(have all their files here)

*What is **/lib**? (and /lib64)*

Houses libraries that programs on your system use.
(they are called DLL on windows, code that any software can use)
* each can be used by multiple programs

*What is **/media**?* (on ubuntu)

Mount directory where automounted items go (ON UBUNTU).
* usually only root can mount things, but can mount a CD-ROM with this without root privilages

*What is **/mnt**?*

Mount directory where automounted items go.
(*Used to mount items like CD-ROMs, USB, HDD on most systems)


*What is **/opt**?*

Optional software 
(you can throw extra software here if you aren't installing through the package manager)
- not used too much 

*What is **/proc**?*

Has directory and configuration files for every program on your system.
(Virtual filesystem that the kernel uses to communicate with you and any other program on your machine about the state of processes that are running)

*What is **/root**?*

Root's home


*What is **/sbin**?*

Critical files that your machine needs to get up and running

*What is **/tmp**?*

temporary files here, temporary space for holding stuff
(emptied upon reboot)


*What is **/usr**?*
A subdivided list of non-super-essential files and commands

include 
(under /usr)

file headers, for writing/compiling code


*What would you put in the **/usr/share**?*

Software/programs that you would want when you re-imaged


*What is **/var/log**?*

This has all of your system logs in it.

--- 


## Tmux - Windows, Panes and Sessions

*Windows*

```bash
tmux 
# Create new window
ctrl+b c
# Rename window
ctrl+b ,
# Navigate windows
ctrl+b n or p
# List windows
ctrl+b w
```

*Panes*

```bash
# Vertical pane
ctrl+b % 
```

*Session*

```bash
# Create new session
tmux new -s SESSION_NAME
# List sessions
tmux list-sessions
# Detach from session
ctrl+b d
# Attach to session
tmux attach -t SESSION_NAME


```
---

## $PATH

```bash
echo $PATH
# append to path
PATH=$PATH:/path/to/dir
# prepend to path
PATH=/path/to/dir:$PATH
```

*Always use an absolute path*

---

## Script Command


```bash
# Simplest Way to Log Shell Sessions
script simplescript.log

# A single command
script -c 'netstat -tupln' netstat.log

# Add timing information, so you can replay the shell session later
script myscript.log --timing=time.log

# Replay a captured shell session (with timing info)
scriptreplay -s myscript.log --timing=time.log
scriptreplay -s myscript.log -t time.log
```
---

## LSOF Command
1:00 Which files are open?
lsof

2:56 Which processes have this file open?
lsof /var/log/nginx-error.log

4:49 Which files does process X have open?
lsof -p 1
lsof -p `pgrep ABC`

7:08 Where is the binary for this process?
lsof -p ABC | grep bin

7:44 Which shared libraries is this program using? (manually upgrading software, i.e. openssl)
lsof -p PID | grep .so

8:03 Where is this thing logging to?
lsof -p ABC | grep log

8:40 Which processes still have this old library open?
lsof grep libname.so

9:45 Which files does user XYZ have open?
lsof -u XYZ
lsof -u XYZ -i # network only

10:25 Which process is listening on Port X (or using Protocol Y)?
lsof -i :80
lsof -i tcp

---
## Tee Command

