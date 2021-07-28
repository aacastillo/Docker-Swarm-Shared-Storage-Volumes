# Docker-Swarm-Shared-Storage-Volumes
### Description
We are going to build an nginx service on a docker swarm manager, however we want to have a custom nginx configuration. We will create a configuration file but we will store it externally in a storage server file, so that the containers from the service do not need to be recreated. The configuration file will be mounted to each service's replica container.

### Prerequisites
- You need a storage server
- You need an initialized Docker Swarm manager

### Installation
1. Log into your storage server
2. Set up the external storage
- Set up the storage directory
```
sudo mkdir -p /etc/docker/storage
sudo chown cloud_user:cloud_user /etc/docker/storage
```
- copy the nginx config file into the storage directory
```
cp /home/cloud_user/nginx.conf /etc/docker/storage/
```
