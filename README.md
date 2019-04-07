In the docker-compose files you'll see some `networks: - public`, I created a network within the Docker Swarm called `public` in `overlay` mode. Any service that I want to expose to the outside world needs to join this `public` network, so the `nginx` instances; exposed on `ports: 80:80` and `443:443` can be automatically loadbalanced by the Docker Swarm, which will receive the IPs over the `public` network from the instances detected.

The `environments: VIRTUAL_HOST` is what `dockergen` uses to see which IP needs to be routed to which server address. 
You can inspect the resulting `nginx` configuration with the following command:

```
docker exec -ti <container-apache-id> cat /etc/nginx/conf.d/default.conf
```
