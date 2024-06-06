# Subject

we want to remote to a Linux server from Linux client with SSH by key, not by password.

also we want to limit users that can connect to server using `SSH`. 



# Environment Setup

to complete our subject, we need 2 Linux machine, one as a server, and the other as client.



### setup server machine

as a Linux server we have installed our `debian-12.2.0-amd64-DVD-1.iso` in `Oracle VM VirtualBox Manager`, and then did following steps:

1. setup it based of guide in URL `https://github.com/hamedvalizadeh/linux-tutorial/blob/master/Setup.md`.
2. enable `SSH` based on guide in URL `https://github.com/hamedvalizadeh/linux-tutorial/blob/master/ssh.md`.

**Caution:** it is not bad idea to config a VPN on this server based on guide in URL `https://github.com/hamedvalizadeh/linux-tutorial/blob/master/VPN.md`, but it is not necessary. 



# setup client machine

we setup our Linux client machine based on guide in URL `https://github.com/hamedvalizadeh/vagrant-tutorial/blob/master/Sample-Scenarios/Simple-VM-Run.md`.



***

# Generate keys in Client

to be able to connect by SSH key, we need to have 2 keys in client that are private and public keys, and copy public one to the server.

to generate them in client we need to have `openssh-client` installed in our client machine.

execute command `ssh-keygen`, this will ask you following questions:

1. Enter a file in which to save the key
   - if you do not enter your desired path, it will use its default (recommended to let it use its default path) .
2. Enter a passphrase
   - this is optional, but if you enter any thing, you will be asked to re-enter it every time you want to SSH to the Linux server (in our example we type `123456789`). 
   - if you do not type any thing and click enter key, this will be skipped.

finally it will show a message confirming that the keys has been generated. to check it, you can execute the command `sudo ls -l ~/.ssh/` the output should contains following files:

```powershell
-rw------- 1 vagrant vagrant 1766 Jun  6 05:48 id_rsa
-rw-r--r-- 1 vagrant vagrant  397 Jun  6 05:48 id_rsa.pub
```



the content inside the file `id_rsa` is the private key that need to be kept secret in your Linux client machine.

the content in the file `id_rsa.pub` is the public key that need to be copied to the Linux server.



***

# Copy public key to Server

to do this we have 3 methods, that doing one of them will copy the public key:

1. using command `ssh-copy-id`.
2. using `SSH`.
3. manually.



each of above methods at the will copy the content of the file `~/.ssh/id_rsa.pub` from client to the file `~/.ssh/authorized_keys` in account of desired user (in our example it is `hamed`) in Linux server.



### command `ssh-copy-id`

if we have this command available in client, and ability to connect to Linux server over SSH by password, we can execute the following command:

```powershell
sudo ssh-copy-id [user name of the Linux server]@[IP or hostname of the Linux server]
```



in our example it is as bellow:

```powershell
sudo ssh-copy-id hamed@192.168.56.106
```



it will ask you the password of the account `hamed` in Linux server, and then do the job. 



### using `SSH`

if you do not have `ssh-copy-id` available, but you have password-based SSH access to an account on your server, you can upload your keys using a conventional SSH method.

to do so, execute the following command in clint:

```powershell
cat ~/.ssh/id_rsa.pub | ssh [user name of the Linux server]@[IP or hostname of the Linux server] "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```



in our example it is as bellow:

```powershell
cat ~/.ssh/id_rsa.pub | ssh hamed@192.168.56.106 "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```



### copy manually

If you do not have password-based SSH access to your server available, you will have to do the above process manually.



execute the following command in client and copy the output:

```powershell
sudo cat ~/.ssh/id_rsa.pub
```



in your Linux server execute the following commands:

```powershell
mkdir -p ~/.ssh
echo public_key_string_from_linux_client >> ~/.ssh/authorized_keys
```



# connect to server using SSH key

that's it. you can just connect to the Linux server from client as usual, executing the normal SSH command in client machine as follow:

```powershell
ssh [user name of the Linux server]@[IP or hostname of the Linux server]
```

 

in our example it is like bellow:

```powershell
ssh hamed@192.168.56.106
```



this will ask you for the passphrase of the key you generated in client (in our example it is `123456789`).



***

# Limit users in Server to use `SSH`

we can limit users with ability to connect to the server with `SSH`. to do so, in server open the file `/etc/ssh/sshd_config` using the following command:

```powershell
sudo vim /etc/ssh/sshd_config
```

  

and add the following line at the end of the file:

```powershell
AllowUsers root hamed 
```



finally execute the bellow command to restart the `SSH` service in the server:

```powershell
systemctl restart sshd
```

 