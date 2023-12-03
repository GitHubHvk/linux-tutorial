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
- ###### chmod 707 file1
  
  - change permission of file1 to rwx---rwx



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



***

# umask

this a value that when a file or directory is created, to calculate permission the value 666 for file and 777 for directory is subtracted from it and remaining will set as permission.

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



***

# diff

- ###### diff f1 f2
  
  - show different between files (( f1)) and (( f2 ))
- ###### diff -u f1 f2
  
  - show different between files (( f1)) and (( f2 )) with better format



***

# find

- ###### file f2
  
  - with print some information about file (( f2 ))
- ###### find / -name f2 -print
  
  - search for the file named (( f2 )) in directory (( / )) and all it's subdirectory, and print the full path of founded files.
- ###### find /var -name f2 -print
  
  - search for the file named (( f2 )) in directory (( /var )) and all it's subdirectory, and print the full path of founded files.



***

# head

- ###### head f2
  
  - print first 10 lines of the file (( f2 ))
- ###### head -3 f2
  
  - print last 3 lines of file (( f2 ))



***

# tail

- ###### tail f2
  
  - print last 10 lines in the file (( f2 ))
- ###### tail -2 f2
  
  - print last 2 lines of file (( f2 ))



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
- add command (( MTG="value 1" )) at the end of the file then save and close it.
- run command (( source /etc/profile)) to refresh it in current session.
- then log out from Linux and login again.



***

# shortcut keys to navigate in shell editor

- ###### ctrl-b

  - move cursor to the left

- ###### ctrl-f

  - move cursor to the right

- ###### ctrl-p

  - view the previous command

- ###### ctrl-n

  - view the next command

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



***

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
