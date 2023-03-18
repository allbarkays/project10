# LOAD BALANCER SOLUTION WITH NGINX AND SSL/TLS

## Project Task Architecture
This project consists of two parts:

1. Configure Nginx as a Load Balancer
2. Register a new domain name and configure secured connection using SSL/TLS certificates

The target architecture for this project looks like this:


![project-architecture.PNG](./images/project-architecture.PNG)



## Step 1 – CONFIGURE NGINX AS A LOAD BALANCER


* Spin up an EC2 Ubuntu 20.04 LTS based VM and named it LB-nginX with TCP port 80 and 443 opened for incoming traffic
* Updated /etc/hosts file 

`sudo vi /etc/hosts`

```
<WebServer1-Private-IP-Address> Web1
<WebServer1-Private-IP-Address> Web1
```

* Installed Nginx as LB to point traffic to the resolvable DNS names of the webservers


```
sudo apt update
sudo apt install nginx
```

Configure Nginx LB using Web Servers’ names defined in `/etc/hosts`

```
upstream myproject {
    server Web1 weight=5;
    server Web2 weight=5;
  }

server {
    listen 80;
    server_name www.domain.com;
    location / {
      proxy_pass http://myproject;
    }
  }

#comment out this line
#       include /etc/nginx/sites-enabled/*;
```

***server (webserver1 IP address);***

***server (webserver2 IP address);***



* Restart Nginx and make sure the service is up and running

```
sudo systemctl restart nginx
sudo systemctl status nginx
```

**Can also ensure Nginx stays active after reboot/restart**

`sudo systemctl enable nginx`


