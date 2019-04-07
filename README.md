# Docker Swarm
This is my basic setup to get a Docker Swarm running. Each of the folders above is a separate `stack` within the swarm.

### Networking and reverse proxying
In the docker-compose files you'll see some `networks: - public`, I created a network within the Docker Swarm called `public` in `overlay` mode. Any service that I want to expose to the outside world needs to join this `public` network, so the `nginx` instances; exposed on `ports: 80:80` and `443:443` can be automatically loadbalanced by the Docker Swarm, which will receive the IPs over the `public` network from the instances detected.

The `environments: VIRTUAL_HOST` is what `dockergen` uses to see which IP needs to be routed to which server address. 
You can inspect the resulting `nginx` configuration with the following command:

```
docker exec -ti <container-apache-id> cat /etc/nginx/conf.d/default.conf
```


### Swarmpit
I use the open source Swarmpit to visualize and control the swarm from a UI. It's nice and reliable, uses the right terminology and seems to work well for me. Of course you can swap the `swarmpit` configuration for any other application you like to manage the swarm, or you can omit it completely from your setup if you want.