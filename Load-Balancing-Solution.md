
**Network Information**

- Client Web Server1 192.168.1.224
- Client Web Server2 192.168.1.137
- Client Web Server3 192.168.1.294
- NFS Server 192.168.1.131
- Load Balancer 192.168.1.223
- MySQL Server 192.168.1.132



** Local DNS Setup for the Load Balancer Using /etc/hosts File**


Setting static IP on the load balancer


- Identify the network interface

```ip addr``
![](interface.png)


Edit /etc/sysconfig/network config file by changing the BOOTPROTO option to ```none``` and configuring the IP, route prefix, gateway and dns server statically as shown below

```sudo nano /etc/sysconfig/network-scripts/ifcfg-enp0s8```

![](config.jpg)



Save changes to network configurations.
```sudo nmcli connection down enp0s8 && sudo nmcli connection up enp0s8```

![](save.png)


Verify the IP has changed
```ip addr```
![](ipchange.jpg)

Edit the hosts file on the load balancer as follows and do the same on the webservers as shown below:

![](https://github.com/drazen-dee28/-Load-Balancing-Solution-for-a-Tooling-Website/blob/main/images/hosts.jpg)



I use ping command to check if the load balancer can communicate with the webservers

![](https://github.com/drazen-dee28/-Load-Balancing-Solution-for-a-Tooling-Website/blob/main/images/ping.jpg)



**Configuring Apache Load Balancer**

Install Apache on the Load balancer

- Download the source code with the command below

```wget http://www.haproxy.org/download/2.0/src/haproxy-2.0.7.tar.gz -O ~/haproxy.tar.gz```

- Extract the files

```tar xzvf ~/haproxy.tar.gz -C ~/```

- Change into the extracted source directory.

```cd ~/haproxy-2.0.7```

 - Compile the program

 ```make TARGET=linux-glibc```

 - Install HAProxy

 ```sudo make install```




**Configuring HAproxy as a load balancer**


- Add the following directories and the statistics file for HAProxy records.

```sudo mkdir -p /etc/haproxy```
```sudo mkdir -p /var/lib/haproxy```
```sudo touch /var/lib/haproxy/stats```


- Create a symbolic link to allow running of HAProxy commands as a normal user.

```sudo ln -s /usr/local/sbin/haproxy /usr/sbin/haproxy```

- I copied the haproxy.init file from the examples to /etc/init.d directory to add the proxy as a service to the system

```sudo cp ~/haproxy-2.0.7/examples/haproxy.init /etc/init.d/haproxy```





I edited the /etc/haproxy/haproxy.cfg file by appending the following lines in the haproxy.cfg file replacing the IP addresses as shown below:

![](https://github.com/drazen-dee28/-Load-Balancing-Solution-for-a-Tooling-Website/blob/main/images/loadbalancer.jpg)




I configured HAproxy monitoring by enabling the HAproxy “stats” page and securing it with http basic authentication with username and password. This is done by adding the following lines to config file located at /etc/haproxy/haproxy.cfg as shown below:



![](https://github.com/drazen-dee28/-Load-Balancing-Solution-for-a-Tooling-Website/blob/main/images/stats.jpg)


I verified the configuration file using the below command in Terminal:

```haproxy -c -f /etc/haproxy/haproxy.cfg```

![](https://github.com/drazen-dee28/-Load-Balancing-Solution-for-a-Tooling-Website/blob/main/images/verification.jpg)



I applied the configurations, restarted the HAproxy service: 
```sudo systemctl restart haproxy.service```


sudo systemctl disable apache2 &  sudo systemctl stop apache2  ,i then restarted the HAproxy service

```sudo systemctl restart haproxy.service```


Then i checked the status of HAproxy service

 ```sudo systemctl status haproxy.service```

 ![](status.png)