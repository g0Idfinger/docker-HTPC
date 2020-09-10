# Changelog
## Planned: 
* Find a photo organzir that supports HEIC. 
## September 10, 2020
* Switched to authelia, removed google auth
* Added Secrets
* Added Guacamole
* Added Cloudflare Companian
* Added Cloudflare DDNS
## April 23, 2020
* Added a section for qbittorrent with vpn built into the same container
* Added configuration for Jackett to use VPN
## April 22, 2020
* Cleaned up Pihole traefik2 labels
* Enabled CloudFlared for PiHole
* Added Dark Theme for PiHole
## April 17, 2020
* Issues with Pihole not able to whitelist domains. Removed security_opt: no-new-privileges:true 
## April 14, 2020
* Updated README.md with a better walkthrough guide.
* Fixed direcory from /$USERDIR/docker to /$USERDIR/docker-HTPC
* Glances - moved glances.conf glances/glances.conf
* Pihole - add instruction to `touch pihole/pihhole.log` before runing pihole container as it cause pihole to not start properly
* CloudFlared - changed dns to defaul 1.1.1.1 and 1.0.0.1 instead of 1.1.1.2 and 1.0.0.2 as they don't work.
## April 9, 2020
* Fixed missing traefik2\rules folder and files
* Fixed Google Auth for traeffik dashboard
* Added info to add darkmode for Pihole Dashboard in readme
