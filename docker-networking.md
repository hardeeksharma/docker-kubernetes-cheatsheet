
# Basic Docker Networking Commands

**Show networks :** docker network ls

  

**Inspect a network :** docker network inspect

  

**create a network :** docker network create —driver

  

**Attach a network to container :** docker network connect

  

**Detach a network from container :** docker network disconnect

  

### By default there are 3 network created :

- **bridge :** connects to system network with out security, by default all containers are connected to this network

  

- **host :** connects to system network with out security. Skips virtual networking of docker.

  

- **none :** gives no internet access.

  

---

  

#### Creating a new network
> `docker network create my_network` This will create a network with default bridge driver configuration. This can be verified  by command `docker network inspect my_network` , the driver value in json is bridge and the IPAM Config are identical to bridge network.
```
{
        "Name": "my_network",
        "Id": "f20c2457b4b01059649e25a3045168c49d43e92320100a5adb09df698f74c104",
        "Created": "2020-10-21T18:20:58.717297839Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.18.0.0/16",
                    "Gateway": "172.18.0.1"
                }
            ]
        }
```
> To see advanced options which we have can use while creating a network run command `docker network create --help`
```
Create a network

Options:
      --attachable           Enable manual container attachment
      --aux-address map      Auxiliary IPv4 or IPv6 addresses used by
                             Network driver (default map[])
      --config-from string   The network from which copying the configuration
      --config-only          Create a configuration only network
  -d, --driver string        Driver to manage the Network (default "bridge")
      --gateway strings      IPv4 or IPv6 Gateway for the master subnet
      --ingress              Create swarm routing-mesh network
      --internal             Restrict external access to the network
      --ip-range strings     Allocate container ip from a sub-range
      --ipam-driver string   IP Address Management Driver (default "default")
      --ipam-opt map         Set IPAM driver specific options (default map[])
      --ipv6                 Enable IPv6 networking
      --label list           Set metadata on a network
  -o, --opt map              Set driver specific options (default map[])
      --scope string         Control the network's scope
      --subnet strings       Subnet in CIDR format that represents a
                             network segment
```