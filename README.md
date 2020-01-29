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
sudo curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose
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
