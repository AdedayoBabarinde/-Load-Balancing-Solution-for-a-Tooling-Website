
**Network Information**

- Client Web Server1 192.168.1.223
- Client Web Server2 192.168.1.122
- Client Web Server3 192.168.1.249
- NFS Server 192.168.1.131
- Load Balancer 192.168.1.139
- MySQL Server 192.168.1.132



** Local DNS Setup for the Load Balancer Using /etc/hosts File**


Setting static IP on the load balancer


- Identify the network interface

```ip addr``


![](https://github.com/drazen-dee28/-Load-Balancing-Solution-for-a-Tooling-Website/blob/main/images/interface.jpg)


Edit /etc/sysconfig/network config file by changing the BOOTPROTO option to ```none``` and configuring the IP, route prefix, gateway and dns server statically as shown below

```sudo nano /etc/sysconfig/network-scripts/ifcfg-enp0s8```


![](https://github.com/drazen-dee28/-Load-Balancing-Solution-for-a-Tooling-Website/blob/main/images/config.jpg)

Save changes to network configurations.
```sudo nmcli connection down enp0s8 && sudo nmcli connection up enp0s8```


![](https://github.com/drazen-dee28/-Load-Balancing-Solution-for-a-Tooling-Website/blob/main/images/save.jpg)


Verify the IP has changed
```ip addr```


![](https://github.com/drazen-dee28/-Load-Balancing-Solution-for-a-Tooling-Website/blob/main/images/ipchange.jpg)

Edit the hosts file on the load balancer as shown below:

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




**Setting up HAproxy as a load balancer**


- Add the following directories and the statistics file for HAProxy records.

```sudo mkdir -p /etc/haproxy```
```sudo mkdir -p /var/lib/haproxy```
```sudo touch /var/lib/haproxy/stats```


- Create a symbolic link to allow running of HAProxy commands as a normal user.

```sudo ln -s /usr/local/sbin/haproxy /usr/sbin/haproxy```

- I copied the haproxy.init file from the examples to /etc/init.d directory to add the proxy as a service to the system

```sudo cp ~/haproxy-2.0.7/examples/haproxy.init /etc/init.d/haproxy```


- Change the file permissions to make the script executable and then reload the systemd daemon

```sudo chmod 755 /etc/init.d/haproxy```
```sudo systemctl daemon-reload```

- Enable the HAPROXY service to allow it to restart automatically at system boot-up

```sudo chkconfig haproxy on```


- Add a new user for HAProxy 
```sudo useradd -r haproxy```

 - Allow the required services and reload the firewall.

![](reload.jpg)






**Configuring the load balancer**







I edited the /etc/haproxy/haproxy.cfg file by appending the following lines in the haproxy.cfg file replacing the IP addresses as shown below:

![](https://github.com/drazen-dee28/-Load-Balancing-Solution-for-a-Tooling-Website/blob/main/images/cfg.jpg)


- Save the file and restart HAProxy 

```sudo systemctl restart haproxy```



- Test the setup

http://192.168.1.139/haproxy?stats



![](https://github.com/drazen-dee28/-Load-Balancing-Solution-for-a-Tooling-Website/blob/main/images/testing.jpg)



**Web Server Configurations**

Configure static IP on the 3 Webservers as shown below:

webserver1-adedayo.lan   ----Centos 8
![](https://github.com/drazen-dee28/-Load-Balancing-Solution-for-a-Tooling-Website/blob/main/images/staticw1.jpg)

webserver2-adedayo.lan   ----Ubuntu 20
![](https://github.com/drazen-dee28/-Load-Balancing-Solution-for-a-Tooling-Website/blob/main/images/staticw2.jpg)

webserver3-adedayo.lan   ----Ubuntu 20
![](https://github.com/drazen-dee28/-Load-Balancing-Solution-for-a-Tooling-Website/blob/main/images/staticw3.jpg)



- Edit the hosts file on the 3 webservers as follows :

![](https://github.com/drazen-dee28/-Load-Balancing-Solution-for-a-Tooling-Website/blob/main/images/hostw1.jpg)


![](https://github.com/drazen-dee28/-Load-Balancing-Solution-for-a-Tooling-Website/blob/main/images/hostw2.jpg)


![](https://github.com/drazen-dee28/-Load-Balancing-Solution-for-a-Tooling-Website/blob/main/images/hostw3.jpg)




**Configure Static IP and Host name for Database Server**

Change the dataserver hostname to ```dbserver-adedayo.lan```

Configure static IP on the database server by edit the yaml config file as shown below:
![](https://github.com/drazen-dee28/-Load-Balancing-Solution-for-a-Tooling-Website/blob/main/images/staticdb.jpg)







I checked the Apache access log file on the 3 webservers as follows:

```sudo tail –f /var/log/httpd/access_log```

![](https://github.com/drazen-dee28/-Load-Balancing-Solution-for-a-Tooling-Website/blob/main/images/accessw1.jpg)