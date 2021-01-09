
**Network Information**

- Client Web Server1 192.168.1.224
- Client Web Server2 192.168.1.137
- Client Web Server3 192.168.1.294
- NFS Server 192.168.1.131
- Load Balancer 192.168.1.223
- MySQL Server 192.168.1.132



** Local DNS Setup for the Load Balancer Using /etc/hosts File**


Setting static IP on the load balancer

Edit /etc/sysconfig/network config file

```sudo nano nano /etc/sysconfig/network```

![](config.jpg)

Edit the coonfig file of the network interace being used 

```sudo nano /etc/sysconfig/network-scripts/ifcfg-enp0s8sudo nano /etc/sysconfig/network-scripts/ifcfg-enp0s8```

![](enp0s8.jpg)


Edit the hosts file on the load balancer as follows and do the same on the webservers as shown below:

![](https://github.com/drazen-dee28/-Load-Balancing-Solution-for-a-Tooling-Website/blob/main/images/hosts.jpg)



I use ping command to check if the load balancer can communicate with the webservers

![](https://github.com/drazen-dee28/-Load-Balancing-Solution-for-a-Tooling-Website/blob/main/images/ping.jpg)



**Configuring Apache Load Balancer**

Install Apache on the Load balancer

```sudo apt install apache2```



```Installing HAproxy load balancer```

```sudo apt-get update```


I  installed HAproxy using the following command in Terminal:

```sudo sudo apt install haproxy```



**Configuring HAproxy as a load balancer**

I edited the /etc/haproxy/haproxy.cfg file by appending the following lines in the haproxy.cfg file replacing the IP addresses as shown below:

![](https://github.com/drazen-dee28/-Load-Balancing-Solution-for-a-Tooling-Website/blob/main/images/loadbalancer.jpg)




I configured HAproxy monitoring by enabling the HAproxy “stats” page and securing it with http basic authentication with username and password. This is done by adding the following lines to config file located at /etc/haproxy/haproxy.cfg as shown below:



![](https://github.com/drazen-dee28/-Load-Balancing-Solution-for-a-Tooling-Website/blob/main/images/stats.jpg)


I verified the configuration file using the below command in Terminal:

```haproxy -c -f /etc/haproxy/haproxy.cfg```

![](https://github.com/drazen-dee28/-Load-Balancing-Solution-for-a-Tooling-Website/blob/main/images/verification.jpg)



I applied the configurations, restarted the HAproxy service: 
```sudo systemctl restart haproxy.service```

I got an error  ```failed to start haproxy load balancer``` so i disable and stopped Apache with

sudo systemctl disable apache2 &  sudo systemctl stop apache2  ,i then restarted the HAproxy service

```sudo systemctl restart haproxy.service```


Then i checked the status of HAproxy service

 ```sudo systemctl status haproxy.service```

 ![](status.png)