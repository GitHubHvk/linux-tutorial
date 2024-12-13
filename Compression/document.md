# gzip

this command compress just one file with extension (( .gz )) and remove the original file.

- ###### gzip f1.txt

  - this command create file (( f1.txt.gz )) and remove (( f1.txt ))



***

# gunzip

this command unzip the file (( .gz )) and remove it.

- ###### gunzip f1.txt.gz

  - this command unzip file (( f1.txt.gz )) to (( f1.txt )) and remove (( f1.txt.gz )).



---

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