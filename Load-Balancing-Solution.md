
**Network Information**

- Web Server1 192.168.1.223
- Web Server2 192.168.1.130
- Web Server3 192.168.1.294
- NFS Server 192.168.1.131
- Load Balancer 192.168.1.128
- MySQL Server 192.168.1.132



** Local DNS Setup for the Load Balancer Using /etc/hosts File**

I identified the name of the ethernet interface you want to configure

```ip a```

![](https://github.com/drazen-dee28/-Load-Balancing-Solution-for-a-Tooling-Website/blob/main/images/enp0s8.png)



I opened the Netplan configuration files  stored in the /etc/netplan directory on the Load Balancer to reveal the YAML configuration file which i will use to configure the IP addresses of the web servers.

```sudo nano /etc/netplan```

![](https://github.com/drazen-dee28/-Load-Balancing-Solution-for-a-Tooling-Website/blob/main/images/netplan.
png)





To configure the static IP, copy and paste the configuration in to the yaml config

```sudo nano /etc/netplan/01-network-manager-all.yaml```
![](https://github.com/drazen-dee28/-Load-Balancing-Solution-for-a-Tooling-Website/blob/main/images/yaml.png)

I saved the file and run the netplan command below to effect the changes

```sudo netplan apply```




Edit the hosts file on the load balancer as follows and do the same on the webservers as shown below:

![](https://github.com/drazen-dee28/-Load-Balancing-Solution-for-a-Tooling-Website/blob/main/images/hosts.png)



I use ping command to check if the load balancer can communicate with the webservers

![](https://github.com/drazen-dee28/-Load-Balancing-Solution-for-a-Tooling-Website/blob/main/images/check.png)



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