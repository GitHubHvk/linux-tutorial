# What is it?

when in Linux we need to install or update a software, we have at least to options available:

- manually

  - to manually install an app om Linux we need to have `deb` file which is installable file of the program we want to install, the use command `dpkg` with option `-i` followed by path to `deb` file. for example if we have the file `nginx.deb`, we could install it by executing command `dpkg -i nginx.deb`.

- by using a package manager.

  - package managers in a nutshell are we based repositories that stores installable applications files and their dependencies, that make it possible for Linux client to manages its applications (install, upgrade, remove) them by the help of that package manager. to do so, the client needs to have some configs in his Linux machine.

  

### Client Linux machine configs

an application to handle requests to remote package repositories is needed to be installed in client-side, and following are some of them:

- apt
  - stands for `advanced package tool`. by default its installed in `debian` and `ubuntu` branches of Linux.
- apt-get
  - by default its installed in `debian` and `ubuntu` branches of Linux.
- `apk`
  - by default its installed in `alpine` branches of Linux.



to say to the client package manager application ((`apt`)), where to get packages from. this is a file located in `/etc/apt/sources.list`. content of this file specifies the remote repositories in which packages and their dependencies reside. 



package manager client applications to secure their connection to the remote repositories, uses a key that is registered in both sides, Linux client and remote repository. to see the list of keys registered in our client machine, we can execute the following command:

```powershell
apt-key list
```



### set proxy

to set a proxy for client package manager app, it is needed to create file in path `/etc/apt/apt.conf.d` named whatever you want for example `proxy.conf` and add yout proxy server IP as follow:

```powershell
Acquire::http::Proxy "http://username:password@proxy-server-ip:8181/";
Acquire::https::Proxy "https://username:password@proxy-server-ip:8182/";
```



### install and app using package manager

to do so, we could use `install` option; say we want to install `nginx`, so we could execute the following command:

```powershell
apt install nginx
```

 

### upgrade a package

to upgrade all previously installed packages in Linux we can execute following command:

```powershell
apt upgrade
```



### remove redundant dependencies

after installing, removing, upgrading packages in long term, some redundant dependencies remains in system that is better to remove them. by running following command we can remove them:

```powershell
apt autoremove
```

 