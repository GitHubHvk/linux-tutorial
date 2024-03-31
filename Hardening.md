# Introduction

this is a concept and contains all the possible steps to make the linux server more and more secure.



***

# Suggested Steps

- when partitioning assign obey following rules:
  - assign 2 GB to the (( /boot )) partition.
  - assign 30% of available disk up to 50 GB to the (( / )) partition. it is better to set the type of this partition as (( LVM )).
  - assign remaining disk space to the (( /var )) partition. usually it is better to hold 70% of disk space for (( /var )) partition.
- to update kernel with the command (( apt update )).
- inspect installed packages and minimize the vulnerabilities.
- inspecting open ports.
  -  the only way to connect to the linux server should be through (( SSH ))
  - it's better to config (( SSH )) to use accept (( secret key )) to login.
- to have password policy.
- to lock user after wrong login.
- to config (( iptables )).
- log activities.
- enable SELinux.
- Disable USB usage



**HINT:** one tool to inspect your Linux vulnerabilities, is lynis.

***

# LYNIS Installation

to install it there are some methods which one of them is direct download. follow these steps to install it:

- create a directory and go to it:
  -  mkdir -p /usr/local/mylynis
  - cd /usr/local/mylynis 
- download the compressed file of (( lynis )):
  - wget https://cisofy.com/files/lynis-3.0.9.tar.gz
- extract it inside the path in which you are. after extracting you will have a folder named (( lynis )) in which there are some files including an executable file name (( lynis )):
  - tar xfvz lynis-3.0.9.tar.gz
- go to (( lynis )) folder:
  - cd lynis
- now run the following command to analyze your system vulnerabilities:
  - ./lynis audit system

***

# Manage Password Policies

in Debian distro open file (( /etc/pam.d/common-passwd )) and add following lines to it to prevent reusing four previous old passwords:

```
auth        sufficient    pam_unix.so likeauth nullok
password 	sufficient	 pam_unix.so remember=4
```



to have strong password policy open file (( /etc/pam.d/system-auth )) and add following line:

```
/lib/security/$ISA/pam_cracklib.so retry=3 minlen=8 lcredit=-1 ucredit=-2 dcredit=-2 ocredit=-1
```



to lock account after some failed attempts (say for example 5), open files (( /etc/pam.d/password-auth )) and (( /etc/pam.d/system-auth )), and add following lines:

```
auth required pam_env.so 
auth required pam_faillock.so preauth audit silent deny=5 unlock_time=604800 
auth [success=1 default=bad] pam_unix.so 
auth [default=die] pam_faillock.so authfail audit deny=5 unlock_time=604800 
auth sufficient pam_faillock.so authsucc audit deny=5 unlock_time=604800 
auth required pam_deny.so
```



**Caution:** after a user has been locked by passing password attempt limitation, only an admin user can unlock it by running following command:

```
/usr/sbin/faillock --user <userlocked>  --reset
```



The next tip for enhancing the passwords policies is to restrict access to the su command by setting the pam_wheel.so parameters in “/etc/pam.d/su”:

```
auth required pam_wheel.so use_uid
```



***

# Permissions and verification

it is good practice to set some permissions on critical files as follow:

```
chown root:root /etc/anacrontab
chmod og-rwx /etc/anacrontab
chown root:root /etc/crontab
chmod og-rwx /etc/crontab
chown root:root /etc/cron.hourly
chmod og-rwx /etc/cron.hourly
chown root:root /etc/cron.daily
chmod og-rwx /etc/cron.daily
chown root:root /etc/cron.weekly
chmod og-rwx /etc/cron.weekly
chown root:root /etc/cron.monthly
chmod og-rwx /etc/cron.monthly
chown root:root /etc/cron.d
chmod og-rwx /etc/cron.d

chmod 644 /etc/passwd
chown root:root /etc/passwd

chmod 644 /etc/group
chown root:root /etc/group

chmod 600 /etc/shadow
chown root:root /etc/shadow

chmod 600 /etc/gshadow
chown root:root /etc/gshadow

chown root:root /etc/grub.conf
chmod og-rwx /etc/grub.conf
```
