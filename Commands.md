

# ls

- ###### ls -l

  - long list the content of current directory

- ###### ls /var/cttest/ -l

  - long list the content of (( /var/cttest/ )) directory

- ###### ls -a

  - print dot files (files that their name starts with dot)




****

# cp

- ###### cp file1 file2

  - copy file1 content to file2 (if file2 does not exists it create one, id file2 exists it replace content with content of file 1)

- ###### cp file1 dir2

  - copy file1 content to directory dir2

- ###### cp file1 file2 file3 dir2

  - copy files (( file1, file2, file3 )) to directory dir2



***

# mv

- ###### mv file1 file2

  - rename file (( file1 )) to (( file2 ))

- ###### mv file1 dir2

  - move file (( file1 )) to directory (( dir2 ))

***

# touch

- ###### touch file1

  - create file1 if not exists otherwise not change the content but edits timestamp of file

***

# rm

- ###### rm file1

  - remove file1

- ###### rm dir1 -r

  - delete directory (( dir1 )) with all contents inside it

***



# echo

- ###### echo my message

  - prints (( my message )) to the standard output.

- ###### echo *

  - print names of all files and directory of current working

 

***

# pwd

- ###### pwd

  - print current working directory



***

# cd

- ###### cd /dir1

  - navigate to directory dir1

- ###### cd

  - returns to home directory of current user

- ###### cd .. 

  - navigate to previous directory




***

# mkdir

- ###### mkdir dir1

  - create new directory named dir1



***

# rmdir

- ###### rmdir dir1

  - delete directory (( dir1 )) if it is empty



***

# grep

- ###### grep xyx /etc/passwd

  - print all lines in file (( passwd )) that contains the word (( xyz ))

- ###### grep first /var/greptest/*

  - print all lines in all files inside (( /var/greptest )) that contains the word (( first ))



***

# chmod

- ###### chmod 666 file1

  - change permission of file1 to rw-rw-rw-

- ###### chmod 777 file1

  - change permission of file1 to rwxrwxrwx

- ###### chmod 700 file1

  - change permission of file1 to rwx------

- ###### chmod 707 file1 (equal to `chmod u+rwx,g-rwx,o+rwx file1`)

  - change permission of file1 to rwx---rwx



![](D:\Tutorial\Linux\linux-tutorial\chmod-logic.png)



## working with characters

imagine file (( f2 )) with permission (( rwx------ ))

1. ###### chmod g+r f2

   1. change permission to (( rwxr----- ))

2. ###### chmod u-r

   1. change permission to (( -wxr----- ))

3. ###### chmod u-wx f2

   1. change permission to (( ---r----- ))

4. ###### chmod u+x,g+w,o+r f2

   1. change permission to (( --xrw-r-- ))



**HINT:** in order to be able to list the files inside the directories, it is needed for the user to have the `x` (execute) permission on the directory. in common cases the permission `xr` is needed for working with directories.



***

# umask

this is a constant value (say it is 0022) that is subtracted from the values 666 for file and 777 for directory and the remaining is used as default permission value to create a new file or directory respectively.

- ###### umask

  - this command show the current value of umask which by default is 0022 (first digit is sticky digit, three next digits are octal value of umask).

- ###### umask 0042

  - this command change the value of umask for the current session.



***

# sudoers

## add user to sudoers:

 - su -
 - nano /etc/sudoers
 - add bellow line to the file and save and close
   - hamed ALL=(ALL=ALL) ALL



**caution:** it is better to open and modify sudoers file with (( visudo )) command, it checks the syntax before saving the file. 



## Log

to see sudo logs, you can run following command (( case-sensitive )):

- ###### journalctl SYSLOG_IDENTIFIER=sudo



***

# less

- ###### less file1

  - print content of file with pagination

- ###### grep line /var/greptest/* | less

  - show all lines containing (( line )) in all files in path (( /var/greptest )) with pagination



**caution:** to close the file opened with ((less)), type ((q)). 



***

# diff

- ###### diff f1 f2

  - show different between files (( f1)) and (( f2 ))

- ###### diff -u f1 f2

  - show different between files (( f1)) and (( f2 )) with better format



***

# find

- ###### file f2

  - will print some information about file (( f2 ))

- ###### find / -name f2 -print

  - search for the file named (( f2 )) in directory (( / )) and all it's subdirectory, and print the full path of founded files.

- ###### find /var -name f2 -print

  - search for the file named (( f2 )) in directory (( /var )) and all it's subdirectory, and print the full path of founded files.



***

# head

- ###### head f2

  - print first 10 lines of the file (( f2 ))

- ###### head -3 f2

  - print first 3 lines of file (( f2 ))



***

# tail

- ###### tail f2

  - print last 10 lines in the file (( f2 ))

- ###### tail -2 f2

  - print last 2 lines of file (( f2 ))



**HINT:** to follow log use the option `-f`.

**Caution:** `tail` is more used than `head`. because we usually want to watch for the last logs.



***

# sed

with this command we can find a pattern and replace it with another pattern. for example if we have a file `text.txt` and want to replace word `Node.js` with `React` in this file we would execute the following command:

```
sed -i 's/Node.js/React/g' test.txt
```



***

# sort

- ###### sort f2

  - print lines in file (( f2 )) in alphabetical order

- ###### sort -n f2

  - print lines in file (( f2 )) in alphabetical and numerical  order if they start with number

- ###### sort -r f2

  - print lines in file (( f2 )) in alphabetical reverse order if they start with number

- ###### sort -n -r f2

  - print lines in file (( f2 )) in alphabetical and numerical reverse order if they start with number



***

# passwd

use this command to change your password. it will ask you your old pass, and will get new password from you



***

# shell variables

- ###### V1=Hello 

  - value (( Hello )) is set to variable (( V1 ))

- ###### echo $A

  - print the value of variable (( V1 ))



***

# environment variables

- ###### env

  - print list of environment varables

- ###### echo $HOME

  - print home directory of the current user

- ###### printenv $HOME

  - print home directory of the current user

- ###### export HOME=/home/hamed

  - this command set the environment variable (( HOME )) equal to path (( /home/hamed )) for the current user in current session



***

# bashrc dot file

this is a bash file that is executed when you log in.

we can set our environment variables here to be available in all sessions for the user. 

for example to set JAVA_HOME to (( /usr/bin/java)), follow these steps:

- vi ~/.bashrc
- edit this file and add command (( export JAVA_HOME=/usr/bin/java )) at the end, and save it.



***

# environment file

this is a file that is loaded when log in.

so to set an environment variable named (( MTG )) globally accessible for all users, do the following steps:

- su -
- vi /etc/environment
- add command (( MTG="value 1" )) at the end of the file then save and close it.
- run command (( source /etc/environment )) to refresh it in current session.
- then log out from Linux and login again.



***

# profile file

this is a file that is loaded when log in.

so to set an environment variable named (( MTG )) globally accessible for all users, do the following steps:

- su -
- vi /etc/profile
- add command (( export MTG="value 1" )) at the end of the file then save and close it.
- run command (( source /etc/profile)) to refresh it in current session.
- then log out from Linux and login again.



# profile.d directory

any file in this directory will be loaded in user login. so to have an environment variable we can also create file in this directory and declare our variable here.

following steps create a file named ((gv.sh)) in ((profile.d)) directory and set a variable named ((G1)) in that file. from now on, the variable ((G1)) will be available globally for the user after login.

1. cd /etc/profile.d
2. touch gv.sh
3. vim gv.sh
4. press ((i))
5. write ((G1="my global 1 variable"))
6. press ((Esc))
7. press (( Shift + : ))
8. writ command ((wq))



***

# shortcut keys to navigate in shell editor

- ###### ctrl-b

  - move cursor to the left
  - ((b)) is abbreviated ((backward))

- ###### ctrl-f

  - move cursor to the right
  - ((f)) is abbreviated ((forward))

- ###### ctrl-p

  - view the previous command
  - ((p)) is abbreviated ((previous))

- ###### ctrl-n

  - view the next command
  - ((n)) is abbreviated ((next))

- ###### ctrl-a

  - move cursor to the beginning of line

- ###### ctrl-e

  - move the cursor to the end of line

- ###### ctrl-w

  - erase preceding word

- ###### ctrl-u

  - erase from cursor to the beginning of the line

- ###### ctrl-k

  - erase from cursor to the end of line

- ###### ctrl-y

  - paste erased text



***

# man

- ###### man ls

  - will show manual of ls command

- ###### man rm

  - will show manual of rm command

- ###### man -k sort

  - search and list all commands that have (( sort )) word in their name or description.



***

# shell input output

- ###### man ls > /var/greptest/f2

  - this command will send output of command (( man ls )) to the file (( f2 ))

- ###### man ls >> /var/greptest/f2

  - this command will append output of command (( man ls )) to the file (( f2 ))

- ###### head f2 | tr a-z A-Z

  - this command pass the output of command (( head f2 )) which is first 10 lines of file (( f2 )) to the input strinm of command (( tr a-z A-Z )) which capitalaze all charcters in it



***

# ps

- ###### ps

  - list all running process (program) of current session

- ###### ps x

  - list all running process of current user

- ###### ps ax

  - list all running process of all users

- ###### ps u

  - list all running process with more info

- ps w

  - list all running process of current session with full command name



**HINT:** it is possible to mix each of above command together, for example `ps aux`, `ps xu`.

**HINT:** to get the process info about a specific application, for example `nginx`, you can run the command `ps aux | grep nginx`; this will list all processes related to `nginx`.    

**HINT:** `$$` is a shell variable that stores the current Shell PID. for example we can call `echo $$` to see the current running shell PID. 



***

# pidof

this command will return all process id of an application. for example to list all process related to `nginx`, execute the following command: 

- pidof nginx



***

# top

this command will show you real-time info about processes, including following:

- %CPU
  - CPU usage, 

- %MEM
  - Memory usage
- VIRT
  - virtual memory used
- USER
  - owner of the process
- PR
  - process priority
- COMMAND
  - the name of the command that started the process,
- SHR
  - shared memory used
- NI
  - nice value of the task, lower nice value has higher priority.  



**HINT:** when you execute the command `top`, the output keep refreshing until you press `q`, but you can specify a limit for it when executing the command, for example to exit after 10 time repetition, execute command `top -n 10`



**HINT:** to show process of a specific user for example `hamed`, execute  command `top -u hamed`.



### keys in running `top`:

when the command has been executed and you are monitoring the output, you can press any of the following keys for described purpose:

-  z
   - highlight running processes.
-  c
   - shows absolute pass of the process.
-  shift+p
   - sort the processes by PID.
-  q
   -  to quit the output




***

# htop

this is like `top` command but is more graphical. first you should install it by executing command `apt install htop`.



**HINT:** to sort result of this command you can click on the desired title in the output table, and sort it as descending or ascending.  this option is also available in output of `top` command.



# kill

- ###### kill 1245

  - terminate the process with id equal to 1245

- ###### kill -stop 1245

  - freeze process with id equal to 1245

- ###### kill -cont 1245

  - continue the stopped process with id equal to 1245

- ###### kill -9

  - this command send the signal with number instead on option.

- ###### kill -l

  - list all numbers related to different signals.



***

# `iotop`

this is a command to list the disk I/O read and write. it could be usable to find the process that cause a lot of I/O.



it has some options as follow:

- -o
  - to show process that are actually doing IO (`iotop -o`)
- --version
  - to show the installed version of the app (`iotop --version`).
- -b 
  - to display in noninteractive mode (`iotop -b`). 
- -n
  - to change the number of iteration and then exists (`iotop -n 3`).
- -p
  - to show IO of a specific process by ID (`iotop -p 10809`).
- -t 
  - to add timestamp to the output.



***

# ‌Background process

you run a program that is time consuming one, to the background (detach it from shell) by writing (( & )) at the end of command.



## Example 1:

gunzip f2.gz & 



***

# Groups

this command shows the name of primary or any supplementary groups for each given user if specified, and for current session user if not specified.

- ###### groups

  - prints group of current session user 

- ###### groups 'hamed'

  - print group of user (( hamed ))



***

# ln

creates a symbolic link.

- ###### ln -s /var/test/f1.txt f1link

  - this command creates a symbolic link for file (( f1.txt )) in path (( /var/test/f1.txt )) with name (( f1link )) that is located in the current working directory.

- ###### ln -s /var/test dlink

  - this command creates a symbolic link for the path (( /var/test )) with name (( dlink )) that is located in the current working directory.



***

# gzip

this command compress just one file with extension (( .gz )) and remove the original file.

- ###### gzip f1.txt

  - this command create file (( f1.txt.gz )) and remove (( f1.txt ))



***

# gunzip

this command unzip the file (( .gz )) and remove it.

- ###### gunzip f1.txt.gz

  - this command unzip file (( f1.txt.gz )) to (( f1.txt )) and remove (( f1.txt.gz )).



***

# tar

this command compress multiple files and folders and also does not delete the source files and folders, and the result would be a file with (( .tar )) prefix.

imaging following files and directories as sample:

1. f1.txt
2. f2.txt
3. dir1
   1. f3.txt
   2. f4.txt
4. f5.txt



- ###### tar cvf tt.tar f1.txt f2.txt dir1 f5.txt

  - this command will archive all above files and folders in a file named (( tt.tar )) without removing them.

- ###### tar xvf tt.tar

  - this command will extract all files and folders to the current working directory and does nor remove (( tt.tar )) file.

- ###### tar tf tt.tar

  - this command print the name of content if the (( .tar )) file.

- ###### tar xvf tt.tar f1.txt dir1/t3.txt

  - this command will extract files (( f1.txt )) and (( dir1/f3.txt )).





**HINT:** to preserve exact permission sets of the extracted files inside tar fire when unpacking it to your local machine, it is recommended to use `p`  option.



***

# compressed archived files

files with extension (( .tar.gz )) are kind of files that are first archived with tar command and then compressed with gzip command. at least there are 2 way to unpack them:

- ###### zcat -dc tt.tar.gz | tar xvf -

- ###### tar zxvf tt.tar.gz

to show the files name and directories inside these kind of files, use following command:

- ###### tar ztvf tt.tar.gz



**caution:** a file with extension (( .tgz )) is equal to file with extension (( .tar.gz ))



****

# xz and bzip2

xz and bzip2 are 2 other programs to compress files, and produce files with extension (( .xz )) and (( .bzip2 )) respectively. to decompress their files we could use (( unxz )) and (( bunzip2 )). they can be used exactly like gzip and gunzip commands.



**caution:** there are some files with extension (( .Z )) that could be decompressed by (( gunzip )).



***

# free -m

this command shows about resources in OS.



***

# lsb_release -a

this command show essential information about current installed OS.



***

# get OS architecture 

dpkg --print-architecture



***

# Get OS version in Linux

cat /etc/os-release



# Get kernel version

uname -r



***

# hostname -i

show the IP address of the host machine



***

# df -h

lists info about OS hard disks



# df -i

lists the `inode` info of the OS.

to make it more readable, use option `-h`, as `df -ih`.



***

# du

to see the real size of a path or file. for example if you have a path `/var/test`, to sea the size of it, you need to execute the following command:

- du -sh /var/test
  - s: to sum all sub directories and files.
  - h: to show this as human-readable




**HINT:** there is an app named `ncdu` that is needed to be installed by command `apt install ncdu`, which is more graphical, and make it possible to interact with it by `arrow` and `enter` keys. 



***

# tune2fs -m 0 /dev/sda1

by running command `rm` the used disks may not became free, so to do it manually we can execute this command. 



# hostnamectl

this is a service to manage host name. for example to change the host-name we can execute the following command:

```powershell
hostnamectl set-hostname new_host_name
```



**HINT:** after changing host name, it is recommended to change the config in file `/etc/hosts` as `127.0.1.1 new_host_name`. 



# file lines count

by executing following command you will find out how many lines of text your file has.

```powershell
wc -l myfile
```

 
