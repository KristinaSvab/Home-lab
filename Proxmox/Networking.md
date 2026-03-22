# Configuring management IP

Open the host console and use the editor of your choice (I am using nano for this) to edit files. Typing command `nano /etc/network/interfaces` will output something like this:

```
auto lo
iface lo inet loopback

iface enp2s0 inet manual

iface wlp4s0 inet manual

auto vmbr0
iface vmbr0 inet static
        address 192.168.1.11/24
        gateway 192.168.1.1
        bridge-ports enp2s0
        bridge-stp off
        bridge-fs 0
        bridge-vlan-aware yes
        bridge-vids 2-4049
```

lo is the loopback interface, brought up automatically. enp2s0 (ethernet port) and wlp4s0 (wireless interface) are set to manual (no IP), because they are part of a bridge. vmbr0 is a bridge with a static IP (192.168.1.11) and gateway (192.168.1.1). The bridge uses enp2s0 as its physical port. It is brought up automatically at boot.

We are going to change address and gateway:

```
auto lo
iface lo inet loopback

iface enp2s0 inet manual

iface wlp4s0 inet manual

auto vmbr0
iface vmbr0 inet static
        address 192.168.2.2/24
        gateway 192.168.2.1
        bridge-ports enp2s0
        bridge-stp off
        bridge-fs 0
        bridge-vlan-aware yes
        bridge-vids 2-4049
```

Note that these IP addresses are specific to my local subnet, so adjust as you wish.

In order for changes to take effect, we will need to restart the networking daemon: `systemctl restart networking`.

Second, we will also need to change /etc/hosts file. This file is a plain text file used in Unix-like operating systems to map hostnames to IP addresses, allowing the system to resolve names to addresses without needing a DNS server. It serves as a local name resolution method, where entries consist of an IP address followed by one or more hostnames.The beginning of the file should look something like this:

```
127.0.0.1 localhost.localdomain localhost
192.168.1.11 pve01.Home pve01
```

and we should change the second line like this:

```
127.0.0.1 localhost.localdomain localhost
192.168.2.2 pve01.Home pve01
```

Again note that your IP addresses and domain names are probably.

## Observing changes in GUI

Access the management GUI on https://192.168.2.2:8006. I've used the IP address chosen in the previous step. You should replace it with yours, or use a hostname if you've already set up the name resolving. You will receive a certificate warning, because Proxmox uses self signed certificate by default. Click advance and then continue to site anyway. We will later configure our own certificate to stop the error. Then you will be prompted to provide root credentials we have configured at the installation.

Click on your datacenter -> node/host -> System -> Network for observing network configuration and datacenter -> node/host -> System -> Hosts where you will see the same file we used earlier (/etc/hosts).
You can also observe certificate, DNS, Time settings and System logs here.