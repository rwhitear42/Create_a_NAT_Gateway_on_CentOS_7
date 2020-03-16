# How To Configure a NAT Gateway on CentOS 7


sudo firewall-cmd --permanent --direct --passthrough ipv4 -t nat -I POSTROUTING -o ens192 -j MASQUERADE -s 10.237.97.128/27
sudo firewall-cmd --permanent --direct --passthrough ipv4 -t nat -I POSTROUTING -o ens192 -j MASQUERADE -s 10.237.99.0/27

sudo firewall-cmd --change-interface=ens192 --zone=external --permanent

sudo firewall-cmd --change-interface=ens224 --zone=internal --permanent
