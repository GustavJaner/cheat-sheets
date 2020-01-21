_Author: Gustav Janér_

**TODO**

# Unix Shell commands

## GREP
Combine grep with regex to make useful searches
```
ls | grep ....

grep -ri "lolcodes" .    # recoursively, ignore captitalization, search for string " " in current directory .
```

## CHMOD

$ chmod 644 <file/folder>
The three numbers places for 644, listed above, represent the following:

  6     4     4
Owner Group Other

The “6” in “Owner” means the owner can read/write. The “4” and second “4” restricts “Group” and “Other” to read only.

Eg. To give full Read/Write/Execute access to all users Owner/Group/Other:
$ cmhod 777 <file>


### Symbolic Permission
0	---	none
1	--x	execute
2	-w-	write
3	-wx	write/execute
4	r--	read
5	r-x	read/execute
6	rw-	read/write
7	rwx	read/write/execute


## LS
```
$ ls    # List current directory
```
### Flags
-a       # Also list hidden files\
-l       # List with long format\
-t       # List by time last modified\


## ECHO

$ echo "Hello, world!" >> README.md

Append a new line to the file, or
Create the file README.md if it does not exist


## RM

$ rm -rf <file> - Recusively force removes a folder and its contents


## KILL
todo

## ZIP
.bz2 - CROSS PLATFORM, good compression format

$ tar -jcvf archive_name.tar.bz2 folder_to_compress
$ tar -jxvf archive_name.tar.bz2

## SIPS
$ sips -Z 640 \*.jpg

-Z is to maintain the image's aspect ratio
"640" is the maximum height and width to be used
downsize every image ending in .jpg.


## SSH

Transfer all local files in current local directory to the remote host to directory: code/test
$ scp * pi@192.168.31.209:~/code

Copy the file "foobar.txt" from a remote host to the local host
$ scp pi@192.168.31.209:alarmOff ~/some/local/directory



# Shell Scripts
## sh vs. bash
### sh
- standardized
- much simpler and easier to learn
- portable across POSIX systems — even if they happen not to have bash, they are required to have sh

### bash
- more features, make programming more convenient and similar to programming in other modern programming languages.
- scoped local variables, arrays etc.

## sh script
$ vim test_script.sh

Insert following at the first line of the file:
	#!/bin/sh         tells the terminal that you are using bash shell
	echo hello world    prints out “hello world” in the terminal

$ chmod 700 test_script   to allow Owner permission to Read/Write/EXECUTE the file

$ sh test_script.sh    execute the script
$ ./test_script.sh
Hello world


## bash script
$ vim test_script  (no extension)

Insert following at the first line of the file:
	#!/bin/bash         tells the terminal that you are using bash shell
	echo hello world    prints out “hello world” in the terminal

$ chmod 700 test_script   // to allow Owner permission to Read/Write/EXECUTE the file
$ ./test_script           execute the script
Hello world


# Misc

## .bash_profile
.bash_profile is a hidden file in the home directory.
This file is loaded before Terminal loads your shell environment and
contains all the startup configuration and preferences for your command line interface.
Within it you can change your terminal prompt, change the colors of text,
add aliases to functions you use all the time, and so much more.
This file is often called a ‘dot file’ because the ‘.’ at the beginning
of it’s name makes it invisible in the Mac Finder. You can view all
invisible files in the Terminal by typing ls -a in any directory.

$ vim .bash_profile
