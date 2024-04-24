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
