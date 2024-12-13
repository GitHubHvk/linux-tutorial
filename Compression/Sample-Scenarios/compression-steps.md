# Subject

we want to practice a best way to compression and decompression process in Linux:

steps:

- create a directory, containing some files and directories inside it.
- compress and decompress it with `gzip`.
- compress and decompress it with `zcat`.



# Create Directory

create following structure:

```powershell
dir1
 - file1
 - dir2
    - file2
    - file3
 - dir3
    - file4 
```





# `gzip`

to compress content of directory `dir1`, first we need to archive it with program `tar` through the following command:

```powershell
tar cvf archived.tar dir1/
```



the above command will archive the entire content of `dir1` inside a file named `archived.tar`. you can see the structure inside this file by the following command:

```powershell
tar tf archived.tar
```

  

this will print the following output:

```
dir1/
dir1/file1
dir1/dir3/
dir1/dir3/file4
dir1/dir2/
dir1/dir2/file2
dir1/dir2/file3
```

 

now we can compress the archived file by the use of program `gzip` as follow:

```powershell
gzip archived.tar
```



above command will remove `archived.tar` file and generate a compressed file named `archived.tar.gz`.



to decompress content of directory `archived.tar.gz`, first we need to unzip it and then extract content of archived file as following 2 commands:

```powershell
gunzip archived.tar.gz
```



this command will unzip the file `archived.tar.gz` to `archived.tar` file and remove the file `archived.tar.gz`.



```powershell
tar xvf archived.tar
```



# `zcat`

this command will show the content of the files inside a compressed file without having to do extract it. so we can use this behavior and mix it with pipe facility to decompress our compressed file in one command line as follow:

```powershell
zcat archived.tar.gz | tar xvf -
```

  

in Linux, the program `tar` has an option named `z`, that is to use `zcat` capability. so we can shorten our above command as follow to decompress our file as follow:

```powershell
tar zxvf archived.tar.gz
```

   

also we and archive and compress our directory `dir1` at once as following command:

```
tar zcvf archived.tar.gz dir1/
```

