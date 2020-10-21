# Basic Commands
**Show networks :** docker network ls 

**Inspect a network :** docker network inspect

**create a network :** docker network create —driver

**Attach a network to container :** docker network connect

**Detach a network from container :** docker network disconnect

### By default there are 3 network created :
- **bridge :**  connects to system network with out security, by default all containers are connected to this network

- **host :** connects to system network with out security

- **none :**