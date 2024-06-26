# Vagrant HaProxy

This repository hosts a Vagrant setup for HAProxy and Apache web servers. It enables easy deployment of a high availability proxy along with two Apache servers for load balancing. Detailed usage instructions are provided, facilitating setup, configuration changes, and management of the virtual environment.


| Host port | Guest machine | Guest port | Notes
------------|---------------|------------|---
| 8080 | haproxy | 8080 | HAProxy Admin Interface
| 8081 | haproxy | 80 | Load Balanced Apache
* It installs Apache on the two servers, configures it with a index page that identifies which host you're viewing the page on.
* It installs HAProxy on the separate host, and drops a configuration file in place with the two webservers pre-configured.  It doesn't require HAProxy to be the default gateway because it NAT's the source IP as well as the destination IP.



## Prerequisites

Before you begin, make sure you have the following installed on your machine:

- [Vagrant](https://www.vagrantup.com/)
- [VirtualBox](https://www.virtualbox.org/)
* Vagrant Reload Provisioner - open powershell console and run "vagrant plugin install vagrant-reload"

## How to use?

1. Clone https://github.com/Think-Cube/VagrantHaProxy.git
2. Open 3 terminal windows, one for each host
3. Terminal #1, run ``` vagrant up haproxy && vagrant ssh haproxy ```
4. Terminal #2, run ``` vagrant up web1 && vagrant ssh web1 ```
5. Terminal #3, run ``` vagrant up web2 && vagrant ssh web2 ```
6.  Open up [http://localhost:8080/haproxy?stats](http://localhost:8080/haproxy?stats) in your host's browser.  This is the HAProxy admin interface.
7.  Open up [http://localhost:8081/](http://localhost:8081/) in your host's browser.  This is the load balanced interface to the two web servers.  **Note** this is port forwarded via your actual host, and will be accessible via your externally accessible IP address - you can access test the load balancer from another workstation if you wish.
8.  Open up [http://172.55.33.11/](http://172.55.33.11/) in a browser to see if web1's Apache is working.
9.  Open up [http://172.55.33.12/](http://172.55.33.12/) in a browser to see if web2's Apache is working.
10.  To see the Apache access logs on web1 and web2, run ``` sudo tail -f /var/log/apache2/access.log ```  If you'd like to filter out the "pings" from the load balancer, run ``` sudo tail -f /var/log/apache2/access.log | grep -v OPTIONS ```
11.  To stop Apache on one of the webservers to simulate an outage, run ``` sudo service apache2 stop ```  To start it again, run ``` sudo service apache2 start ```
12.  To make changes to haproxy, edit the config file with ``` sudo nano /etc/haproxy/haproxy.cfg ```  When you want to apply the changes, run ``` sudo service haproxy reload ```  If you break things and want to reset back, just run ``` sudo cp /etc/haproxy/haproxy.cfg.orig /etc/haproxy/haproxy.cfg && sudo service haproxy reload ```
13.  When you're all done, type ``` exit ``` at the shell to get back to your local terminal.
14.  To shut down the VM's, run ``` vagrant halt web1 web2 haproxy ```
15.  To remove the VM's from your hard drive, run ``` vagrant destroy web1 web2 haproxy ```
16.  If you wish to remove the cached image file from which these machines were created, run ``` vagrant box remove precise32 ```

## Vagrant file includes

* Ha-Proxy
* 3x Ubuntu server
