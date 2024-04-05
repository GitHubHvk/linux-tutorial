# Install Docker

as some distro maintainers include some unofficial packages in their Linux distribution, it is recommended to uninstall those packages before installing the docker. so run the following command to uninstall them:

- dpkg -r `docker.io`
- dpkg -r `docker-compose`
- dpkg -r `docker-doc`
- dpkg -r `podman-docker` 



If you have installed the `containerd` or `runc` previously, uninstall them to avoid conflicts with the versions bundled with Docker Engine:

- dpkg -r `containerd`
- dpkg -r `runc` 



before installing docker in new machine, you need to set up docker `apt` repository. so run the following commands:

1. apt-get update

2. apt-get install ca-certificates curl

3. install -m 0755 -d /etc/apt/keyrings

4. curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc

5. chmod a+r /etc/apt/keyrings/docker.asc

6. ```
   echo \
     "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
     $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
     sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   sudo apt-get update 
   ```



now everything is ready to install the docker. so run the following command:

- apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin



after installing docker, to test it is installed correctly, run the following command:

- docker run hello-world
  - When the container runs, it prints a confirmation message and exits.