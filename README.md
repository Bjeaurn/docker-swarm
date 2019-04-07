# Docker Swarm
This is my basic setup to get a Docker Swarm running. Each of the folders above is a separate `stack` within the swarm.

## Networking and reverse proxying
In the docker-compose files you'll see some `networks: - public`, I created a network within the Docker Swarm called `public` in `overlay` mode. Any service that I want to expose to the outside world needs to join this `public` network, so the `nginx` instances; exposed on `ports: 80:80` and `443:443` can be automatically loadbalanced by the Docker Swarm, which will receive the IPs over the `public` network from the instances detected.

The `environments: VIRTUAL_HOST` is what `dockergen` uses to see which IP needs to be routed to which server address. 
You can inspect the resulting `nginx` configuration with the following command:

```
docker exec -ti <nginx-container-id> cat /etc/nginx/conf.d/default.conf
```

### Create the public network
`docker network create public -d overlay`

### VIRTUAL_HOST
Throughout the `docker-compose.yml` files, you will see `environment` settings called `VIRTUAL_HOST`. I've omitted the actual values for now (could move these to a `.env` file I guess), so make sure they reflect your setup and situation before you start deploying.

## Apps included

### Registry
Docker-registry is a `RegistryV2` for private hosting of docker images. It works, but is not in a usable state at the time of writing due to HTTPS issues. I intend to use this to create my apps and send them to this privately hosted and managed Docker Registry for easy deployment within the swarm. You could omit this and use a public registry.

### Swarmpit
I use the open source Swarmpit to visualize and control the swarm from a UI. It's nice and reliable, uses the right terminology and seems to work well for me. Of course you can swap the `swarmpit` configuration for any other application you like to manage the swarm, or you can omit it completely from your setup if you want.

### Example
This is just an example, only containing a basic `nginx` image that is published on the `public` network so it can be reverse proxied into. It works perfectly.

### Reverse-proxy
This contains the entire multi-container setup to run a `reverse-proxy`, based on the work done by [@jwilder/nginx-proxy](https://github.com/jwilder/nginx-proxy). Read the README contained within the folder for more details on options and how it works exactly.

This stack basically makes sure that the apps published on the `public` network can be reverse proxied into by the outside world. It uses `dockergen` to listen to docker events, and read the `environment: VIRTUAL_HOST` and other values to generate the appropriate `nginx` configuration files. It should automatically reload nginx, but that doesn't seem to work in my multi container setup as of yet. As I said; read the contained README for more details.