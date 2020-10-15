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

### Docker Install
* `sudo apt-get install apt-transport-https ca-certificates curl software-properties-common`
* `curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -`
* `sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"`
* `sudo apt update`
* `sudo apt install docker-ce`
* `sudo docker run hello-world` To test docker
* `sudo useradd -m username` optional to create a different user
   * `sudo passwd username` to set password
* `sudo usermod -aG docker ${USER}` to assign to docker group

### Clone the repo.
1. Create .env file for environmental variables, make sure to change USER to your username
    * `mv $USERDIR/docker-HTPC/.env.example $USERDIR/docker-HTPC/.env`
    * edit the variables to your liking, comment out ones you don't need and vice versa
2. Create files for Traefik
    * mkdir $USERDIR/docker-HTPC/traefik2/
    * mkdir $USERDIR/docker-HTPC/traefik2/acme
    * touch $USERDIR/docker-HTPC/traefik2/acme/acme.json
    * chmod 600 $USERDIR/docker-HTPC/traefik2/acme/acme.json
    * touch $USERDIR/docker-HTPC/traefik2/traefik.log 
    * For other providers other than cloudflare, [check here](https://docs.traefik.io/v2.0/https/acme/#providers).
    
3. You will have to put your sensitive information in files. Create folder/files for Docker Secrets
    * mkdir $USERDIR/docker-HTPC/secrets
    * chmod 600 $USERDIR/docker-HTPC/secrets
    * sudo su
    * cd $USERDIR/docker-HTPC/secrets
    * make your files for each individual credentials or APIs that you want to use.  I have created the following files and entered in the corresponding info:
         * authelia_duo_api_secret_key
         * authelia_jwt_secret
         * authelia_notifier_smtp_password
         * authelia_session_secret
         * authelia_storage_mysql_password
         * cloudflare_api_key
         * cloudflare_email
         * cloudflare_zoneid
         * google_client_id
         * google_client_secret
         * guac_db_name
         * guac_mysql_password
         * guac_mysql_user
         * my_email
         * mysql_root_password
         * oauth_secret
         * plex_claim
         * RADARR_API_KEY
    
4. (Optional) Enable or use HTTP Basic Authentication by renaming `shared\.htpasswd.example` to `shared\.htpasswd` in the folder and adding username and hashed password to it. 
5. Configure environmental variables (`.env` file)
  * Rename the included `.env.example` to `.env`.
  * Edit variables in `.env` file. 
  * All variables (ie. `${XXX}`) in docker-compose.yml come from `.env` file stored in the same place as docker-compose.yml. 
  * Ensure good permissions for the `.env` file (recommended: 640).
6. Edit `docker-compose-t2.yml` to include only the services you want or add additional services to it. 
7. If using pihole ensure you create the files indicated below for dhcphelper, also if you have host DNS issues read below to troubleshoot/fix
8. Start your docker stack "docker-compose -f docker-compose-t2.yml up -d" 

### Configuration Files:
#### Environment

    Configure .env with the variables. see .env.example
    Use .env file now for variables instead of /etc/environment

#### DHCP-Helper for use with PiHole

1. Create folder dhcp-helper under ~/docker
2. Create file Dockerfile in ~/docker-HTPC/dhcp-helper
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
