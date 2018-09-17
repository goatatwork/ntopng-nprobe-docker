## Ntop+Nprobe

#### Mission

A dockerized ntopng install that talks to an nProbe instance. The purpose of the nProbe instance to provide a "flow sink" for anything on the network that I point to it. The end result should be that anytime I point flow data to the nProbe collector, I can view it in ntopng.

#### Containers

- nprobe
- ntopng
- redis

Networking:

- **ntopng** is listening on 3000/TCP for connections to the web front end. This should be exposed to the world via 3000/TCP or some other TCP port of your choosing.
- **nprobe** is listening on 2055/UDP for incoming flow data. This should be exposed to the world via 2055/UDP or some other UDP port of your chosing.
- **nprobe** is listening internally, on the ***ntopng*** network, on 5556/TCP. This is the port that **ntopng** will use to get flow data.

**ntopng** does not need host networking in this case because it is only paying attention to what is going on over at `--interface="tcp://nprobe:5556"`, **nprobe**'s open 5556/TCP on the ***ntopng*** network.

#### docker-compose Notes

Both the **ntopng** and the **nprobe** services are built when the services start. The `docker-compose.yml` file is using `command: ["/etc/ntopng/ntopng.conf"]` and `command: ["/etc/nprobe/nprobe.conf"]` to tell ntop and nprobe, respectively, to look at the configuration files for their configs. If you want to provide your own startup options, either change the config files before building, or provide them as an array to the `command:` configuration value. Example: To start nprobe with command line options instead of from the config file, change:

```
command: ["/etc/nprobe/nprobe.conf"]
```

To this:

```
command: ["--zmq", "tcp://*:5556","--collector-port","2055","-n","none","-i","none"]
```

Also note the image names being used on build with the `image:` value in `docker-comose.yml`. If you want to rename those or tag them differently, `docker-compose.yml` is the place to do it.

`docker-compose up -d --build` Will bring everything up. You can now log into **ntopng** on port *3000* at the IP of your host.

You should now be able to view traffic from any network device sending flow data to your host's IP on 2055/UDP.
