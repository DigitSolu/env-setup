Here is how to setup a docker swarm cluster with three nodes.
Ref:
- https://docs.docker.com/engine/install/ubuntu/
- https://docs.docker.com/engine/swarm/swarm-tutorial/create-swarm/

1. Install docker engine on all three instances, refer to docker document for steps (https://docs.docker.com/engine/install/ubuntu/)
2. Take one instance to be manager, on that instance, run:
```
docker swarm init --advertise-addr <MANAGER-IP>
```
3. (Optional) If need to make a more secure network between instances, remove the default overlay network and create one with encrypted option
```
# list network
docker network ls
# remove the one, name should be "ingress" and type "overlay"
docker network rm <NETWORK_ID>
# create a new overlay network
docker network create \
  --driver overlay \
  --ingress \
  --subnet=10.11.0.0/16 \
  --gateway=10.11.0.2 \
  --opt com.docker.network.driver.mtu=1200 \
  --opt encrypted encrypted_ingress
```
4. Get the command line for joinning the swarm
```
# docker will return the command to join the swarm
docker swarm join-token worker
```
5. On the other teo instances, run that command, the command will be look like this:
```
docker swarm join \
  --token **************************** \
  <MANAGER-IP>:<PORT>
```
6. On the manager instance, list out nodes and should see all three nodes
```
docker node ls
```

Next instance Portainer, a web interface to manage docker, refer to Portainer's document: https://docs.portainer.io/start/install-ce/server/swarm
