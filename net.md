# netns

to be able to add new network namespace, the package `iproute2` need to be installed in host machine. in alpine linux run the following command to install it:

```powershell
apk add iproute2
```

 

after above command executed successfully, to add a new network namespace execute the following command:

```powershell
ip netns add [your desigred name]

example:
 - ip netns add netns0
```

 