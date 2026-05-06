# Create the Ubuntu LXC Container
Next, to create the LXC container, click "Create CT" on Proxmox Web Interface, typically found in the right-top corner.
Under the "General" tab, set the container ID to anything you want (100-999; I am picking 100), Hostname (e.g., "udms" for Ultimate Docker Media Server), and a password for the "root" user. 
Ensure "Unprivileged container" and "Nesting" options are checked.
Then, under the "Template" tab, pick the storage location where you downloaded the Ubuntu template and the actual template file you downloaded, as shown in the image below.
The "Disks" section is highly customizable depending on your situation. I have my stuff stored in a NAS, so all I need is a disk to run the OS. Therefore, I am picking one disk of size 96 GB (customize it to your liking) in my ZFS mirrored storage. Enable ACLs. Typically, I also like to enable noatime (reduces disk i/o) and discard (since I am using an SSD), for mount options.
It is a good practice to assign a static IP address to a home server. But before you do that, ensure that the IP address is not already allocated to another device. You can find this information on your router page. Alternatively, you can choose DHCP now and come back to the LXC's "Network" page to find out what IP was assigned and convert that into a static assignment.

Pay attention to the format of the IP and gateway IP specifications. DNS will be configured later.
Then, check if everything looks OK. Leave "Start after created" disabled for now as we will need to make additional changes to the LXC configuration.
Finally, go to the LXC "Options" tab and enable "Start at boot" to make the LXC start when Proxmox starts. Edit the "Features" and enable keyctl and FUSE.

# Update the OS
Most fresh OS installations lack all the security and package updates. So, let's take care of that next.
Run the following commands to refresh the packages list and install any updates.
```bash
apt update
apt upgrade
```
# Secure SSH Access with Tailscale SSH
# Install Basic/Required Packages

At this point, you may SSH into your server and continue remotely if you prefer. Let's install some basic/required packages (based on my experience).
```bash	
apt install acl apache2-utils apt-transport-https argon2 ca-certificates curl gnupg htop libnss-resolve lsb-release nano ncdu net-tools netcat-traditional ntp pwgen software-properties-common ufw unzip zip
```
- acl: Utilities for managing Access Control Lists for files and directories.
- apache2-utils: Utility programs for the Apache HTTP Server.
- apt: Package management utility for Debian-based systems.
- apt-transport-https: APT transport for downloading packages over HTTPS.
- argon2: High-performance password hashing function.
- ca-certificates: Common CA certificates for SSL/TLS.
- curl: Command-line tool for transferring data with URLs.
- gnupg: GNU Privacy Guard, a tool for secure communication and data storage.
- htop: An interactive process viewer and system monitor.
- install: Command for APT to install packages.
- libnss-resolve: NSS module for systemd-resolved (DNS resolver).
- lsb-release: Utility to display Linux Standard Base and distribution-specific information.
- nano: A simple and user-friendly text editor.
- ncdu: A disk usage analyzer with an ncurses interface.
- net-tools: Obsolete but still common tools for network configuration.
- netcat-traditional: A simple Unix utility that reads and writes data across network connections.
- ntp: Network Time Protocol daemon and utilities for synchronizing system time.
- pwgen: A utility for generating pronounceable passwords.
- software-properties-common: Tools for managing your  software repositories.
- sudo: Executes a command as the superuser.
- ufw: Uncomplicated Firewall, a user-friendly frontend for iptables.
- unzip: An extraction utility for .zip archives.
- zip: A compression and file packaging utility.

# Perform Server Tweaks

A few final system configuration tweaks to enhance the performance and handling of large lists of files (e.g., Plex/Jellyfin metadata). Edit /etc/sysctl.conf using the following command:
```bash
nano /etc/sysctl.conf
```
Add the following 3 lines at the end of the file:
```
vm.swappiness=10
vm.vfs_cache_pressure = 50
fs.inotify.max_user_watches=262144
```
Save and exit by pressing Ctrl X, Y, and Enter. That is all the prep work I do before I start building my Docker server stack.

# Enable Firewall
Ubuntu/Debian systems come with a built-in firewall called UFW (Universal Firewall). It is disabled by default.
Let's start by adding some default policies (deny all incoming and allow all outgoing) and then allow incoming connections only from your local network (e.g., 192.168.100.0/24). Customize the network subnet based on your situation.
```bash
ufw default deny incoming
ufw default allow outgoing
ufw allow from 192.168.100.0/24
```
This will allow all connections from devices in the IP range 192.168.100.1 to 192.168.100.254. Customize the IP range to fit your situation.
Then, let's activate UFW using the command:
```bash
ufw enable
```
Finally, check the status of UFW using **ufw status**. You should see that it is active with the rule we added above:
```bash
ufw status
```
# Create a New User
Some operating systems allow the creation of a non-root user during installation, and some do not. If you did not create a primary user during setup, use the following commands to create it:
```bash
adduser lushman
adduser lushman sudo
```
With the first command, we are adding a new user called "lushman". With the second command, I am adding "lushman" to the "sudo" group, so I have the privilege to use the sudo command.
Reboot and login as the new user and you should be good to go.

# Final Thoughts on Prepping Ubuntu/Debian for Docker
**Setting up Ubuntu/Debian in an unprivileged LXC container** might seem complex at first, but it provides the perfect foundation for a secure and efficient Docker environment, all while using minimal resources. After years of running Docker services, I can confidently say this approach strikes the ideal balance between performance and security.

In my case, running Ubuntu in a Proxmox unprivileged LXC means I get near-bare-metal performance without sacrificing isolation. The nesting and keyctl options we enabled are crucial for Docker to function properly, while FUSE support opens the door for tools like Rclone when you need to mount cloud storage.
