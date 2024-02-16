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



**HINT:** one tool to inspect your linux vulnerabilities, is lynis.

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

