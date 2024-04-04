# Purpose

is open source software for web serving, reverse proxying, caching, load balancing, media streaming, and more.

It can successfully handle high loads with many concurrent client connections, and can function as a web server, a mail server, or a reverse proxy server.

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
  error_log /var/log/nginx/error_notice.log notice;
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

# Server Context

this context is member of (( http )) context.

Inside nginx, you can specify multiple virtual servers to process the requests by using server context.

```nginx
server {
  listen      *:80 default_server;
  server_name hamed.co;

  return 200 "Hello from netguru.co";
}

server {
  listen      *:80;
  server_name foo.co;

  return 200 "Hello from foo.co";
}

server {
  listen      *:81;
  server_name bar.co;

  return 200 "Hello from bar.co";
}
```



in above config file:

- request to (( http://hamed.co:80 )) and (( http://bar.co:80 )), and (( http://www.foo.co:80 )) will result in processing the first virtual server.
- request to (( http://foo.co:80 )) will result in processing the second virtual server.
- request to (( http://bar.co:81 )), (( http://foo.co:81 )) will result in processing the third virtual server.



### server block selection

nginx first try to match a block by listen directive, and if multiple candidates  are selected then try to match with server_name directive. if again multiple blocks remain in selection list, the one with (( default_server )) tag is selected.



***

# listen directive

this directive is a member of server context.

this is combination of IP and port, and each one which is not specified, the default value will be set instead. for example if no listen is set at all, the values is pretended to be (( 0.0.0.0:80 )) by nginx.

- A block with no `listen` directive uses the value `0.0.0.0:80`.
- A block set to an IP address `111.111.111.111` with no port becomes `111.111.111.111:80`
- A block set to port `8888` with no IP address becomes `0.0.0.0:8888`

### value

- An IP address/port combo.
- A lone IP address which will then listen on the default port 80.
- A lone port which will listen to every interface on that port.
- The path to a Unix socket.



### selection order

- nginx first looks to match exact value of port in request matching in server blocks.

- then attempts to collect a list of the server blocks that match the request, most specifically based on the IP address and port
  - This means that any block that is functionally using `0.0.0.0` as its IP address (to match any interface), will not be selected if there are matching blocks that list a specific IP address.

in above selection order if one match is found, the request is sent to that block to be processed, otherwise if multiple matches is found, then the selection process process will use ((server_name)) directive.



***

# server_name directive

this directive is a member of server context.

this directive is the value of the ((Host)) header in request. ((Host)) header in request is the the value of the domain requesting.

### selection order

- Nginx will first try to find a server block with a `server_name` that matches the value in the `Host` header of the request *exactly*. If this is found, the associated block will be used to serve the request. If multiple exact matches are found, the **first** one is used.
- If no exact match is found, Nginx will then try to find a server block with a `server_name` that matches using a leading wildcard (indicated by a `*` at the beginning of the name in the config). If one is found, that block will be used to serve the request. If multiple matches are found, the **longest** match will be used to serve the request.
- If no match is found using a leading wildcard, Nginx then looks for a server block with a `server_name` that matches using a trailing wildcard (indicated by a server name ending with a `*` in the config). If one is found, that block is used to serve the request. If multiple matches are found, the **longest** match will be used to serve the request.
- If no match is found using a trailing wildcard, Nginx then evaluates server blocks that define the `server_name` using regular expressions (indicated by a `~` before the name). The **first** `server_name` with a regular expression that matches the “Host” header will be used to serve the request.
- If no regular expression match is found, Nginx then selects the default server block for that IP address and port.



***

# location context

it is part of server context. each server block could have multiple location contexts. also it could be nested in another location context.

this context is to handle client request for resources, like images, documents, videos, and so on.



this context has 2 parameters as follow:

- modifier: this is optional and is used to affect the way that the Nginx attempts to match the location block.
  - (none): If no modifiers are present, the location is interpreted as a *prefix* match. the location given will be matched against the beginning of the request URI to determine a match.
  - (=): this block will be considered a match if the request URI exactly matches the location given.
  - (~): this location will be interpreted as a case-sensitive regular expression match.
  - (~*): the location block will be interpreted as a case-insensitive regular expression match.
  - (^~): if this block is selected as the best non-regular expression match, regular expression matching will not take place.
- location_match: the URI to match.

```nginx
location optional_modifier location_match {

    . . .

}
```



### examples:

- location /site {} will match all following URIs:
  - /site
  - site/page1
  - /site/page1/index.html
  - /site/index.html
- location = /page1 {}:
  - will match (( /page1 ))
  - will not match (( /page1/index.html ))
  - will not match (( /page1/site ))
- location ~ \\.(jpe?g|png|ico|gif)$ {}
  - will match (( /page1/a.png ))
  - will not match (( /page1/a.PNG ))
- location ~* \\.(jpe?g|png|ico|gif)$ {}
  - will match (( /page1/a.png ))
  - will match (( /page1/a.PNG ))



### selection order

Nginx evaluates the possible location contexts by comparing the request URI to each of the locations. it does following steps:

- it checks each URI against prefixed-based location (none regular).
- first location with (( = )) modifier is checked.
- hen moves on to evaluating non-exact prefixes. It discovers the longest matching prefix location for the given request URI, which it then evaluates as follows:
  - If the longest matching prefix location has the `^~` modifier, then Nginx will immediately end its search and select this location to serve the request.
  - If the longest matching prefix location *does not* use the `^~` modifier, the match is stored by Nginx for the moment so that the focus of the search can be shifted.
- moves on to evaluating the regular expression locations (both case sensitive and insensitive). If there are any regular expression locations *within* the longest matching prefix location, Nginx will move those to the top of its list of regex locations to check. Nginx then tries to match against the regular expression locations sequentially. The **first** regular expression location that matches the request URI is immediately selected to serve the request.
- If no regular expression locations are found that match the request URI, the previously stored prefix location is selected to serve the request.





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

