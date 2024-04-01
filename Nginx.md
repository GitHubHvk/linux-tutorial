# Purpose

is open source software for web serving, reverse proxying, caching, load balancing, media streaming, and more.

In addition to its HTTP server capabilities, NGINX can also function as a proxy server for email (IMAP, POP3, and SMTP) and a reverse proxy and load balancer for HTTP, TCP, and UDP servers.

Because it can handle a high volume of connections, NGINX is commonly used as a reverse proxy and [load balancer](https://www.nginx.com/blog/five-reasons-use-software-load-balancer/) to manage incoming traffic and distribute it to slower upstream servers – anything from legacy database servers to microservices.

Dynamic sites, built using anything from Node.js to PHP, commonly deploy NGINX as a content cache and reverse proxy to reduce load on application servers and make the [most effective use](https://www.nginx.com/blog/nginx-and-nginx-plus-everywhere/) of the underlying hardware.



***

# Processes

nginx has one master process and several worker processes. The main purpose of the master process is to read and evaluate configuration, and maintain worker processes. Worker processes do actual processing of requests.

the nginx master process id is written in (( /var/run/nginx.pid )) or (( /usr/local/nginx/nginx.pid )).

to get list of all running nginx process, execute the following command:

- ps -ax | grep nginx



***

# Config File

The way nginx and its modules work is determined in the configuration file. 

By default, the configuration file is named  ((nginx.conf)) and placed in the following directories 

- /usr/local/nginx/conf
- /etc/nginx
- /usr/local/etc/nginx



**Caution:** it is possible to categorize different part of config file in separate (( .conf )) file located in directory  (( /etc/nginx/conf.d )), and they will be loaded as nginx configuration settings.



***

# set up static site

https://medium.com/@jasonrigden/how-to-host-a-static-website-with-nginx-8b2dd0c5b301 



***

# Signals

after installing Nginx and starting it, we can control it through some signals as follow:

- nginx -s quit
  - to stop nginx processes with waiting for the worker processes to finish serving current requests
- nginx -s reload
  - Changes made in the configuration file will not be applied until the command to reload configuration is sent to nginx or it is restarted.
  - it checks the syntax validity of the new configuration file and tries to apply the configuration provided in it. If this is a success, the master process starts new worker processes and sends messages to old worker processes, requesting them to shut down. Old worker processes, receiving a command to shut down, stop accepting new connections and continue to service current requests until all such requests are serviced. After that, the old worker processes exit.
- nginx -s reopen





https://nginx.org/en/docs/beginners_guide.html