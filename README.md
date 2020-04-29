# nordVPiN
 scripts for NordVPN on the Pi

nordVPiN is a collection of scripts to setup, manage and monitor VPN connections, specifically for NordVPN on the Raspberry Pi (Raspbian Buster).

**Heavily customised installations may run into issues. User "pi" must be present**

dlnord - Run this first, and only once, as root. It will perform the following:
- check for installation of openVPN and install if required
- create ~/.config/nordVPiN, location for 2 config files mentioned below
- download zip from nordVPN containing server connection configs
- unzip the zip
- modify the configs to support automatic authentication from credentials stored in a file
- create a file with user provided credentials (**Warning** this is stored in plain text)
- move modified server confs and auth file to /etc/openvpn/
- install scripts 'vpn', 'proxy' and 'cronvpn' to /usr/local/bin/
- add line 'AUTOSTART="current"' to /etc/default/openvpn, which will alllow systemd to start the openvpn daemon with last used config file on reboots etc. current.conf is resaved everytime  a connection is made via the accompanying 'vpn' script
- add a cron job to run accompanying script 'cronvpn' at 5 min intervals


vpn - without args will disconnect and reconnect to a random server from default country (USA). vpn countrycode (eg. vpn uk) to connect to a random server from that country 

cronvpn - runs as a cronjob, checking for connectivity. If no connectivity, will disconnect and reconnect to a random server from default country (USA)

proxy - will display current IP information, geolocation etc, usefull for monitoring/troubleshooting
