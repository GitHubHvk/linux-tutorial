# Setup Repository

- nano /etc/resolv.conf

- replace all content of the file with the following line:

  - nameserver 8.8.8.8

- nano /etc/apt/sources.list

- replace all content of the file with the following (https://wiki.debian.org/SourcesList):

  - ```
    deb http://deb.debian.org/debian bookworm main
    deb-src http://deb.debian.org/debian bookworm main
    
    deb http://deb.debian.org/debian-security/ bookworm-security main
    deb-src http://deb.debian.org/debian-security/ bookworm-security main
    
    deb http://deb.debian.org/debian bookworm-updates main
    deb-src http://deb.debian.org/debian bookworm-updates main
    
    ```



- run following commands respectively:

  - ```cmd
    apt update
    
    apt upgrade
    ```



------

# Install VSCode

[Source]: https://linuxiac.com/how-to-install-vs-code-on-debian-12/	"Install VSCode Steps"

- su -

- apt install software-properties-common apt-transport-https wget gpg
- wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
- install -D -o root -g root -m 644 packages.microsoft.gpg /usr/share/keyrings/packages.microsoft.gpg
- sh -c 'echo "deb [arch=amd64,arm64,armhf signed-by=/usr/share/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list'
- apt update
- apt install code

