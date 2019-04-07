The reason this docker-compose has two networks is that you may want to add additional services, like a database or a backend. These will use the `default` network so ther are not exposed to any other stacks or the outside world over the `public` network.

If you're just running an nginx like in this example, you could remove the `default` network, or change the `default` network to connect to the external public network like:

```
networks:
  default:
    external:
      name: public
```