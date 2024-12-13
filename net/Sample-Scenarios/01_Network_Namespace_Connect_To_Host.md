# Subject

we want to:

1. create a network namespace named `netns0`.
2. add a pair of ethernet devices named `veth0` and `ceth0`, and send `ceth0` to namespace `netns0`.
3. add IP to each ethernet devices, and configure them.
4. test our network namespace.



***

# Create Network Namespace

before creating the namespace, create a file named `inspect-net-context.sh` in some directory of the host machine, for example in root directory, and grant read, write and execute privileges to all users by executing command `chmod u+rwx,o+rwx,g+rwx inspect-net-context.sh`: 

```powershell
echo "# Network device"
ip link list

echo -e "\n# Route table"
ip route list

echo -e "\n# iptables rules"
iptables --list-rules
```

 

execute this file to inspect the current status of your network setting.



**HINT:** to execute this file simply call `~/inspect-net.context.sh`.



now execute the following command to create a network namespace named `netns0`:

```powershell
ip netns add netns0
```

 

to check if the network namespace is added, execute command `ip netns list`; you should see a list containing `netns0`.



now execute the following command to switch to network namespace `netns0`:

```powershell
nsenter --net=/run/netns/netns0 bash
```

 

now again execute `inspect-net-context.sh` and compare the result with previous network settings. it is different. 



switch back to host network namespace.



**HINT:** to switch back to the host network namespace, execute `exit`.   



***

#  Create Pair of ethernet devices

to be able to communicate between host machine network namespace and `netns0` network namespace we need to have a pair of network devices to make it possible for us. to create them, execute following command: 

```powershell
ip link add veth0 type veth peer name ceth0
```



now check if it is added by executing `ip link list`. 



to this state we have created both ethernet devices in our host machine network namespace; we need to move one of the to `netns0` namespace and set IP for each of them, to be able to communicate between host network namespace and `netns0` namespace. to do so, switch to hot network namespace and execute following commands:



1. move ethernet device `ceth0` to network namespace `netns0`:

   - ```powershell
     ip link set ceth0 metns netns0
     ```

2. start ethernet devices `veth0` and `lo`:

   - ```powershell
     ip link set lo up
     ip link set veth0 up
     ```

3. set IP address to ethernet device `veth0`:

   - ```powershell
     ip addr add 172.18.0.11/16 dev veth0
     ```

4. switch to network namespace `netns0`:

   - ```powershell
     nsenter --net=/run/netns/netns0 bash
     ```

5. start ethernet devices `ceth0` and `lo`:

   - ```powershell
     ip link set lo up
     ip link set ceth0 up
     ```

6. set IP address to ethernet device `ceth0`:

   - ```
     ip addr add 172.18.0.10/16 dev certh0
     ```

   

   

***

# Test

now switch to host machine network namespace and ping the IP of `netns0` network namespace IP as follow:

```powershell
ping -c 2 172.18.0.10
```



if the ping result is successful, it shows you have communication ability from host to `netns0`. to test ability to communicate from `netns0` to host, first switch to  `netns0` network namespace and ping IP of host machine network namespace as follow:

```powershell
ping -c 172.18.0.11
```





