# Asus-router

This ia a guide for configuring wireless router. I am using an old Asus wireless router RT-AC88U, but the concepts should be similar everywhere.

# Factory reset

Since I didn't now the previous configuration of the router, the first think I did was factory reset. It is also a great learning experience to start completely form scratch. Usually routers have a dedicated button for that. You will need a clip or a similar object to press it. Hold until power LED starts flashing. Wait for a few minutes for a reboot and for everything to settle up successfully.

# Accessing the management website

In order to access the management website you need to be connected to the router either wirelessly ot ethernet cable. Go to http://router.asus.com or enter its IP address into search bar on your browser. If you receive connection refused error, make sure you are connecting over HTTP and not HTTPS.

The initial set up guide will appear. Go to advanced configuration -> wireless router operation mode (default) -> WAN internet connection choose Automatic IP (DHCP) -> No need to enter any credentials -> In wireless settings Set network name (SSID) and wireless security - Wifi password -> Set router management username and password.

# Ports and antennas

At the back of the router there are several ports:

- Electricity
- Power button
- WAN port -> for giving the router access to the world wide internet. You should use ethernet cable to connect it to one of the ethernet ports at your house.
- 8 LAN ports -> for connecting your home devices to its subnet.
- USB
- WPS button
- and the reset button, we have already used in the previous step.

It provides 4 antennas for radio signals (Wi-fi).

# Network Map

After successful login the **network map** will show up. Here you can easily observe wired and wireless connected clients along with information about them, such as IP addresses. Here you will also see any USB devices attached. On the right side you can observe **system status** with CPU and RAM usage and status of ethernet ports. Finally, a quick overview and settings for **wireless security** is also displayed.

# Wireless settings

Advanced settings -> Wireless

## General settings

Advanced settings -> Wireless -> General

This section provides overview of general information like network name, authentication method, WPA encryption type and password and channel bandwidth.

## Advanced settings

Advanced settings -> Wireless -> Professional
This section allows you to configure additional parameters for wireless.

## Disabling radio

I've used this section for turning off wireless, since at the beginning I only wanted to use this used router as a primary wired router for my home lab.

Go to Advanced settings -> Wireless -> Professional
In the first line chose bandwidth (2.4 or 5 GHz) and choose enable radio no one line bellow.
Repeat the process with the bandwidth, you didn't choose in previous step.

# LAN settings

Advanced settings -> LAN

## LAN IP

Advanced settings -> LAN -> LAN IP

Here is the information about router host name, domain name and it's LAN IP address and subnet mask.
It's IP address will be the gateway for connected devices, and is also the IP we are using for accessing this management site.

## DHCP Server

Advanced settings -> LAN -> DHCP Server

DHCP (Dynamic Host Configuration Protocol) is a protocol for the automatic configuration used on IP networks. The DHCP server can assign each client an IP address and informs the client of the of DNS server IP and default gateway IP. RT-AC88U supports up to 253 IP addresses for your local network.

### Basic config

- You can choose if you want to enable DHCP or not.
- Domain name that will be received by clients.
- IP Pool Starting Address and IP Pool Ending Address specify the range form which the connected devices will receive IP addresses.
- Lease time determines how long will the client own the automatic IP address received. It is the time that clients won't change IP in seconds.

You can also assign **DNS and WINS server** settings.

### Manually assigning IP addresses

In some cases it is useful for devices to have static IP address. For example, for convenience, if you have a service that you will be connecting to often or if you want to have an application being accessible form outside internet. You can manually assign IP around DHCP list (max 64 clients). Which means you should shorten your DHCP pool in order to have some room for static IPs. I have reserved 192.168.2.2 to 192.168.2.66 for static mapping and 192.168.2.67 to 192.168.2.254 for DHCP. Subnet mask is 255.255.255.0

In order to use this feature, you wll have to enable Manual Assignment. Go to Manually Assigned IP around the DHCP list (Max Limit : 64), enter Client Name (MAC address) and map it to the IP you want to give it. Click Add and then apply to save changes.

I've added a static IP for my [Proxmox VE host](../Proxmox/README.md).
