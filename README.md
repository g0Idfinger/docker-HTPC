# **Docker HTPC**

## This is built on Ubuntu
The following containers are in the the docker-compose-t2.yml file

#### FRONTENDS

* Traefik V2 - used for reverse proxy
* Authelia - private forwad auth with DUO push
* Portainer - Web GUI to manage docker stacks/containers/images/networks
* Heimdall - for having all mgmt urls in one place

#### DATABASE

* MariaDB - DB for some of the containers
* phpMyAdmin - Manage MariaDB

#### DOWNLOADERS

* qBittorrent - torrent downloader

#### INDEXERS
* Jackett - Torrent Proxy

#### PVRS
* Radarr - Movie Managment
* Sonarr - TV Shows Management

#### MEDIA SERVER
* Plex - Media Server
* Tautulli - Plex stats and monitoring
* Piwigo - Photo album

#### SYSTEM

* Firefox - web browser
* Glances - system monitor
* PiHole - ad blocker
* dchphelper - used with pihole to allow for dhcp

#### MAINTENANCE

* Ouroboros - Automatic Docker Container Updates
* Docker-GC - Automatic Docker Garbage Collection

## Usage

### Installation
Install Ubuntu, Docker and Docker Compose

1. Clone the repo.
2. Create files for Traefik

    * touch $USERDIR/docker/traefik2/acme/acme.json
    * chmod 600 $USERDIR/docker/traefik2/acme/acme.json
    * touch $USERDIR/docker/traefik2/traefik.log 
    * For other providers other than cloudflare, [check here](https://docs.traefik.io/v2.0/https/acme/#providers).
    
3. (Optional) Enable or use HTTP Basic Authentication by renaming `shared\.htpasswd.example` to `shared\.htpasswd` in the folder and adding username and hashed password to it. 
4. Configure environmental variables (`.env` file)
  * Rename the included `.env.example` to `.env`.
  * Edit variables in `.env` file. 
  * All variables (ie. `${XXX}`) in docker-compose.yml come from `.env` file stored in the same place as docker-compose.yml. 
  * Ensure good permissions for the `.env` file (recommended: 640).
5. Edit `docker-compose-t2.yml` to include only the services you want or add additional services to it. 
6. If using pihole ensure you create the files indicated below for dhcphelper, also if you have host DNS issues read below to troubleshoot/fix
7. Start your docker stack "docker-compose -f docker-compose-t2.yml up -d" 

### Configuration Files:
#### Environment

    Configure .env with the variables. see .env.example
    Use .env file now for variables instead of /etc/environment

#### DHCP-Helper for use with PiHole

1. Create folder dhcp-helper under ~/docker
2. Create file Dockerfile in ~/docker/dhcp-helper
3. Add:

    `FROM alpine:latest`  
    `RUN apk --no-cache add dhcp-helper`  
    `EXPOSE 67 67/udp`  
    `ENTRYPOINT ["dhcp-helper", "-n"]`  


## HOW TO FIX HOST DNS ISSUES

#### disable systemd-resolved service.
sudo systemctl disable systemd-resolved.service

#### Stop the service
sudo systemctl stop systemd-resolved.service

##### Then, remove the link to /run/systemd/resolve/stub-resolv.conf in /etc/resolv.conf
sudo rm /etc/resolv.conf

#### Add a manually created resolv.conf in /etc/
sudo vim /etc/resolv.conf

#### Add your prefered DNS server there
nameserver <IP OF HOST>
   
## Dark Mode for pihole

* Console into docker container
   
   `docker exec -it pihole /bin/bash`

1. `cd /var/www/html/admin/style/vendor/`
2. `sudo git clone https://github.com/jacobbates/pi-hole-midnight.git`
3. `sudo rm -f skin-blue.min.css`
4. `sudo cp pi-hole-midnight/skin-blue.min.css .`
5. `sudo rm -rf pi-hole-midnight`
