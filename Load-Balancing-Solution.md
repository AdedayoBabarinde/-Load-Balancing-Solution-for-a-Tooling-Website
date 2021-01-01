
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

![](https://github.com/drazen-dee28/-Load-Balancing-Solution-for-a-Tooling-Website/blob/main/images/netplan.png)





To configure the static IP, copy and paste the configuration in to the yaml config

```sudo nano /etc/netplan/01-network-manager-all.yaml```
![](https://github.com/drazen-dee28/-Load-Balancing-Solution-for-a-Tooling-Website/blob/main/images/yaml.png)

I saved the file and run the netplan command below to effect the changes

```sudo netplan apply```




Edit the hosts file on the load balancer as follows and do the same on the webservers as shown below:

![](https://github.com/drazen-dee28/-Load-Balancing-Solution-for-a-Tooling-Website/blob/main/images/hosts.png)