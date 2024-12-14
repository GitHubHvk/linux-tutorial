# sudoers

to add a user to sudoers, at least there are 2 methods:

1. edit sudoers file.
2. use command.



### Edit `sudoers` file

at least there are 2 methods to add a user to sudoers list by editing the file. config it for the desired user, or create a group of users and change the role of the group;



following steps are for single user:  

 - `su -`
 - nano /etc/sudoers
 - add bellow line to the file and save and close
   - hamed ALL=(ALL=ALL) ALL



following steps are for group of users:  

 - `su -`
 - nano /etc/sudoers
 - add bellow lines to the file and save and close (this will create a group named `ADMINS` and adds users `user1` and `user2` to it)
   - User_Alias ADMINS = user1, user2
   - ADMINS ALL = NOPASSWD: ALL
   - ADMINS ALL = (ALL) ALL



**caution:** it is better to open and modify sudoers file with (( `visudo` )) command, it checks the syntax before saving the file. 



### Command

to do so, we can use the program `adduser`, to add the desired user to the predefined `sudo` group, as follow:

```powershell
adduser hamed sudo
```

 

above command will add the user `hamed` to the `sudo` group.

The change will take effect the next time the user `hamed` logs in..
