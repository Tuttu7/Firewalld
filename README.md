#### Installing firewalld

```
[root@ip-172-31-59-247 ~]# yum info firewalld
Updating Subscription Management repositories.
Unable to read consumer identity

This system is not registered with an entitlement server. You can use subscription-manager to register.

Red Hat Enterprise Linux 9 for x86_64 - AppStream from RHUI (RP  20 MB/s |  16 MB     00:00    
Red Hat Enterprise Linux 9 for x86_64 - BaseOS from RHUI (RPMs)  16 MB/s | 7.8 MB     00:00    
Red Hat Enterprise Linux 9 Client Configuration                  16 kB/s | 2.2 kB     00:00    
Available Packages
Name         : firewalld
Version      : 1.1.1
Release      : 3.el9
Architecture : noarch
Size         : 516 k
Source       : firewalld-1.1.1-3.el9.src.rpm
Repository   : rhel-9-baseos-rhui-rpms
Summary      : A firewall daemon with D-Bus interface providing a dynamic firewall
URL          : http://www.firewalld.org
License      : GPLv2+
Description  : firewalld is a firewall service daemon that provides a dynamic customizable
             : firewall with a D-Bus interface.

[root@ip-172-31-59-247 ~]# yum install firewalld
```

#### View status of firewalld 

```
[root@ip-172-31-59-247 ~]# systemctl status firewalld
○ firewalld.service - firewalld - dynamic firewall daemon
     Loaded: loaded (/usr/lib/systemd/system/firewalld.service; enabled; vendor preset: enabled)
     Active: inactive (dead)
       Docs: man:firewalld(1)
[root@ip-172-31-59-247 ~]# systemctl start firewalld
[root@ip-172-31-59-247 ~]# systemctl status firewalld
● firewalld.service - firewalld - dynamic firewall daemon
     Loaded: loaded (/usr/lib/systemd/system/firewalld.service; enabled; vendor preset: enabled)
     Active: active (running) since Sun 2023-02-26 06:43:10 UTC; 2s ago
       Docs: man:firewalld(1)
       
 ```
 
 #### To display the services or ports currently open on the firewall for the public zone,
 
 ```
 [root@ip-172-31-59-247 ~]# firewall-cmd --list-all --zone=public
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: eth0
  sources: 
  services: cockpit dhcpv6-client ssh
  ports: 
  protocols: 
  forward: yes
  masquerade: no
  forward-ports: 
  source-ports: 
  icmp-blocks: 
  rich rules: 




[root@ip-172-31-59-247 ~]# firewall-cmd --list-all --zone=public
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: eth0
  sources: 
  services: cockpit dhcpv6-client ssh
  ports: 
  protocols: 
  forward: yes
  masquerade: no
  forward-ports: 
  source-ports: 
  icmp-blocks: 
  rich rules: 
[root@ip-172-31-59-247 ~]# firewall-cmd --list-services
cockpit dhcpv6-client ssh

[root@ip-172-31-59-247 ~]# firewall-cmd --list-ports 

```


#### Open port for services 

```

[root@ip-172-31-59-247 ~]# firewall-cmd --zone=public --permanent --add-service=http
success
[root@ip-172-31-59-247 ~]# firewall-cmd --zone=public --permanent --add-port=80/tcp
success

[root@ip-172-31-59-247 ~]# firewall-cmd --reload
success
[root@ip-172-31-59-247 ~]# firewall-cmd --zone=public --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: eth0
  sources: 
  services: cockpit dhcpv6-client http ssh
  ports: 80/tcp
  protocols: 
  forward: yes
  masquerade: no
  forward-ports: 
  source-ports: 
  icmp-blocks: 
  rich rules: 
[root@ip-172-31-59-247 ~]# firewall-cmd --list-ports
80/tcp



The --permanent option makes the firewall changes persist through reboots




 ```
 
 
 #### To close a port 
 
 ```
 [root@ip-172-31-59-247 ~]# firewall-cmd --zone=public --permanent --remove-service=http
success

[root@ip-172-31-59-247 ~]# firewall-cmd --zone=public --permanent --remove-port=80/tcp
success



# firewall-cmd --reload
success

```


 #### Adding Rich rule to access SSH service only from a specific IP address 
 
 ```
 [root@ip-172-31-59-247 ~]# firewall-cmd --permanent --zone=public --add-rich-rule='rule family="ipv4" source address="192.168.1.8" port protocol="tcp" port="22" accept'

[root@ip-172-31-59-247 ~]# firewall-cmd --reload
success
[root@ip-172-31-59-247 ~]# firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: eth0
  sources: 
  services: cockpit dhcpv6-client http ssh
  ports: 80/tcp
  protocols: 
  forward: yes
  masquerade: no
  forward-ports: 
  source-ports: 
  icmp-blocks: 
  rich rules: 
	rule family="ipv4" source address="192.168.1.8" port port="22" protocol="tcp" accept
  
  ```
 
 

