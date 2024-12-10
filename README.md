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

## Drain Control Nodes
- drain all control nodes, so only the worker nodes will get containers and control nodes only do control tasks
```BASH
sudo docker node update --availability drain control-01
sudo docker node update --availability drain control-02
sudo docker node update --availability drain control-03
```

## Install keepalived on all Control Nodes
- on each control node, install keepalived
```BASH
sudo apt update
sudo apt install -y libipset13 keepalived
```
- get the interface for each of your control nodes
```BASH
ip a
```
- on each control node create a config file
```BASH
sudo nano /etc/keepalived/keepalived.conf
```
- place something like the following for the control-01 node:
```BASH
vrrp_instance VIP_1 {
  state MASTER
  interface ens18
  virtual_router_id 55
  priority 150
  advert_int 1
  unicast_src_ip xxx.xxx.xxx.xxx
  unicast_peer {
    yyy.yyy.yyy.yyy
    zzz.zzz.zzz.zzz
  }

  authentication {
    auth_type PASS
    auth_pass C3P9K9gc
  }

  virtual_ipaddress {
    vvv.vvv.vvv.vvv/24
  }
}
```
- place something like the following for the control-2&3 node:
```BASH
vrrp_instance VIP_1 {
  state BACKUP
  interface ens18
  virtual_router_id 55
  priority 150
  advert_int 1
  unicast_src_ip yyy.yyy.yyy.yyy
  unicast_peer {
    xxx.xxx.xxx.xxx
    zzz.zzz.zzz.zzz
  }

  authentication {
    auth_type PASS
    auth_pass C3P9K9gc
  }

  virtual_ipaddress {
    vvv.vvv.vvv.vvv/24
  }
}
```
- start and enable the service
```BASH
sudo systemctl stop keepalived.service
```
- get the status
```BASH
sudo systemctl status keepalived.service
```

