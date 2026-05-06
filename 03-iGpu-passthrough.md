# Benefits of GPU Passthrough on Proxmox LXC

Let me break down why you might want to set this up:
## 1. Hardware-Accelerated Transcoding

If you're running Plex, Jellyfin, or Emby, this is a game-changer! Instead of crushing your CPU with software transcoding, your GPU can handle 4K and HDR content effortlessly. In my setup with the Intel i7-13800H's QuickSync, the difference is night and day!
## 2. Resource Efficiency
Your CPU stays free for other tasks while the GPU handles the heavy lifting of transcoding. This means you can run other CPU-intensive applications without affecting your media streaming performance.

## 3. Multiple Container Support

One of my favorite features is that you can share the GPU across several containers. This is perfect if you're running both Plex and Jellyfin, or want to experiment with different media server setups.

# Step-by-Step Proxmox GPU Passthrough to LXC Container

Let me walk you through the process I use in my homelab.
## **Step 1**: Identify Your GPU Device

First, let's find your GPU device node:
```bash
cd /dev/
ls
```
![](assets/proxmox-dri-folder.jpg)

You should see a folder called dri. Let's check what's inside:
```bash
cd dri
ls
```
You'll typically see renderD128 or something similar.

![](assets/render128-graphics-node-proxmox.jpg)

If you have multiple GPUs, you might see additional entries (e.g. render129).
## **Step 2**: Configure LXC Container

Now comes the important part. We need to edit your container's configuration file:
```bash
cd /etc/pve/lxc
sudo nano 800.conf  # Replace 800 with your container ID
```
Here is the base container config we will edit (yours may look slightly different - don't worry).

![](assets/udms-lxc-container-configuration-on-proxmox.jpg
)

Add these lines to the above configuration:
```bash
lxc.cgroup2.devices.allow: c 226:0 rwm
lxc.cgroup2.devices.allow: c 226:128 rwm
lxc.cgroup2.devices.allow: c 29:0 rwm
lxc.mount.entry: /dev/dri dev/dri none bind,optional,create=dir
lxc.mount.entry: /dev/dri/card0 dev/dri/card0 none bind,optional,create=file
lxc.mount.entry: /dev/dri/renderD128 dev/dri/renderD128 none bind,optional,create=file
```
Once added, the LXC configuration may look like the one below.

![](assets/udms-lxc-container-conf-with-graphics-node.jpg)

If you have multiple  graphics cards you may have card1 in addition to card0. Likewise, you may have more than just renderD128. You can use the following commands to identify
## **Step 3**: Apply the Changes

Time to make these changes take effect:

1. Save the configuration file.
2. Reboot your LXC container:
  
```bash
sudo reboot
```
## **Step 4**: Verify the Setup

After the container reboots, let's verify everything is working by running the following commands inside the LXC container:
```bash
cd /dev/
ls
cd dri
ls -al
```
The device is owned by nobody:nogroup.

![](assets/graphics-node-permissions-inside-proxmox-lxc.jpg)

But the permissions are 666 (indicated by rw-rw-rw) so it should be OK as anyone can read and write to the graphics node.
## **Step 5**: Proper Permissions for Graphics Node

If for whatever reason, you do not have rw-rw-rw then hardware transcoding will fail and you will see a permission denied error in Plex logs. If this happens you will have to change the permissions on Proxmox host.

So, on your Proxmox host, set the proper ownership using the following commands:
```bash
sudo chmod -R 666 /dev/dri/card0
sudo chmod -R 666 /dev/dri/renderD128
```
Finally, check the permissions again:
```bash
ls -al
```
Now you should see that the graphics node is owned by root and the permissions are 666.

![](assets/graphics-node-permission-666-on-proxmox-host.jpg)

# Testing Your GPU Passthrough on Proxmox LXC

Now it is time to test if hardware transcoding works. If you have Plex or Jellyfin working with hardware transcoding you may use them. If not, take a look at the resources below:

# Troubleshooting Tips

In my experience running this setup, here are some common issues you might encounter:Linux & Unix
- Permission Issues: Ensure the container has proper access rights - check that rw-rw-rw (666) permissions are set on the graphics node
- Different Device Numbers: Your GPU might use different device numbers (e.g., renderD129 instead of renderD128) - adjust your LXC configuration accordingly
- Container Won't Start: Double-check your LXC configuration syntax - a missing character can prevent the container from booting
