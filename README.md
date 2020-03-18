# docker

This is built on Ubunto
The following containers are in the the docker-compose-t2.yml file

Traefik V2 - used for reverse proxy
Google Oauth - for security and SSO
Portainer - Web GUI to manage docker stacks/containers/images/networks
Organizr - for having all mgmt urls in one place
MariaDB - DB for some of the containers
phpMyAdmin - Manage MariaDB

Jackett - Torrent Proxy
qBittorrent - torrent downloader
Radarr - Movie Managment
Sonarr - TV Shows Management
Plex - Media Server
Tautulli - Plex stats and monitoring

Piwigo - Photo album
Firefox - web browser
Glances - system monitor

PiHole - ad blocker
dchphelper - used with pihole to allow for dhcp

Ouroboros - Automatic Docker Container Updates
Docker-GC - Automatic Docker Garbage Collection

———————————————————————————————
# Configuration Files:
  # Environment

    Configure .env with the variables. see .env.example
    Use .env file now for variables instead of /etc/environment

  # DHCP-Helper for use with PiHole

    Create folder dhcp-helper under ~/docker
    Create file Dockerfile in ~/docker/dhcp-helper
    Add:
    
      FROM alpine:latest
      RUN apk --no-cache add dhcp-helper
      EXPOSE 67 67/udp
      ENTRYPOINT ["dhcp-helper", "-n"]


——————————————————————————————

Commands:

Ensure you are in the docker folder as environment variables wont work

docker-compose -f docker/docker-compose-t2.yml up -d

————————————————————————————————

HOW TO FIX HOST DNS ISSUES

### disable systemd-resolved service.
sudo systemctl disable systemd-resolved.service

### Stop the service
sudo systemctl stop systemd-resolved.service

#### Then, remove the link to /run/systemd/resolve/stub-resolv.conf in /etc/resolv.conf
sudo rm /etc/resolv.conf

### Add a manually created resolv.conf in /etc/
sudo vim /etc/resolv.conf

#### Add your prefered DNS server there
nameserver <IP OF HOST>


———————————————————————————————————————
