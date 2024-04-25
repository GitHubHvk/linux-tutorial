# What Is It?

it is a package of all dependencies like files, binaries, libraries, and configurations that a container needs to be run independently.



***

# Layers

an image is created by the help of a `dockerfile ` (( this is a file in which there are some lines of code, that each line is executed separately  )). also it is possible to create an image from a container.

each line in docker file will add a layer to the image.

layers are immutable and each layer represents a set of file system changes that add, remove, or modify files. these 2 principles let you extend or add to existing images. for example when your are building a python app, you cand start from the python image and add additional layers to install your app dependencies  and add your code.

layers are reused between images. it means build process of the second image will use similar layers from the first image and cause faster build and lower storage and band-width required to distribute the image.



### See Layers

to see layers of an image run the following command:

```
docker image history [image name]

example:
 - docker image history nginx
```



***

# Dockerfile

this is a text-based file containing instructions to build a docker image. each instruction in this file will result to a layer in image.



following instructions are some of the most important ones:

- FROM <base-image>
  - specify the base image that the build process will extend.
- WORKDIR <path>
  - specify the path in the inside the image that files will be copied and commands will be executed.
- COPY <host-path> <image-path>
  - tells the builder to copy files from host to image.
- RUN <command>
  - to run the specified command when building the image. 
- ENV <name> <value>
  - set an environment variable that the running container will use. 
- EXPOSE <port number>
  - indicates a port that image would like to expose.
- USER <user or uid>
  - sets the default user for all subsequent instructions. 
- CMD ["<command>", "<arg1>"]
  - sets commands a container using this image will run.



***

# build

by executing this command, docker will create an image based on instructions written in a docker file.

```powershell
docker build .
```

 

the `.` in the above command specify the path of the `dockerfile` and other referenced files. this path is called build context. 

after running above command, an image will be created without tag. to tag it at the build time you can can use the option `-t` as follow:

```powershell
docker build -t <your tag> .
```

  

***

#  Tagging

tagging is the method to set a memorable name for image. but it has a structure that is important. by this instruction we can specify which registry to publish, the repository, namespace, and version. the structure is as follow:

```powershell
[[HOST]:[PORT_NUMBER]/]PATH[:TAG]
```

 

- HOST
  - Specifies the registry address to publish image. if not specified the default that is ducker registry is used.
- PORT_NUMBER
  - registry port number if a hostname is provided.
- PATH
  - path of the image in registry. for docker hub registry it is as `[NAMESPACE/]REPOSITORY`. if no `NAMESPACE` is used then the default value that is `library` is used.
  - the path format for other registries may differ.
- TAG
  - a custom identifier that is usually used to specify version. if no tag is used, the default value `latest` is used.



examples:

- docker/welcome-to-docker
  - registry is `docker.io`, namespace is `docker`, repository is `welcome-to-docker`, and version is `latest`.
- ghcr.io/docker-samples/example:pr-311
  - registry is `ghcr.io`, namespace is `docker-samples`, repository is `example`, and version is `pr-311`.



to tag an image execute following command:

```powershell
docker image tag <local image id/locat image tag> <your desigred tag structure>

example:
 - docker image tag 746e docker.io/library/node-test:1.0
 - docker image tag myapp docker.io/library/node-test:1.0
```



***

# push

to push an image to the registry use the following command:

```powershell
docker push <your image tag	>
```

 

***

# dangling images

these are those images that has no tag and there is no purpose for them. they are created during carelessly build processes. for example imagine we have an image tagged `n1/r1:1.0` and we change the `dockerfile` and build another image with exact same tag as `n1/r1:1.0`; the result will be a new image tagged `n1/r1:1.0` and an old image without tag which is called `dangling image`. to get rid of these images we can delete them by running following command:

- docker rmi $(docker images -q --filter "dangling=true")
  -  `rmi`: a method to remove image.
  - `$`: pass the query result to `rmi` method.
  - `-q`: to show just the id of the images.
  - `--filter`: is an option to pass a filter expression to narrow the result of images list. here we want to list `gangling images`.   
