Because the dockergen and letsencrypt companion aren't playing nicely with Swarm (yet), I have the replicas turned off so they're not constantly trying to send messages and eating up CPU cycles.

The dockergen is suggested to be limited to only 1 replica, you can't up this cause it'll be listening to the same docker events and generating the same files.

The nginx should be able to get scaled up to provide some sort of high availability. But because the commands from the dockergen aren't coming through properly; you will have to reset the nginx by hand.

On the server in the terminal:
```
docker ps | grep reverse-proxy_nginx.
docker exec -ti <container-id> nginx -s reload
```

You will have to do this for all the instances of Nginx you're running. Maybe this can be easily scripted.

If you don't want to use this system, or just want it to automatically generate and update; remove the nginx, nginx-gen and letsencrypt and add in `jwilder/nginx-proxy`, you can replicate this multiple times, but it's secretly dockergen.
