# Proxy Types

Proxy in a nutshell is a program that acts on behalf of another machine (a client, web server, or other backend server). whether this machine is client side or server side will specify the type of proxy.

totally there are two categories for proxies:

- Reverse Proxy
  - this proxy is set in server side.
  - a reverse proxy serves on behalf of servers, managing requests from external clients, providing load balancing, and increasing security by concealing server identities.
  - safeguards servers from external threats.
  - a reverse proxy hides the identities of servers.
- Forward Proxy
  - this one is set in client side.
  - controlling access to internet resources, enhancing security, and enforcing policies.
  - protects internal network clients.
  - a forward proxy hides clients' identities.



![](Proxy_Diagram.png)



some Reverse proxy programs are as follow:

- httpd
- nginx
- iis
- haproxy



***

# Set Local Domain

first find what is your system IP by running following command:

-  ifconfig -a

after that open file (( /etc/hosts )) and add domain and IP address at the end of it with this format (( [system IP] [domain address] )); as following examples:

- 10.0.2.15 hamed.ir
- 10.0.2.15 wp.hamed.ir

you can have multiple domains by adding multiple mapping of IP to domain at the of this file. 







