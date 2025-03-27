---
title: Deploy Your Projects with Docker & Portainer
type: blog
date: 2025-01-05 13:06:02 +0530
prev: blog/devops/
sidebar:
  open: true
---

In this guide, we'll walk through setting up a robust Docker Swarm environment with **Portainer** for container management and **Nginx Proxy Manager** for reverse proxy and SSL management. This powerful duo will help streamline your container management while enhancing security and accessibility.

## Step 1: Install Docker Engine

Let's start by installing the latest Docker Engine. Add Docker’s official GPG key and repository:

<!--more-->

```bash
# Install required packages
sudo apt-get update
sudo apt-get install ca-certificates curl

# Add Docker's GPG key
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add Docker repository
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

# Install Docker
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Test the installation
sudo docker run hello-world

# Add user to Docker group
sudo usermod -aG docker $USER
```

Reboot or log out and log back in to apply the group changes.

## Step 2: Set Up Docker Swarm & Networks

Now, let’s create a Docker Swarm and set up dedicated networks for your services:

```bash
docker swarm init

# Create backend and frontend networks
docker network create --attachable --driver overlay --internal --scope swarm backend
docker network create --attachable --driver overlay --scope swarm frontend
```

These networks will keep services isolated and enable communication between containers in the Swarm.

## Step 3: Deploy Portainer for Easy Management

Next up: **Portainer**! This web UI tool makes managing your Docker containers a breeze.

```bash
# Create Portainer directory and stack file
mkdir -p /opt/portainer
touch /opt/portainer/portainer-agent-stack.yml

# Deploy Portainer stack
docker stack deploy -c /opt/portainer/portainer-agent-stack.yml portainer

# Check running services
docker service ls

# View logs for Portainer
docker service logs portainer_portainer
```

Once Portainer is up and running, you can access it at:

```bash
https://192.168.0.1:9443/#!/init/admin
```

Complete the admin setup and start managing your containers!

## Step 4: Deploy Nginx Proxy Manager

For reverse proxying and SSL management, let’s set up **Nginx Proxy Manager**.

```bash
# Create directories for Nginx Proxy Manager
mkdir -p /opt/nginx-proxy-manager/letsencrypt
mkdir -p /opt/nginx-proxy-manager/data
touch /opt/nginx-proxy-manager/nginx-proxy-manager.yml

# Deploy the Nginx stack
docker stack deploy -c /opt/nginx-proxy-manager/nginx-proxy-manager.yml nginx

# Check the logs
docker service logs nginx_app
```

You can access the Nginx Proxy Manager UI at:

```bash
https://192.168.0.1:81
```

Follow the setup prompts to complete the installation.

## Step 5: Configure Reverse Proxy & SSL

Add reverse proxy rules to **Nginx Proxy Manager** for secure routing:

1. **Portainer**: Point to https://portainer_portainer:9443.
2. **Nginx**: Point to http://nginx_app:81.

This will route external traffic securely to your services, with SSL enabled!

## Step 6: Remove Exposed Ports for Security

To increase security, remove any exposed ports after setting up the reverse proxy:

```bash
docker service update --publish-rm published=9000,target=9000 portainer_portainer
docker service update --publish-rm published=9443,target=9443 portainer_portainer
docker service update --publish-rm published=8000,target=8000 portainer_portainer

docker service update --publish-rm published=81,target=81 nginx_app
```

Ensure your firewall (`ufw`) is updated accordingly.

## Portainer Stack File

Here’s the full Portainer stack configuration:

```yaml
version: '3.2'

services:
  agent:
    image: portainer/agent:2.21.2
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - /var/lib/docker/volumes:/var/lib/docker/volumes
    networks:
      - backend
    deploy:
      mode: global
      placement:
        constraints: [node.platform.os == linux]

  portainer:
    image: portainer/portainer-ce:2.21.2
    command: -H tcp://tasks.agent:9001 --tlsskipverify
    ports:
      - "9443:9443"
      - "9000:9000"
      - "8000:8000"
    volumes:
      - portainer_data:/data
    networks:
      - backend
      - frontend
    deploy:
      mode: replicated
      replicas: 1
      placement:
        constraints: [node.role == manager]

networks:
  backend:
    external: true
  frontend:
    external: true

volumes:
  portainer_data:
```

## Nginx Proxy Manager Stack File

Here’s the Nginx Proxy Manager stack configuration:

```bash
version: '3.8'

services:
  app:
    image: 'jc21/nginx-proxy-manager:latest'
    ports:
      - '80:80' # Public HTTP Port
      - '443:443' # Public HTTPS Port
      - '81:81' # Admin Web Port
    networks:
      - frontend
    volumes:
      - certificates:/etc/letsencrypt
      - npm-data:/data
    deploy:
      placement:
        constraints:
          - node.role == manager
      replicas: 1

networks:
  frontend:
    external: true

volumes:
  certificates:
    driver: local
    driver_opts:
      o: bind
      type: none
      device: /opt/nginx-proxy-manager/letsencrypt
  npm-data:
    driver: local
    driver_opts:
      o: bind
      type: none
      device: /opt/nginx-proxy-manager/data
```

## Conclusion

With **Docker Swarm**, **Portainer**, and **Nginx Proxy Manager**, you now have a powerful, scalable, and secure solution for managing containers and routing traffic. These tools simplify the process of handling complex infrastructures and ensure that your services are always accessible and protected.

Start managing your containers like a pro—happy deploying! 🚀
