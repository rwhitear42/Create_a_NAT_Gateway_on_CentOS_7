# How To Configure a NAT Gateway on CentOS 7

References:

* https://ronnybull.com/2015/11/20/how-to-centos-7-router/
* https://www.liquidweb.com/kb/how-to-start-and-enable-firewalld-on-centos-7/

### Enable and Start Firewalld Service
```
sudo systemctl enable firewalld
systemctl start firewalld
systemctl status firewalld
```

In the following example, interface **eth0** will be external/public. Interface **eth1** will be internal/private.

```
sudo vi /etc/hostname
```

```
sudo vi /etc/sysconfig/network-scripts/ifcfg-eth0 #Outside/public interface.

BOOTPROTO=static
ONBOOT=yes
IPADDR="<PUBLIC_IP>"
NETMASK="<PUBLIC_NETMASK>"
GATEWAY="<PUBLIC_DEFAULT_GATEWAY>"
DNS1="<PUBLIC_DNS>"
ZONE=external
```

```
sudo vi /etc/sysconfig/network-scripts/ifcfg-eth1 #Inside/private interface.

OOTPROTO=static
ONBOOT=yes
IPADDR="<PRIVATE_IP>"
NETMASK="<PRIVATE_NETMASK>"
DNS1="<PUBLIC_DNS>"
```

```
sudo systemctl restart network
```

Enable IP forwarding within the system.

```
sudo vi /etc/sysctl.d/ip_forward.conf

# File contents.
net.ipv4.ip_forward=1
```

```
sudo sysctl -p /etc/sysctl.d/ip_forward.conf
```

```
sudo firewall-cmd --permanent --direct --passthrough ipv4 -t nat -I POSTROUTING -o eth0 -j MASQUERADE -s <PRIVATE_NETID>/<NETMASK_CIDR_PREFIX>
sudo firewall-cmd --change-interface=eth0 --zone=external --permanent
sudo firewall-cmd --set-default-zone=internal
```

Reload the firewall service and then restart.

```
sudo firewall-cmd --complete-reload
sudo systemctl restart network && systemctl restart firewalld
```

```
sudo firewall-cmd --list-all
sudo firewall-cmd --list-all --zone=external
```

All done.
