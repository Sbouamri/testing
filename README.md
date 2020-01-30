# testing
Build on Jenkins test
```
sudo apt -y install openssh-server

sudo apt -y install git
```

```
sudo apt install curl
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo apt-get install software-properties-common
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io

#Aggiungere l’user al docker group:
sudo usermod -aG docker [user]

sudo systemctl restart docker

#Installare docker-compose:
sudo curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" \
-o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose
```
```
sudo fdisk -l

sudo mkfs.ext4 /dev/sdb

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

```
sudo reboot
ifconfig 		
#per controllare di aver settato corretto
ping 192.168.75.11

```
```
sudo nano /etc/hostname
vm1 			# Settare il nome della macchina

sudo nano /etc/hosts
127.0.1.1 vm1 	

#VMware 
192.168.75.11 vm1
192.168.75.12 vm2
192.168.75.13 vm3

sudo reboot

#per verificare che funziona
ping vm2

```
```
sudo nano /etc/network/interfaces

# Network Gluster
auto ens38
iface ens38 inet static
 address 10.0.0.11
 netmask 255.255.255.0
 network 10.0.0.0
 gateway 10.0.0.2


sudo reboot
ifconfig 		#per controllare di aver settato corretto


```
```
sudo nano /etc/hosts

#VMware GlusterFS
10.0.0.11 vm1g
10.0.0.12 vm2g
10.0.0.13 vm3g

sudo reboot

#per verificare che funziona
ping vm2g


```
```
#VMware  VM
192.168.75.11 vm1
192.168.75.12 vm2
192.168.75.13 vm3

#VMware GlusterFS
10.0.0.11 vm1g
10.0.0.12 vm2g
10.0.0.13 vm3g

```
```
sudo apt update; 
sudo apt -y install glusterfs-server glusterfs-client


sudo service glusterfs-server start


sudo gluster peer probe vm2g
sudo gluster peer probe vm3g

(peer probe: success)


sudo gluster peer status

```
```
sudo mkdir -p /gluster/bricks/1 		(su vm1)
sudo mkdir -p /gluster/bricks/2 		(su vm2)
sudo mkdir -p /gluster/bricks/3 		(su vm3)


sudo su 
echo '/dev/sdb /gluster/bricks/1 ext4 defaults 0 0' >> /etc/fstab 		(su vm1)
echo '/dev/sdb /gluster/bricks/2 ext4 defaults 0 0' >> /etc/fstab 		(su vm2)
echo '/dev/sdb /gluster/bricks/3 ext4 defaults 0 0' >> /etc/fstab 		(su vm3)

 
sudo mount -a	 (su vm1, vm2, vm3)


```
```
sudo gluster volume create gfs replica 3 \
vm1g:/gluster/bricks/1/brick \
vm2g:/gluster/bricks/2/brick \
vm3g:/gluster/bricks/3/brick


sudo gluster volume start gfs


sudo gluster volume info gfs
sudo gluster volume status gfs


sudo gluster volume set gfs auth.allow 10.0.0.11, 10.0.0.12,10.0.0.13

sudo gluster volume set gfs nfs.disable Off

```
```
sudo mkdir /gfs
sudo su
echo 'localhost:/gfs /gfs glusterfs defaults,_netdev,backupvolfile-server=localhost 0 0' >> /etc/fstab
sudo mount -a


sudo su
chown -R anna:anna /gfs/


df -h

```
```
echo “Hello world” > /gfs/hello.txt


cat /gfs/hello.txt

```
```
mkdir /gfs/joomla
mkdir /gfs/phpmyadmin
mkdir /gfs/mysql
mkdir /gfs/grafana
chmod -R a=rwx /gfs/grafana/

```
```
docker swarm init --advertise-addr ens33


docker swarm join-token manager


docker swarm join-token worker


docker node promote <node name>
docker node demote <node name>


docker node ls
docker node ls -f "role=worker"

```
```
git clone https://github.com/Sbouamri/VCC-Project

```
```
cd joomla
docker build . -t joomla_img
cd ..
cd phpmyadmin
docker build . -t pmy_img
cd ..


nano docker-compose.yml

```
```
docker stack deploy joomlastack -c docker-compose.yml

docker stack ps joomlastack

docker stack services joomlastack

docker stack ps joomlastack --no-trunc

docker service logs <service name>

docker stack rm joomlastack

```
```
docker node update --availability Drain vm2


docker node update --availability Active vm2


docker service scale joomlastack_joomla=2 

```
```
./prometheus/promtool check config monitoring/prometheus/prometheus.yml 

./prometheus/promtool  check rules monitoring/prometheus/alert.rules

```
```
#VMware virtualization and cloud exam
192.168.75.11 vm1
192.168.75.12 vm2
192.168.75.13 vm3

#VMware GlusterFS
10.0.0.11 vm1g
10.0.0.12 vm2g
10.0.0.13 vm3g

#Servizi VM
192.168.75.11 joomla.local
192.168.75.11 managedb.joomla.local
192.168.75.11 prometheus.joomla.local
192.168.75.11 monitor.joomla.local
#192.168.75.11 map.joomla.local

```
```
gluster volume heal <VOLNAME> info split-brain 

```
