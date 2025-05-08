- **Desde VM-A**:
sudo ip addr add 192.168.100.2/24 dev enp0s8
sudo ip link set enp0s8 up

- **Desde VM-B**:
sudo ip addr add 192.168.100.3/24 dev enp0s8
sudo ip link set enp0s8 up

- **Desde VM-A**:

  ping -c 4 192.168.100.3

- **Desde VM-B**:

  ping -c 4 192.168.100.2






- **VM-A con Netplan**


sudo tee /etc/netplan/01-intnet.yaml << 'EOF'
network:
  version: 2
  ethernets:
    enp0s8:
      dhcp4: no
      addresses:
        - 192.168.100.2/24
EOF

sudo netplan apply


ip addr show enp0s8


- **4.2 VM-B con NetworkManager**


sudo nmcli connection add \
  type ethernet \
  ifname enp0s8 \
  con-name intnet \
  autoconnect yes \
  ip4 192.168.100.3/24

sudo nmcli connection up intnet


nmcli connection show intnet
ip addr show enp0s8


