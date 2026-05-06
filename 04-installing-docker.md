# Ubuntu/Debian Docker and Docker Compose Installation - Easy Way
Docker provides a convenience script that makes Docker and Docker compose installation on various operating systems a breeze. This is perfect for quick setups:
```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```
# Verify that Docker is Running on Ubuntu

There are many ways to check if Docker is running on Ubuntu. One way is to use the following command:
```bash
sudo systemctl status docker
```
You should see an output that says active for status.

![](assets/list-of-active-docker-containers.jpg)

## **Step 1**: Check if Docker Compose Plugin is Installed

If you've installed Docker CE using the steps above or using the convenience script, Docker Compose should already be installed as a plugin. You can verify this with:
```bash
sudo docker compose version
```
Notice there's no hyphen between "docker" and "compose" - this is the new Docker Compose V2 plugin format.

![](assets/docker-compose-version.png)

## **Step 2**: Installing Docker Compose Manually (If Needed)

If for some reason the Docker Compose plugin isn't included with your Docker installation, you can install it manually:
```bash
sudo apt update
sudo apt install docker-compose-plugin
```
This automatically gets the latest version and installs it.

# Tips to Enhance Docker Experience
## 1. Setup Bash Aliases to Simplify Commands

If you have no clue what I am talking about, then I suggest you to read our Bash Aliases guide for a general idea.

With bash aliases set, you can run several docker and docker compose commands using shortcuts (instead of the full commands listed above). For example, just dps (or whatever you set) instead of sudo docker ps -a.

## 2. Add User to Docker Group
```bash
adduser lushman docker
```
## 3. Minimum Docker API Version

With Docker v29 and above, Docker deprecated older API versions. While some apps like Traefik have updated to use the newer Docker API, many homelab apps still use the older APIs (e.g. Socket Proxy, Dozzle, Portainer, etc.).

> :warning: **Warning:** If you see an error like "client version 1.24 is too old. Minimum supported API version is 1.44" in your container logs, you will need to set the minimum Docker API version to 1.24.

Until those apps update their Docker images to use the newer API versions, you can set the minimum API version to 1.24. In short:
```bash
sudo systemctl edit docker
```
Add [Service] and the next line Environment=DOCKER_MIN_API_VERSION=1.24, then restart Docker with sudo systemctl restart docker.
