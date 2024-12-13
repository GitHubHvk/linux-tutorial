# SSH

with this program you can connect remotely to your linux server through a command-line, foe example PowerShell from windows.



***

# Enable SSH In VM Linux

in network setting of your VM, select two adaptors:

- NAT
  - to be able to access internet from inside VM Linux.
- Host Only
  - to be able to access to your VM Linux remotely (connect to it through SSH).

then start VM Linux and login directly to it, and check command bellow to see if adaptor related to ((Host Only)) get an IP address or not; 

- ifconfig

if it does not get IP address yet, then run command:

- dhclient



after that you should enable SSH in your Linux as server. to do that run the following commands:

- apt-get purge openssh-server
- apt-get install openssh-server



now you can use connect to your VM Linux server through power-shell in your operating system by running following command:

- ssh [user name in VM Linux]@[Host Only Adaptor IP of VM Linux]
  - for example: ssh hamed@192.168.56.103 



# Putty CLI

also it is possible to connect to a Linux via putty command line; say we have a server with IP `192.168.56.101` and a username `hamed`, and password `123`, to connect to it with putty CLI we can execute the following command:

```powershell
putty.exe -ssh hamed@192.168.56.101 -pw "123"
```

  