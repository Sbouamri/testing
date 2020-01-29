# testing
Build on Jenkins test
```
sudo apt -y install openssh-server

sudo apt -y install git
```


```
sudo nano /etc/network/interfaces

# Network interface VM1
auto ens33
iface ens33 inet static
 address 192.168.75.11
 netmask 255.255.255.0
 network 192.168.75.0
 gateway 192.168.75.2
 dns-nameservers 8.8.8.8

```
