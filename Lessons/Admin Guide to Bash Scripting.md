# Admin Guide to Bash Scripting

## Chapter 2 - Basics

#### Redirecting I/O, Utility Commands and Pipes

Redirecting stdout, stdin and stderr

**Legend**

`0` - stdout
`1` - stdin
`2` - stderr


Redirecting stdin (1): 
```
1> filename.txt
```
Redirecting stderr (2): 
```
2> filename.txt
```

Redirecting both stdin (1) and stderr (2):
```bash
2>&1 filename.txt
```
#### Utility Commands
- `sort`: sorts input and prints a sorted output
- `uniq`: removes duplicate lines of data from the input stream
- `grep`: searches incoming lines for matching text
- `fmt`: receives incoming text and outputs formatted text
- `tr`: translates characters
- `head/tail`: outputs the first/last few lines of a file
- `sed`: stream editor: more powerful than tr as a char translator
- `awk`: an entire programming lang designed for constructing filters. Very powerful and complex.

#### Demoggification (or UUOC: Userless Use of Cat)



## Chapter 3 - Variables

#### Bash Functions

```bash

tail () {
    command tail -n $1
    
    # to declare local var that can only be accessible within the function
    local VAR=30
}

```

#### Working with Arrays in Bash

To initialize an Array
```bash
NUMBERS=(1 2 3)
echo $NUMBERS

# get the last item from the array
echo ${NUMBERS[2]}

# Get all array items
echo ${NUMBERS[@]}

# Get array length
echo ${#NUMBERS[@]}

# Get the index applicable to the array
echo ${!NUMBERS[@]}

# Add new item in the array
NUMBERS+=(9)

# Get middle value
echo ${NUMBERS[@]:START_INDEX:END_INDEX}
```

## Chapter 4 - Substitutions

#### Command substitutions


#### Process substitutions

```bash
bash <(command to run)

diff <(ls /tmp) <(ls /bin)
```

## Chapter 5 - Flow Control

#### Using the for Loop


```bash
for i in $(ls); \
do echo "item:" $i; \
done
```

#### Using the while or until loop

```bash
#!/bin/bash

COUNTER=0

# While
while [$COUNTER -lt 10 ]; do
  echo the counter is $COUNTER
  let COUNTER=COUNTER+1
done
```

```bash
#!/bin/bash

COUNTER=20

# Until
until [ $COUNTER -lt 10 ]; do
  touch file$COUNTER
  let COUNTER-=1
done
```

#### Handling Signals and Traps

What are Signals?

Programs in Linux are managed partially by `signals` from the kernel

- SIGKILL
- SIGINT
- SIGTERM
- SIGUSR1

> Note: SIGINT Doesn't always behave this way (In general though, this behavior is accurate)

> Note: You cannot trap SIGKILL (Much like pulling the plug from a server is a last resort, SIGKILL is special in that it cannot be trapped. If you must resort to SIGKILL, something else has gone terribly wrong).
> 
```bash
trap function SIGINIT
```

#### Using Exit Status, Tests and Builtins

```bash
if [ -d $BACKUP TARGET]
```
> `-d` means if the director exists
> 



## Chapter 7 - Debugging Bash Scripts

#### Part 1

`Set`: command changes how the shell acts
> for example, set -f changes how the shell parses a glob (or wildcard). set -x prints all the actions the shell takes to run the command. set -v prints back lines as they're read.

``` 
# sample use case
#! /bin/bash -x,-u or different syntax
or
`set -x` before the line you want to test
or
sudo bash `-x` ./script.sh param-1 param-2
```

```
# Usefull syntax
set -n or +n
set -u or +u
set -x or +x
set -f or +f
```

#### Part 2
#### 3 Additional troubleshooting builtins

1. `break`: exits a loop without the control condition being met
2. `continue`: skips processing the rest of this loop and moves to the next iteration
3. `readonly`: sets a variable to readonly - cannot be changed

## Chapter 8 - Quick and Dirty Regex in Bash

`Regex`: Regular expressions is a sequence of characters that define a search pattern

Most common grep

- '|' - simply like 'or'
- '^' - starts with ...
- \<WORD\> - scaping or containing only inside

```bash
grep -E 'regex here' filename
```

```bash
#!/bin/bash

if [ -z "$1" ]; then
	echo "You have failed to pass a parameter. Please try again."
	exit 255;
fi


MYLOG=$1

function ctrlc {
	rm -rf /home/$USER/work_backup
	rm -f /home/$USER/$MYLOG
	echo "Received Ctrl+C"
	exit 255
}

trap ctrlc SIGINT

echo "Timestamp before work is done $(date +"%D %T")" >> /home/$USER/$MYLOG
echo "Creating backup directory" >> /home/$USER/$MYLOG
mkdir /home/$USER/work_backup

echo "Copying Files" >> /home/$USER/$MYLOG
for i in $(ls work | grep -E 'adent|financ')
do
        cp -v /home/$USER/work/$i /home/$USER/work_backup >> /home/$USER/$MYLOG
done
#cp -v /home/$USER/work/$FILES /home/$USER/work_backup/ >> /home/$USER/$MYLOG
echo "Finished Copying Files" >> /home/$USER/$MYLOG
echo "Timestamp after work is done $(date +"%D %T")" >> /home/$USER/$MYLOG
```

## Chapter 9 - Best Practices

#### Reviewing Objectively Bad Scripts

