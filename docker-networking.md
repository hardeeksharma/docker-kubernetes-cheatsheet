
  

# Basic Docker Networking Commands

**Show networks :** docker network ls

**Inspect a network :** docker network inspect

**Create a network :** docker network create —driver

**Attach a network to container :** docker network connect

**Detach a network from container :** docker network disconnect

### By default there are 3 network created :
- **bridge :** connects to system network with out security, by default all containers are connected to this network
- **host :** connects to system network with out security. Skips virtual networking of docker.
- **none :** gives no internet access.
---
### Creating a new network

> `docker network create my_network` This will create a network with default bridge driver configuration.
\
This can be verified by command `docker network inspect my_network` , the driver value in json is bridge and the IPAM Config are identical to bridge network.

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

--attachable Enable manual container attachment
--aux-address map Auxiliary IPv4 or IPv6 addresses used by

Network driver (default map[])

--config-from string The network from which copying the configuration
--config-only Create a configuration only network
-d, --driver string Driver to manage the Network (default "bridge")
--gateway strings IPv4 or IPv6 Gateway for the master subnet
--ingress Create swarm routing-mesh network
--internal Restrict external access to the network
--ip-range strings Allocate container ip from a sub-range
--ipam-driver string IP Address Management Driver (default "default")
--ipam-opt map Set IPAM driver specific options (default map[])
--ipv6 Enable IPv6 networking
--label list Set metadata on a network
-o, --opt map Set driver specific options (default map[])
--scope string Control the network's scope
--subnet strings Subnet in CIDR format that represents a
network segment
```

### Running a container with our new network

Assume we want to run a `nginx` container which is attached to our new network `my_network'

> **Command**  `docker container run -d --name nginx2 --network my_network nginx`. By adding an argument `--network` we can pass the name of network we want our new container connected to.

> This can be verified by inspecting our network my_network `docker network inspect my_network`, we can see in the container json the name of the continer attached to this network.
```
"Containers": {
"7b819aa9e8d129b37c549ab9d0657b7347707fbe6d35152121983b8b7b493056": {
"Name": "nginx2",
"EndpointID": "35b117361e396f2154b78f5014558e8efe1e8645b7846d14ab492430dc1e7304",
"MacAddress": "02:42:ac:12:00:02",
"IPv4Address": "172.18.0.2/16",
"IPv6Address": ""
}
}
```

  

### Connecting/disconnecting a container to an existing network on fly, can be achieved via cli easily.

> **Command Syntax** `docker network connect [network_name/hash] [container_name/hash]`

> **Command** `docker network connect bridge nginx2`

In the above command `bridge` is the network name or we can use the hash that is created automatically by docker, and `nginx2` is the container name we ran above. Now when we inspect the container we'll see that our container is now connected to two networks.

```
 "Networks": {
                "bridge": {
                    "IPAMConfig": {},
                    "Links": null,
                    "Aliases": [],
                    "NetworkID": "310615086fb0946bd2b5a71dcb6a95dc6f68896c9f9666bc814144c0f1be9480",
                    "EndpointID": "e29e7d5045a80b5717a95fed81dc92a161e85b8558a669419391d05a7a251c55",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": {}
                },
                "my_network": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": [
                        "7b819aa9e8d1"
                    ],
                    "NetworkID": "f20c2457b4b01059649e25a3045168c49d43e92320100a5adb09df698f74c104",
                    "EndpointID": "35b117361e396f2154b78f5014558e8efe1e8645b7846d14ab492430dc1e7304",
                    "Gateway": "172.18.0.1",
                    "IPAddress": "172.18.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:12:00:02",
                    "DriverOpts": null
        }
}
```

### Disconnect an image from network
> To remove a container from a network we can run command `docker network disconnect f20c2457b4b0 nginx2`.

