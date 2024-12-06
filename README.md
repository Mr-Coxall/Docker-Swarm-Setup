# Docker Swarm Setup

- install debian server
  - all with same SSH keys!
- set static ip (nmtui)
- update && upgrade
- install docker
```BASH
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```
- install docker swarm
  - sudo docker swarm init --advertise-addr 192.168.1.181 (1st control node)
- get control node connection string
  - sudo docker swarm join-token manager
- connect all control nodes and worker nodes
- check
  - sudo docker node ls
- 
