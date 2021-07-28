# Docker-Swarm-Shared-Storage-Volumes
### Description
We are going to build an nginx service on a docker swarm manager, however we want to have a custom nginx configuration. We will create a configuration file but we will store it externally in a storage server file, so that the containers from the service do not need to be recreated. The configuration file will be mounted to each service's replica container.

### Prerequisites
- You need a storage server
- You need an initialized Docker Swarm manager on another server
- You need atleast one worker node
- Install curl on both the swarm manager and worker node

### Installation
**1. Log into your storage server**
**2. Set up the external storage**
- Set up the storage directory
```
sudo mkdir -p /etc/docker/storage
sudo chown <user>:<user> /etc/docker/storage
```
- copy the nginx config file into the storage directory
```
cp /home/<user>/nginx.conf /etc/docker/storage/
```
**3. Install the ```vieux/sshfs``` plugin on both the swarm manager and the worker node**
```
docker plugin install --grant-all-permissions vieux/sshfs
```
**4. Create the nginx service that uses the shared volume**
- create the ```nginx-web``` service on the swarm manager, make sure to change <cloud_user password> with actual password, and <user> and <storage_IP>
```
docker service create -d \
   --replicas=3 \
   --name nginx-web \
   -p 8080:9773 \
   --mount volume-driver=vieux/sshfs,source=nginx-config-vol,target=/etc/nginx/,volume-opt=sshcmd=<user>@<storage_IP>:/etc/docker/storage,volume-opt=password="<cloud_user password>" nginx:latest
```

- Check that it is working properly
```curl localhost:8080```
