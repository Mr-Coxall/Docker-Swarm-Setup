# Code to add to Control Nodes

https://community.cloudflare.com/t/increase-received-buffer-size-for-tunnel-on-docker/563964/1
sudo sysctl -w net.core.rmem_max=3000000
sudo sysctl -w net.core.rmem_default=3000000
sudo sysctl -w net.core.wmem_max=3000000
sudo sysctl -w net.core.wmem_default=3000000

sudo sysctl -a | grep net.core
