# docker
Home Linux Server

Commands:

Ensure you are in the docker folder as environment variables wont work

docker-compose -f docker/docker-compose-t2.yml up -d

# Configuration Files:

———————————————————————————————

# Environment
Use .env file now for variables instead of environment


——————————————————————————————

# DHCP-Helper for use with PiHole

Create folder dhcp-helper under ~/docker

Create file Dockerfile in ~/docker/dhcp-helper

FROM alpine:latest
RUN apk --no-cache add dhcp-helper
EXPOSE 67 67/udp
ENTRYPOINT ["dhcp-helper", "-n"]

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
