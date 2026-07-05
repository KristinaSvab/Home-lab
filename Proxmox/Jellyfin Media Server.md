# Installing Jellyfin Media Server

Jellyfin is a free and open-source media server and suite of multimedia applications designed to organize, manage, and share digital media files to networked devices.

The most straightforward method is to use the Proxmox VE Helper Scripts, which automate the installation process into an LXC. Using the [community-maintained helper scripts](https://community-scripts.org/scripts/jellyfin?id=jellyfin) is the standard way to deploy Jellyfin on Proxmox. It handles the complex setup, networking, and initial configurations automatically.

INSTALLATION PROCESS NOTES: default, enable GPU acceleration and installation of GPU drivers

Once finished, the script will output the IP address of your new Jellyfin container. Navigate to `http://<YOUR-IP-ADDRESS>:8096` in your web browser to finish the setup. Configure hostname, administrator username and password, language preferences and network accessibility.
