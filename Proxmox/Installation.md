# Download & Install

- [Download](https://www.proxmox.com/en/downloads) the official ISO image. Click on Proxmox VE 9.0 ISO installer

- Ensure the checksum is correct
  On Windows: `CertUtil -hashfile [filename] [algorithm]`

- Prepare an empty USB driver and [download Rufus](https://rufus.ie/en/#download). Make sure USB driver is empty, since everything written on it will be deleted. Follow rufus.exe instructions to create bootable USB drive.

- Boot from USB using Settings in Windows 11. Go to Settings -> System -> Recovery -> Advanced startup (in recovery options). When PC reboot choose Use a device option and select your USB bootable drive. After installation process, unplug USB driver when prompted. Make sure nothing valuable is on the computer since everything will be erased during installation process!

- When the Proxmox VE menu appears, choose graphical install and accept the EULA.

- Choose the target hard disk where you want to install Proxmox. Since I installed it directly in my old PC, there was only one option.

- Choose country, timezone and keyboard layout.

- Now you will have to create root password and provide email, where you will be receiving notifications from Proxmox.

- In the final step you will be setting up the network configuration. Choose management interface, hostname (FQDN), IP address (CIDR), the default gateway and a DNS server.

- The installer summarize the selected options. Confirm everything is correct and press install. Remove the USB drive and reboot the system.

- Once the system reboots, the Proxmox GRUB menu loads. Select Proxmox Virtual Environment GNU/Linux and press Enter.

- Next, the Proxmox VE welcome message appears. It includes an IP address that loads Proxmox. Navigate to that IP address in a web browser of your choice.

- After navigating to the required IP address, you may see a warning message that the page is unsafe because Proxmox VE uses self-signed SSL certificates. Click the IP link to proceed to the Proxmox web management interface.

- To access the interface, log in as root and provide the password you set when installing Proxmox. Make sure to store credentials for later use.

# Package repositories

Proxmox VE uses APT as its package management tool like any other Debian-based system. Repositories are a collection of software packages, they can be used to install new software, but are also important to get new updates. You need valid Debian and Proxmox repositories to get the latest security updates, bug fixes and new features.

Proxmox VE provides three different package repositories in addition to requiring the [base Debian repositories](https://pve.proxmox.com/wiki/Package_Repositories#_debian_base_repositories). This are Proxmox VE Enterprise, Proxmox VE No-subscription and Proxmox VE Test. Since we don't have an enterprise license for our home lab, we will be using [Proxmox VE No-subscription repository](https://pve.proxmox.com/wiki/Package_Repositories#sysadmin_no_subscription_repo). It is free and provide all the necessary functionality, but you don't get official support.

Additionally we will be adding official [Ceph repositories for Proxmox VE](https://pve.proxmox.com/wiki/Package_Repositories#sysadmin_package_repositories_ceph). These also comes with Enterprise, No-subscription and Test versions and we will again be using No-subscription repository. [Ceph](https://ceph.io/en/) is an open-source, distributed storage system.

By default Proxmox comes with enterprise repository, so you might want to delete /etc/apt/sources.list.d/pve-enterprise.sources file or comment out it's content. That way you won't be receiving fetching errors or getting notifications about invalid subscription.

The following configuration files are valid for Proxmox VE 9.0 and Debian 13 Trixie release.

## Debian Base Repositories

File /etc/apt/sources.list.d/proxmox.sources

```
Types: deb deb-src
URIs: http://deb.debian.org/debian/
Suites: trixie trixie-updates
Components: main non-free-firmware
Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg

Types: deb deb-src
URIs: http://security.debian.org/debian-security/
Suites: trixie-security
Components: main non-free-firmware
Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg
```

## Proxmox VE No-Subscription Repository

File /etc/apt/sources.list.d/proxmox.sources

```
Types: deb
URIs: http://download.proxmox.com/debian/pve
Suites: trixie
Components: pve-no-subscription
Signed-By: /usr/share/keyrings/proxmox-archive-keyring.gpg
```

## Ceph no-subscription repositories for Proxmox VE 9

File /etc/apt/sources.list.d/ceph.sources

```
Types: deb
URIs: http://download.proxmox.com/debian/ceph-squid
Suites: trixie
Components: no-subscription
Signed-By: /usr/share/keyrings/proxmox-archive-keyring.gpg
```

# Additional useful configuration

## Preventing Proxmox from sleeping when closing the PC lid

Change the file /etc/systemd/logind.conf

Uncomment the line **HandleLidSwitch=suspend** and change it to **HandleLidSwitch=ignore**.

systemclt restart systemd-logind

## Disable the "No valid subscription" pop-up

Open the Javascript file /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js with the editor of your choice.

Search for: 'No valid subscription.

change `res.data.status.toLowerCase() !== 'active'` line (few lines up) to `res.data.status.toLowerCase() == 'active'`.