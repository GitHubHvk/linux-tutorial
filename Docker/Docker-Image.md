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

