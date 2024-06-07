# What is Logging

it is referred to collecting and storing logs of services and servers.



# Best Practice

it is recommended to have a centralize server to store all other servers and services in it.

the overall view of the this logging method, is as follow:

- there are some services that are called log forwarder, that the send logs of the server and it's services to a central server.
- in central server they are stored and analyzed.



# Log Rotate

when logs became too much, to avoid extra storage using, based on some criteria including log size, an time as examples, log files compressed, removed or shrunk. 



# Some Tools

- ELK
  - this is a stack of three popular projects: **Elasticsearch, Logstash, and Kibana**
- Loki
  -  an advanced log aggregation system developed by Grafana Labs.  