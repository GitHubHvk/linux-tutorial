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



following command checks the validation of config file:

- nginx -t



***

# Important Commands

to start nginx service, each one of the following commands will do the job:

- nginx
- systemctl start nginx



While your nginx instance is running, you can manage it by sending signals.

- nginx -s quit
  - to stop nginx processes with waiting for the worker processes to finish serving current requests.
- nginx -s stop
  - fast shutdown.

- nginx -s reload
  - Changes made in the configuration file will not be applied until the command to reload configuration is sent to nginx or it is restarted.
  - it checks the syntax validity of the new configuration file and tries to apply the configuration provided in it. If this is a success, the master process starts new worker processes and sends messages to old worker processes, requesting them to shut down. Old worker processes, receiving a command to shut down, stop accepting new connections and continue to service current requests until all such requests are serviced. After that, the old worker processes exit.
- nginx -s reopen



***

# Config File Structure

nginx has modules that are controlled by directive inside config file. By default, the nginx configuration file can be found in:

- /etc/nginx/nginx.conf
- /usr/local/etc/nginx/nginx.conf
- /usr/local/nginx/conf/nginx.conf



directives are names that are separated from their values by space. values could be simple or block.

simple value is one line that ends with semi-colon. a block is consist of other directives wrapped in braces. 

each directive that have other directives inside braces, is called a context. for example ((location)) context, or ((http)) context, or ((server)) context.

the directive that has braces but itself is not inside any block, is said to be declared in the ((main)) context. like ((event)), and ((http)). 



### Directive

the option that consists of name and parameters; it should end with a semicolon; like bellow:

- gzip on;



### Context

the section where you can declare directives (similar to scope in programming languages):

- ```nginx
  worker_processes 2; # directive in global context
  
  http {              # http context
      gzip on;        # directive in http context
  
    server {          # server context
      listen 80;      # directive in server context
    }
  }
  ```



## Directive Types

There are 3 types of directives, each with its own inheritance model.



### Normal

Has one value per context. Also, it can be defined only once in the context. Subcontexts can override the parent directive, but this override will be valid only in a given subcontext.

- ```nginx
  gzip on;
  gzip off; # illegal to have 2 normal directives in same context
  
  server {
    location /downloads {
      gzip off;
      # gzip is off here
    }
  
    location /assets {
      # gzip is on here
    }
  }
  ```



### Array

Adding multiple directives in the same context will add to the values instead of overwriting them altogether. Defining a directive in a subcontext will override ALL parent values in the given subcontext.

- ```
  error_log /var/log/nginx/error.log;
  error_log /var/log/nginx/error_notive.log notice;
  error_log /var/log/nginx/error_debug.log debug;
  
  server {
    location /downloads {
      # this will override all the parent directives
      error_log /var/log/nginx/error_downloads.log;
    }
  }
  ```



### Action

Their inheritance behavior will depend on the module.

For example, in the case of the `rewrite` directive, every matching directive will be executed:

- ```
  server {
    rewrite ^ /foobar;
  
    location /foobar {
      rewrite ^ /foo;
      rewrite ^ /bar;
    }
  }
  ```

- in the above config file, If the user tries to fetch `/sample`:

  - a server rewrite is executed, rewriting from `/sample`, to `/foobar`
  - the location `/foobar` is matched
  - the first location rewrite is executed, rewriting from `/foobar`, to `/foo`
  - the second location rewrite is executed, rewriting from `/foo`, to `/bar`

as another example, in the case of (( return )) directive, the first one is returned if more than one is used in the same context.

- ```
  server {
    location / {
      return 200;
      return 404;
    }
  }
  ```

- in the above config file the `200` status is returned immediately.



***

# Sample Scenarios

to understand better how nginx config works, we would have some sample as follows:



### run static site

imagine we have an ((index.html)) file that want to serve it as a web page when browsing URI (( http://localhost:80 )). to do so we do following steps:

1. create a directory named (( /static_site )) in the root directory, and paste (( index.html )) file inside it.

2. create a file named (( my_static_site.conf )) inside directory (( /etc/nginx/sites/available/ )), and fill this with following scripts.

   1. ```nginx
      server{
       listen 80;
       server_name _;
       root /static_site;
       index index.html;
      }
      ```

3. run following command to create a linked file to (( my_static_site.conf )) inside directory (( /etc/nginx/sites-enabled )):

   1. ```powershell
      ls -s /etc/nginx/sites-available/my_static_site.conf /etc/nginx/sites-enabled
      ```

4. run the following command to reload config file in nginx without restarting it:

   1. ```nginx
      nginx -s reload
      ```



### run static site with image repository

imagine we have an ((index.html)) file that want to serve it as a web page when browsing URI (( http://localhost:80 )), and have some images that want to serve in web when browsing URI (( http://localhost:80/images/[image name].[image extension] )) . to do so we do following steps:

1. create a directory named (( /static_site_with_image_repository )) in the root directory, and paste (( index.html )) file inside it.

2. create a directory named (( /images )) inside directory (( /static_site_with_image_repository )) and paste your images inside it.

3. create a file named (( static_site_with_image_repository.conf )) inside directory (( /etc/nginx/sites/available/ )), and fill this with following scripts.

   1. ```nginx
      server{
          listen 80;
          server_name _;
          root /static_site_with_image_repository;
          index index.html;
          location /images/ {
          }
      }
      ```

4. run following command to create a linked file to (( static_site_with_image_repository.conf )) inside directory (( /etc/nginx/sites-enabled )):

   1. ```powershell
      ls -s /etc/nginx/sites-available/static_site_with_image_repository.conf /etc/nginx/sites-enabled
      ```

5. run the following command to reload config file in nginx without restarting it:

   1. ```nginx
      nginx -s reload
      ```




### run static site with proxy image repository

imagine we have an ((index.html)) file that want to serve it as a web page when browsing URI (( http://localhost:80 )), and have some images that want to serve in web when browsing URI (( http://localhost:80/images/[image name].[image extension] )), but to serve images we have another service hosting with port 8080. to do so we do following steps:

1. create a directory named (( /static_site_with_proxy_image_repository )) in the root directory, and paste (( index.html )) file inside it.

2. create a directory named (( /images/proxy_service )) inside directory (( /static_site_with_proxy_image_repository )) and paste your images inside it.

3. create a file named (( static_site_with_proxy_image_repository.conf )) inside directory (( /etc/nginx/sites/available/ )), and fill this with following scripts.

   1. ```nginx
      server{
          listen 80;
          server_name _;
          root /static_site_with_proxy_image_repository;
          index index.html;
          location / {
          }
          location /images/ {
              proxy_pass http://localhost:8080;
          }
      }
      ```

4. create a file named (( proxy_image_repository.conf )) inside directory (( /etc/nginx/sites/available/ )), and fill this with following scripts.

   1. ```
      server{
          listen 8080;
          server_name _;
          root  /images/proxy_service;
          
          location /images/ {
          }
      }
      ```

5. run following command to create a linked file to (( proxy_image_repository.conf )) and (( static_site_with_proxy_image_repository.conf )) inside directory (( /etc/nginx/sites-enabled )):

   1. ```powershell
      ls -s /etc/nginx/sites-available/proxy_image_repository.conf /etc/nginx/sites-enabled
      ```

   2. ```
      ls -s /etc/nginx/sites-available/static_site_with_proxy_image_repository.conf /etc/nginx/sites-enabled
      ```

6. run the following command to reload config file in nginx without restarting it:

   1. ```nginx
      nginx -s reload
      ```

