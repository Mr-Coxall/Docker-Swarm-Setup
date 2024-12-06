# Docker Swarm Setup

- install debian server
  - all with same SSH keys!
- set static ip
```BASH
sudo nmtui
```
- update && upgrade
```BASH
sudo apt update && sudo apt upgrade -y
```
- install docker
```BASH
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install -y ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
- install docker swarm ONLY on control-1 to start with!
```BASH
sudo docker swarm init --advertise-addr xxx.xxx.xxx.xxx
```
  - ensure you replace xxx.xxx.xxx.xxx with your control-01 ip aaddress
- get control node connection string
```BASH
sudo docker swarm join-token manager
```
- get worker node connection string
```BASH
sudo docker swarm join-token worker
```

## Connect all Control and Worker node
- connect all control nodes and worker nodes
- check
```BASH
sudo docker node ls
```

## Install keepalived on all Control Nodes
- on each control node, install keepalived
```BASH
sudo apt update
sudo apt install -y libipset13 keepalived
```
- 
