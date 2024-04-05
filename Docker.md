# What Is It?

Is an open platform for developing, shipping, and running applications without any dependency to the host environment. it separates your application from host infrastructure, so cause software delivery to be mush more quicker, easier, safer, and stable. 



***

# Docker Structure

docker uses client-server architecture. there is an app that is running in client which communicates with a running app in the server with REST API. both of these apps could be on the same machines or be separated in local and host.



### Docker Daemon (dockerd)

this is the app which is running in host machine and is responsible of managing and creating images, containers, networks, and volumes. this app listens to the client API requests and execute the appropriate command in docker engine. 



### Docker Client (docker)

is an app that provides `CLI` for users to communicate with Daemon. this app sends the user entered commands to the `dockerd` to execute them in docker engine. this app uses Docker API to communicate with `dockerd`.



### Docker Desktop

this is an application installable on Mas, Linux, ore Windows, that consists of `dockecr client`, `docker daemon`, `docker compose`, `docker content trust`, `kubernetes`, and `credential helper`.



***

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



***

# Image

