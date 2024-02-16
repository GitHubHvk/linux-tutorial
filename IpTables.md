# What is IpTables

Iptables is a firewall program for Linux. It will monitor traffic from and to your server using tables. These tables contain sets of rules, called chains, that will filter incoming and outgoing data packets.

***

# Target

Targets specify where a packet should go.

followings are iptables targets:

- ACCEPT
  - will allow the packet to pass through.
- DROP
  - will not let the packet pass through.
- RETURN
  - stops the packet from traversing through a chain and tell it to go back to the previous chain.



followings are most important iptables extensions targets which are total of 39 targets:

- DNAT
- LOG
- MASQUERADE
- REJECT
- SNAT
- TRACE
- TTL



Targets are divided into terminating and non-terminating:

- Terminating.
  - ends rule traversal and the packets will be stopped there.
- non-terminating.
  - touch a packet in some way and the rule traversal will continue afterward.



***

# Chains

There are 5 chains in `iptables` and each is responsible for a specific task. they’re responsible for packets either as soon as they arrive.

- prerouting
  - to decide what happens to a packet as soon as it arrives at the network interface
- input
  - controls incoming packets to the server.
  - to open/block a port
- forward
  - filters incoming packets that will be forwarded somewhere else.
  -  is responsible for packet forwarding. We may want to treat a computer as a router and this is where some rules might apply to do the job.
- output
  -  filter packets that are going out from your server.
- postrouting
  - is where packets leave their trace last, before leaving our computer.





# Tables

different tables are responsible for different tasks. The list contains 

- filter
  - is responsible for filtering and restricting the packets to/from our computer.
  - to block a port to stop receiving anything, this is your stop.
- nat
  - is responsible for creating new connection. Which is shorthand for Network Address Translation.
- mangle
  - is for changing something inside the packet either before coming in or leaving out.
- raw
  - is dealing with the raw packet as the name suggests. Mainly this is for tracking the connection state.
- security.
  - It is responsible for securing your computer after the filter table. Which consists of [SELinux](https://www.redhat.com/en/topics/linux/what-is-selinux). If you’re not familiar with the term, it’s a very strong security tool on modern linux distributions.



**Caution:**

- The first two are the most used. 
- if we don’t specify any table, filter is used by default.





***

# Table-Chain-Target

each table contains a number of built-in chains and may also contain user-defined chains. 

each chain is a list of rules which can match a set of packets. 

each rule specifies what to do with a packet that matches. This is called a `target', which may be a jump to a user-defined chain in the same table.



***

# Cautions

before using ((iptables)) as our firewall it is better to stop other firewalls as follow:

- systemctl disable --now ufw



***

# Install iptables

- apt-get update
- apt-get install iptables



***

# List current configuration

- iptables -L -n -v --line-numbers
  - To list the current configuration
  - -L: for list.
  - -n: for numeric output (disable name resolution; results in faster performance).
  - -v: for verbose (more information is involved).



***

# Define Chain Rule

Defining a rule means appending it to the chain. To do this, you need to insert the **-A** option (**Append**) right after the iptables command, like so:

- iptables -A <chain> -i <interface> -p <protocol (tcp/udp) > -s <source> --dport <port no.>  -j <target>
  - -A: adding new rules to a chain.
  - -i: the network interface whose traffic you want to filter, such as eth0, lo, ppp0, etc.
  - -p: the network protocol where your filtering process takes place. It can be either **tcp**, **udp**, **udplite**, **icmp**, **sctp**, **icmpv6**, and so on. Alternatively, you can type **all** to choose every protocol.
  - -s:  the address from which traffic comes from. You can add a hostname or IP address.
  - -dport: the destination port number of a protocol, such as **22** (**SSH**), **443** (**https**), etc
  - -j: the target name (**ACCEPT**, **DROP**, **RETURN**). You need to insert this every time you make a new rule.



###### Some Scenarios:

To allow traffic on local host

- iptables -A INPUT -i lo -j ACCEPT

  - lo: loopback interface

  - The command above will make sure that the connections between a database and a web application on the same machine are working properly. 



Enabling Connections on HTTP, SSH, and SSL Port

- iptables -A INPUT -p tcp --dport 22 -j ACCEPT
- iptables -A INPUT -p tcp --dport 80 -j ACCEPT
- iptables -A INPUT -p tcp --dport 443 -j ACCEPT



To accept packets based on source:

- iptables -A INPUT -s 192.168.10.20 -j ACCEPT 



To Reject packet based on source:

- iptables -A INPUT -s 19.16.10.21 -j DROP



To Drop packets from range of IPs:

- iptables -A INPUT -m iprange --src-range 192.168.10.21-192.168.10.25 -j DROP

Dropping other traffics

- iptables -A INPUT -j DROP
  - It is crucial to use the **DROP** target for all other traffic after defining **–dport** rules. 
  - Now, the connection outside the specified port will be dropped.
  

***

# Deleting Rules

To Delete all rules and work in a clean state:

- iptables -F



***

# Delete a Rule

to delete a rule we should specify which number in which chain to delete.

to get the number we should pick the value from ((num)) column from result of the following command:

- iptables -L --line-numbers

then run the following command:

- iptables -D INPUT 2



***

# Stop every in/out packet

- iptables -P INPUT DROP
- iptables -P OUTPUT DROP
- iptables -P FORWARD DROP
  - -P: for policy



***

# Persist Changes

The iptables rules that we have created are saved in memory. That means we have to save them to a file to be able to load them again after a reboot. To make these changes you can use these commands depending if you are saving IPv4 or IPv6 rules:

- iptables-save > /etc/iptables/rules.v4
- iptables-save > /etc/iptables/rules.v6



whenever you restart your VPS you will need to load the saved rules with the following commands:

- iptables-restore < /etc/iptables/rules.v4
- iptables-restore < /etc/iptables/rules.v6



If you want for the loading process to be completely automatic, you can set up ((iptables-persistent)) package and it will take care of loading the rules:

- apt install iptables-persistent

