## Technitium DNS Multi-Architecture Image for AMD64, ARM64, and ARM
Technitium DNS is a full featured Web based DNS/DHCP server - https://technitium.com/dns/

Version 5.6

Technitium DNS Server is an open source tool that can be used for self hosting a local DNS server for privacy & security or, used for experimentation/testing by software developers on their computer. It works out-of-the-box with no or minimal configuration and provides a user friendly web console accessible using any web browser.

### The docker run command below will pull the correct architecture (amd64,arm64,arm32) for your host.
#### Creating a docker network with an alias will ensure seamless updates.

`docker network create technitium-network`

`docker run -d --name technitium -p 53:53/udp -p 53:53/tcp -p 67:67/udp -p 5380:5380 --network=technitium-network --network-alias=technitium-dns -v data:/app/config m400/technitium`

or by version number  

`docker run -d --name technitium -p 53:53/udp -p 53:53/tcp -p 67:67/udp -p 5380:5380 --network=technitium-network --network-alias=technitium-dns -v data:/app/config m400/technitium:5.6`

Above command maps ports 53 udp and tcp for dns, port 67 udp for built-in dhcp server, port 5380 for web interface, creates a volume named data that will save server configuration.

### Default username 'admin' and password 'admin'

### Docker-compose example
```
version: '3.5'
services:
  dns_server:
    image: m400/technitium
    networks:
      technitium-network:
        aliases:
          - technitium-dns
    ports:
    - 53:53/udp
    - 53:53/tcp
    - 67:67/udp
    - 5380:5380
    #environment:
    #- PUID=1000                  #https://docs.docker.com/engine/security/userns-remap/
    #- PGID=1000
    volumes:
    - data:/app/config
    restart: unless-stopped
volumes:
  data:
networks:
  technitium-network:

```

### Additional Information
1.) Ports 53, 67 and 5380 should not be in use on the host running the container. Docker will bind the container to those ports.

2.) The container will set the Default DNS server name as the container ID. This will have to be changed after updating the container image because changing the image creates a new container with a new ID. If not changed service will be broken. The solution is to change the old container ID to new container ID. 

3.) A better solution is to create the container with a user defined network and alias. The network alias should then be set in the settings tab which is saved to the mapped volume.

#### Settings tab with alias.
![screenshot](https://user-images.githubusercontent.com/47049792/100488543-d4704d00-30dc-11eb-9df2-953eda7c8195.png)
	
#### Settings tab with Default Container ID.
![screenshot](https://user-images.githubusercontent.com/47049792/100488561-fa95ed00-30dc-11eb-8b44-0327dd3d0cab.png)


