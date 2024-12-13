# sudoers

to add a user to sudoers, at least there are 2 methods:

1. edit sudoers file.
2. use command.



### Edit `sudoers` file

 - `su -`
 - nano /etc/sudoers
 - add bellow line to the file and save and close
   - hamed ALL=(ALL=ALL) ALL



**caution:** it is better to open and modify sudoers file with (( `visudo` )) command, it checks the syntax before saving the file. 



### Command

to do so, we can use the program `adduser`, to add the desired user to the predefined `sudo` group, as follow:

```powershell
adduser hamed sudo
```

 

above command will add the user `hamed` to the `sudo` group.

The change will take effect the next time the user `hamed` logs in..
