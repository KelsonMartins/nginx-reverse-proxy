# Nginx Reverse Proxy

While developing a web application, a common method of calling the application from a local machine is through `http://localhost:{ports}`, which essentially means that we are required to expose several ports to access different modules of the application.

This repository is self-explanatory, it contains an example `nginx reverse proxy` configuration for Docker containers.
For implementation, we are setting up a Docker Wordpress website with MySql Db.

# What is a Proxy?

A proxy acts as a firewall between you and the internet. A proxy provides security, privacy, and other levels of functionality. You can set up a proxy server in Nigeria. Then you can make an HTTP request on the internet and that request would go to Nigeria first and then on the internet in easy language.

An example of proxy would be multiple computers in one company or building. All computers inside a company have IP address under the supreme IP address of a company. All the internet requests are made under one IP address which makes it harder to trace back to an individual computer.

Proxy is all about making secure external requests.

## Why Do We Need Reverse Proxy?

The most obvious reason for using Reverse Proxy is to avoid changing ports every time you try to access different modules of the application through the same URL. Through Reverse Proxy we can reach frontend, backend, or other services without changing port through a single domain. Another important reason for using Reverse Proxy is to mask services behind a proxy and avoid dealing with CORS issues.

Reverse proxy lets you make secure internal requests.

```yml
## Without Reverse Proxy 
# Domain Name: http://mydomain.com 
# Mysql wordpress: http://mydomain.com:10088 
# Angular app: http://mydomain.com:7787 
# Backend: https://mydomain:9876

## With Reverse Proxy 
# Domain Name: http://mydomain.com 
# Mysql wordpress: http://mydomain.com/db 
# Angular app: http://mydomain.com/ang 
# Backend: https://mydomain/wp
```

> ATTENTION!:
> > Docker uses iptables to access incoming connections.

## Run Sample

To run this sample, we start by starting the docker containers. We can do so, running the command bellow:

```powershell
docker-compose up -d 
```
When complete, we should have two containers deployed, one of which we cannot access directly.

We can check our applications (one with NGINX and the other one with Apache).
Navigate to http://localhost:8080, and this will hit NGINX Reverse Proxy which will in turn will load the NGINX web application.

We can also check with navigating to http://localhost:8081 or http://localhost/wp, through the Nginx Reverse Proxy asymmetric path and the Apache web application will be loaded.

### Troubleshooting

If you're having problems whilst copying missing files, they will most likely be connected to missing self-signed generated certs.
To create these as part your local dev environment, please run the commands bellow: 
>> cd certs
>> openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout your_website_domain.key -out your_website_domain.crt

Once the services are up, try to connect your web application to the localhost link. If it is not answered, check your iptables table for correctness.
> $ sudo iptables -t nat -L -n


