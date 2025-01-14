**Linux command line Basics**
-----------------------------------

The command line in Linux is referred to as a shell. The shell is a
program that allows the user to interact with Linux at the command line.
Most work to be done on the cluster will be done using the command line
(shell). Once you logged in to the cluster using SSH, you will be in
your home directory on a shell prompt (username@server:~$\_). The
following are some of the commands for system operations via the shell

**Man pages**
=======================

Man pages refers to manual pages on Linux. They provide the help guide
for most commands and applications on linux. Most Linux files and
commands have pretty good man pages to explain their use. Type man
followed by a command (for which you want help) and start reading. Press
q to quit the man page. sample below:

username@allot:~$ man whoami (shows manual page for the command whoami)

username@allot:~$ man syslog.config (shows the manual page for a
configuration file)

username@allot:~$ man syslogd (show the manual for a daemon (background
program))

**Working with directories**
=======================

The Linux filesystem is based around files and directories. Users issue
commands to navigate around the filesystem. The common file system
commands to work with include **pwd, cd, ls, mkdir, cp, touch etc** .
These commands are available on any Linux system.

This section will also discuss absolute and relative paths and path
completion in the bash shell.

.. _section-1:

**pwd**
=======================

The pwd (Print Working Directory) command basically displays the current
directory you are in. This would appear as:

username@allot:~$ pwd

/home/username

.. _section-2:

**cd**
=======================

On the command line cd (or change directory) changes your current
directory to the one specified:

username@allot:~$ cd /var

username@allot:~$ pwd

/var

username@allot:~$ cd /home/user

username@allot:~$ pwd

/home/user

There is also a shortcut back to your home directory by typing the
character ~ (tilda) which has the same effect as typing (for example)
/home/user.

username@allot:~$ cd ~

username@allot:~$ pwd

/home/user

The filesystem is usually organized as hierarchies, To go to the
directory above (higher hierarchy or parent directory), we use the
characters ..

username@allot:~$ pwd

/home/user

username@allot:~$ cd ..

username@allot:~$ pwd

/home

There is also the character. which means the current directory. This is
not useful for the cd command but is very useful for copying files to
your current directory.

**Absolute and relative paths**
=======================

Important to note that When you type a path starting with a slash (/),
then the root of the file tree is assumed. If you don't start your path
with a slash, then the current directory is the assumed starting point.

/directory1/directory2 is not the same as directory1/directory2

**/** on linux systems is known as ROOT Directory, Which signifies the
beginning of the file-system hierarchy

Absolute path:

username@allot:~$ pwd

/home/user

user@allot:~$ cd /var

user@allot:~$ pwd

/var

Relative path

username@allot:~$ pwd

/home/user

username@allot:~$ cd myfiles

username@allot:~$ pwd

/home/user/myfiles

The command line will help you in typing a path without errors. If you
type a partial command such as:

username@allot:~$ cd /home/user/my

Pressing the **TAB** key on your keyboard will fill in the rest of the
directory name (or filename/program) depending on the uniqueness, or
presence.

username@allot:~$ cd /home/user/myfiles

.. _section-3:

**ls**
=======================

This command lists the contents of a directory:

username@allot:~$ ls

myfile1.txt myfile2.txt workdirectory1

**ls** has a number of useful different options to format the output
listing.

username@allot:~$ ls –l (show a long listing with more information)

username@allot:~$ ls –a (show all files including those that are hidden)

username@allot:~$ ls –la (combines both of the options above)

.. _section-4:

**mkdir**
=======================

This commands creates a directory (folder) in the current (or specified)
directory:

username@allot:~$ mkdir workdirectory1

username@allot:~$ cd workdirectory1

username@allot:~$ pwd

/home/user/workdirectory1

.. _section-5:

**rmdir**
=======================

This command removes the specified directory, note the directory must be
empty and must not be the directory you are currently in:

username@allot:~$ rmdir workdirectory1

.. _section-6:

.. _section-7:

**Working with files**
=======================

Working on the linux shell would require you to carry out file
operations using commands to create, remove, copy, move and rename
files.

When working with files on linux you have to know that MYFILE1 is not
the same as MYfile1 or myfile1 due to case sentivity. Also Linux treats
everything on Linux as a file. A directory is a special kind of file,
but it is still a (case sensitive!) file. Each terminal window (for
example /dev/pts/4), any hard disk or partition (for example /dev/sdb1),
and any process are all represented somewhere in the file system as a
file. It is important to note that not all files or directories are
accessible to every user. File and directory access is dependent on the
permissions on the latter. As with most commands in linux, options can
be specified while executing any given command e.g. ls -l

.. _section-8:

**file**
=======================

This command determines the file type. Unlike Windows, Linux does not
determine the file type from the extension but from examining the file
header/contents itself.

username@allot:~$ file mypicture.png

pic33.png: PNG image data, 3840 x 1200, 8-bit/color RGBA, non-interlaced

username@allot:~$ file parallel.c

parallel.c: ASCII C program text

.. _section-9:

**touch**
=======================

This creates an empty file, which can be useful for various uses.

username@allot:~$ touch newfile.c

username@allot:~$ ls

newfile.c

**rm**
=======================

Remove a file, as always be very careful with this command and without a
backup, this file will be lost forever.

username@allot:~$ ls

newfile.c oldfile.c

username@allot:~$ rm oldfile.c

username@allot:~$ ls

newfile.c

username@allot:~$ rm –i oldfile.c (this will prompt to answer yest/no
before deletion occurs)

username@allot:~$ rm –r mydirectory (works recursively down the
specified directory to remove specified directory but not removing any
non-empty directories)

username@allot:~$ rm –rf mydirectory (works recursively down the
specified directory to remove non-empty directories with the –f option
which means force. This is a very powerful option which must be used
with extreme care!)

As with many Linux commands there are a few options with can be used
with **rm** (these can be view by typing **man rm**).

.. _section-10:

**cp**
=======================

Copy files or directories from a source to a destination:

Command syntax is ‘cp SOURCE DESTINATION’

username@allot:~$ cp workfile.c mybackup.c (copies workfile.c to
mybackup.c)

username@allot:~$ cp workfile.c mydirectory1 (copies workfile.c to
mydirectory1)

username@allot:~$ cp \*c backupdirectory/ (copies all \*.c files to
backupdirectory)

username@allot:~$ cp –r mydirectory1 mydirectory2 (copies one directory
to another, note the option –r for recursive copying)

As with many Linux commands there are a few options with can be used
with **cp** (these can be view by typing **man cp**).

.. _section-11:

**mv**
=======================

Move files from a source to a destination. A versatile command that can
rename a file too:

username@allot:~$ mv file1.c testfile.c (rename file1.c to testfile.c)

username@allot:~$ mv directory1 directory2 (rename directory)

username@allot:~$ mv file1.c /home/user/myrepo (mv file1.c to
/home/user/myrepo/file1.c)

**rename**
=======================

Although preferably to use the **mv** command, this command does exist
to rename files

username@allot:~$ touch file1.c file2.c file3.c

username@allot:~$ ls

file1.c file2.c file3.c

username@allot:~$ rename .c .backup \*.c

username@allot:~$ ls

file1.backup file2.backup file3.backup

.. _section-12:

**Working with file contents**
=======================

This section will look at working with file contents themselves, such
commands
are **head**, **tail**, **cat**, **tac**, **more** and **less**.

.. _section-13:

**head**
=======================

By default, the head command will show the first ten lines of a file.

username@allot:~$ head /etc/passwd

root:x:0:0:root:/root:/bin/bash

daemon:x:1:1:daemon:/usr/sbin:/bin/sh

bin:x:2:2:bin:/bin:/bin/sh

sys:x:3:3:sys:/dev:/bin/sh

sync:x:4:65534:sync:/bin:/bin/sync

games:x:5:60:games:/usr/games:/bin/sh

man:x:6:12:man:/var/cache/man:/bin/sh

lp:x:7:7:lp:/var/spool/lpd:/bin/sh

mail:x:8:8:mail:/var/mail:/bin/sh

news:x:9:9:news:/var/spool/news:/bin/sh

.. _section-14:

**tail**
=======================

Similar to **head** but this time it will show the last 10 lines of the
file by default.

username@allot:~$ tail /etc/services

vboxd 20012/udp

binkp 24554/tcp # binkp fidonet protocol

asp 27374/tcp # Address Search Protocol

asp 27374/udp

csync2 30865/tcp # cluster synchronization tool

dircproxy 57000/tcp # Detachable IRC Proxy

tfido 60177/tcp # fidonet EMSI over telnet

fido 60179/tcp # fidonet EMSI over TCP

.. _section-15:

**cat**
=======================

The **cat** command (short for concatenate) one of the most universal
tools, yet all it does is copy standard input to standard output. In
combination with the shell, this can be very powerful and diverse. Some
examples will give a glimpse into the possibilities.

username@allot:~$ cat /etc/resolv.conf

domain example.com

search example.com

nameserver 192.168.1.42

username@allot:~$ cat file1.c file2.c >file3.all (concatenate c files
into file3.all)

.. _section-16:

**tac**
=======================

Works the same as **cat** but will show you the file backwards:

username@allot:~$ cat numbers

one

two

three

username@allot:~$ tac numbers

three

two

one

.. _section-17:

**more**
=======================

The more command is useful for displaying files that take up more than
one screen. More will allow you to see the contents of the file page by
page. Use the space bar to see the next page, or q to quit. Some people
prefer the less command to more.

.. _section-18:

**less**
=======================

Very similar to more but with some additional features

.. _section-19:

**Basic Linux Tools**
=======================

This chapter introduces commands to find or locate files and to compress
files, together with other common tools that were not discussed before.

**find**
=======================

This command is very useful to find files, more options are provided on
the command line by typing **man find**. Here are some useful examples
below:

username@allot:~$ find /etc (find all files in the /etc directory)

username@allot:~$ find . –name “\*.conf” (find all files that end in
.conf from the current directory)

username@allot:~$ find . –newer file1.c (find all files newer than
file1.c)

username@allot:~$ find /etc >etcfiles.txt (find all files but this time
put them in (pipe) to the file etcfiles.txt)

.. _section-20:

.. _section-21:

**date**
=======================

The **date** command can display the date, time, time zone, and more.

username@allot:~$ date

Tue Jan 14 12:18:58 PM WAT 2025

.. _section-22:

**cal**
=======================

The **cal** command displays the current month, with the current day
highlighted.

username@allot:~$ cal

April 2022

Su Mo Tu We Th Fr Sa

1 2

3 4 5 6 7 8 9

10 11 12 13 14 15 16

17 18 19 20 21 22 23

24 25 26 27 28 29 30

**sleep**
=======================

The **sleep** command is sometimes used in scripts to wait a number of
seconds. This example shows a five-second sleep.

username@allot:~$ sleep 5 (five seconds later)

username@allot:~$

.. _section-23:

**sort**
=======================

The command **sort** will sort lines of text files. By default, the
output is to the screen but this can be piped to a file or another
program.

username@allot:~$ sort myfile.txt

apple

banana

cherry

.. _section-24:

**gzip – gunzip**
=======================

This is a useful compression program (like **zip** which also exists in
Linux). The **gzip** command can make files take up less space.

username@allot:~$ gzip myfile.c (will create myfile.c.gz)

username@allot:~$ gunzip myfile.c.gz (will create myfile.c again)

.. _section-25:

**bzip2 – bunzip2**
=======================

Files can also be compressed with **bzip2** which takes a little more
time than **gzip**, but compresses better.

username@allot:~$ bzip2 myfile.c (will create myfile.c.bz2)

username@allot:~$ bunzip2 myfile.c.bz2 (will create myfile.c again)

.. _section-26:

**zip – unzip**
=======================

A compression program which is compatible with other **zip** programs
found in MS Windows and other OSes.

username@allot:~$ zip myfile.c (will create myfile.c.zip)

username@allot:~$ unzip myfile.c.zip (will create myfile.c again)

.. _section-27:

**grep**
=======================

The **grep** filter is famous among Linux (and UNIX) users. The most
common use of **grep** is to filter lines of text containing (or not
containing) a certain string.

username@allot:~$ grep “word” /folder/file

As with most Linux commands, there are also a large number of useful
options that will go with each command and **grep** is certainly no
exception here

username@allot:~$ grep –i “Word” /folder/file (search in a case
insensitive way)

username@allot:~$ grep –r “Word” /folder/folder (search recursively down
any directories too)

username@allot:~$ grep –v “Word” /folder/file (search for everything not
containing “Word”)

.. _section-28:

**wc**
=======================

Counting words, lines, and characters are easy with **wc**.

username@allot:~$ wc myfile.c (show number of words, lines, and
characters)

5 10 100 tennis.txt

.. _section-29:

**File Permissions**
=======================

**Introduction**
=======================

Similar to many other operating systems Linux uses a method of access
rights on files and directories. These can be view by using the ls
command

username@allot:~$ ls –l (option l is for long listing)

-rwx--x--x 1 hpcuser001 hpcuser001 68 Jan 14 12:15 newfile.sh

-rwxr-xr-x 1 hpcuser001 hpcuser001 265 Jan 13 11:45
script_slurm_hostname.sh

-rwx------ 1 hpcuser001 hpcuser001 235 Jan 13 12:49
script_slurm_jupyterlab.sh

-rw--r--r-- 1 hpcuser001 hpcuser001 147 Jan 13 11:50 slurm-778.out

-rwx------ 1 hpcuser001 hpcuser001 3415 Jan 13 11:54 slurm-779.out

-rwx------ 1 hpcuser001 hpcuser001 2580 Jan 13 12:51 slurm-787.out

drwx------ 2 hpcuser001 hpcuser001 2 Jan 14 12:12 workdirectory1

Each file and directory has access rights that are associated with each
one. When we look at the 10 symbol string above on the left-hand side
(e.g. **drwxr-xr-x**).

- The first letter present whether the file is a directory or not.

- The next three represent the file permission for the user that owns
  that file (i.e. dbird in this example).

- The next three represent the file permission of the group to whom that
  user belongs (i.e. group admin).

- The last three represent the file permissions for everyone else (i.e.
  all users).

For each of the permission parts the letters mean the following in their
groups:

- r indicates read permission to read and copy the file, its absence
  indicates this is not available.

- w indicates write permission to write the file, its absence indicates
  this is not available.

- x indicates execution permission to allow the file to be executed, its
  absence indicates this is not available.

Using the example above would mean:

- Example **(1)** has read/write access for user dbird and read access
  only for everyone else.

- Example **(2)** is a directory with full access for user dbird and
  read access for only users in the admin group.

- Example **(3)** is an application which is only accessible by the user
  dbird, note not only is it read and write but it also has its
  ‘execution’ permission set for that user also.

**Changing access rights**
=======================

This command allows the user to change file (and directory) permissions.

- u User

- g Group

- o Other

- a All

- r Read

- w Write (and erase)

- x Execution (and access directory

- + Add permission

- - Remove permission

username@allot:~$ chmod go-rwx myfile.c (remove read, write and execute
permissions removed for group and other)

username@allot:~$ chmod u+x myapp.pl (make the program myapp.pl
executable to the user (i.e. the owner of the file))

**Text editing**

In order to edit files on linux terminal, programs such as vim, emacs,
nano can be used.

username@allot:~$ nano myfile1.c (opens a new file in the nano text
editor)

**Environment variables**

The following variables are automatically available after you log in:

$USER your account name

$HOME your home directory

$PWD your current directory

You can use these variables on the command line or in shell scripts by
typing $USER, $HOME, etc. For instance: ‘echo $USER’. A complete listing
of the defined variables and their meanings can be obtained by typing
‘printenv ‘.

You can define (and redefine) your own variables by typing:

export VARIABLE=VALUE

**Aliases**

If you frequently use a command that is long and has for example many
options to it, you can put an alias (abbreviation) for it in
your ~/.bashrc file. For example, if you normally prefer a long listing
of the contents of a directory with the command ‘ls -laF \| more’, you
can put following line in your ~/.bashrc file:

alias ll='ls -laF \| more'

You must run ‘source ~/.bashrc’ to update your environment and to make
the alias effective, or log out and in :-). From then on, the command
‘ll’ is equivalent to ‘ls -laF \| more’. Make sure that the chosen
abbreviation is not already an existing command, otherwise you may get
unexpected (and unwanted) behavior. You can check the existence and
location of a program, script, or alias by typing:

which [command]

whereis [command]

**~/bin**

If you frequently use a self-made or self-installed program or script
that you use in many different directories, you can create a directory
~/bin in which you put this program/script. If that directory does not
already exist, you can do the following. Suppose your favorite little
program is called ‘myscript’ and is in your home ($HOME) directory:

mkdir -p $HOME/bin

cp myscript $HOME/bin

export PATH=$PATH:$HOME/bin

PATH is a colon-separated list of directories that are searched in the
order in which they are specified whenever you type a command. The first
occurrence of a file (executable) in a directory in this PATH variable
that has the same name as the command will be executed (if possible). In
the example above, the ‘export’ command adds the ~/bin directory to the
PATH variable and any executable program/script you put in the ~/bin
directory will be recognized as a command. To add the ~/bin directory
permanently to your PATH variable, add the above ‘export’ command to
your ~/.bashrc file and update your environment with ‘source ~/.bashrc’.

Make sure that the names of the programs/scripts are not already
existing commands, otherwise you may get unexpected (and unwanted)
behaviour. You can check the contents of the PATH variable by typing:

printenv PATH

echo $PATH
