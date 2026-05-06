# Proxmox LXC Network Device Passthrough Guide

Let me walk you through the exact process I do my Proxmox LXC network device passthrough. 

Don't worry if it seems complex at first - I'll break it down into manageable steps.

First, open your Proxmox web interface and connect to it using your preferred SSH client. 
You'll also want to SSH into your UDMS container (or whichever container you're working with).

## **Step 1**: Network Node Identification

On your Proxmox server, enter these commands:
```bash
cd /dev
ls
```
![Network node identification command](assets/mobaxterm-command.png)

Entering The “Cd /Dev/” And The “Ls” Command

![](assets/net-folder.png)

You'll spot a folder called "net" - that's our network device.

## **Step 2**: Proxmox LXC Configuration
Now, let's access the LXC configuration:
```bash
cd /etc/pve/lxc
ls
```
You'll see configuration files for all your containers. In my case, I'm using container 800 for my UDMS setup, so I'll edit "800.conf". 

![](assets/network-800-conf.png)

Use whatever number matches your container:
```bash
sudo nano 800.conf
```
After entering your password, add these two lines below the "unprivileged" line:
```
lxc.cgroup2.devices.allow: c 10:200 rwm
lxc.mount.entry: /dev/net dev/net none bind,create=dir
```
![](assets/code-for-lxc.png)


You will have to do [the exact same thing](https://tailscale.com/docs/features/containers/lxc/lxc-unprivileged), if you were to say run Tailscale natively on the LXC container instead of in a Docker inside LXC.

Reboot your container to apply the changes.

## **Step 3**: Verify Network Node Availability

Once your container is back online, verify the network device is available **inside your container**:
```bash
cd /dev
ls
cd net
ls -al
```
You should see an entry ending with "tun" **inside your container** - that's our network device node.

![](assets/network-node-tun.png)

But right now the device is owned by **nobody:nogroup**. That is the wrong permission. So, let's fix that.

## **Step 4**: Proper Permissions for Network Node

Back on your Proxmox host, set the proper ownership:
```bash
sudo chown 100000:100000 /dev/net/tun
ls -al
```
Finally, check the container again:
```bash
ls -al
```
![](assets/tun-owned-by-root.png)

You should now see the "tun" device owned by the root user.

# Security Best Practices
Let me share some security considerations I've learned from running this setup in my own homelab:

- Always run containers with unprivileged user IDs and limited capabilities
- Regularly scan your Docker images using tools like Docker scan
- Keep both Docker and Proxmox LXC updated with the latest security patches
- Use network namespaces or virtual networks to isolate containers
- Implement regular backups of your containers and data
- Only use trusted Docker image sources
- Avoid running containers as root whenever possible
- Implement proper encryption and access controls
