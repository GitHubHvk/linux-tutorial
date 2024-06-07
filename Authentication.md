# What is Authentication

we mean the access control over files and folders, and in general or in a nutshell who can do what.



# Important File and Folders

there are some files and folders that are mainly used to store the info related to access control of users. they are as follow:

- /etc/shadow
- /etc/passwd
- /etc/sudoers
- /etc/ssh/sshd_config



# Add New User

to create a new user with `hamed2` as username, execute the following command:

```powershell
adduser hamed2
```



it will ask you some questions, and create the user.



# Add User to the `Sudoers` 

there a group in most of Linux distributions named `sudo`. in order to make the user able to do some actions more that their privileges, we can add them to this group; for example to add `hamed2` to `sudoers` group, execute the following command:

```powershell
adduser hamed2 sudo
```

  

**HINT:** there is a file placed in path `/etc/sudoers`that you can open in with command `visudo /etc/sudoers` and add the line `hamed2 ALL=(ALL=ALL) ALL` to make it possible for `hamed2` to do `sudo` actions. but it is not recommended, and the better way is to add the user `hamed2` to the `sudo` group.

**Caution:** because of line `%sudo   ALL=(ALL:ALL) ALL` in file `/etc/sudoers` it is possible to add users to `sudo` group and the user will be able to do `sudo` actions.



# Delete a User

to delete a user use the command `userdel`.



# Change Password Of User

the command is `passwd`.



# Allow Users Connect With `SSH`

if the options `AllowUsers` is not used in the file `/etc/ssh/sshd_config` all users are able to use `SSH` to connect to the server, otherwise thy are not.

imaging we have this option in that file which is previously configured as `AllowUsers root hamed`, in this condition if we want the new user `hamed2` be able to connect to the server with `ssh`, we should reconfigure this line as `AllowUSers root hamed hamed2`, and then to make `sshd` daemon affected of this change, execute the command `systemctl restart sshd`.



# Some important commands



- `last`
  - List Last Login Logs
- `w`
  - List the Active Users Sessions
- `whoami`
  - What is my User.
- `id`
  - shows ids of the user, and the groups which the user is member of. 
- `su`
- `sudo`
  - if the user is member of `sudo` group, this command let him to do some actions higher that his privileges. 





