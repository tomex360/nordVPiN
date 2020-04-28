# nordVPiN
 scripts for NordVPN on the Pi

**Work in progess - Do not use


nordVPiN is a collection of scripts to setup and manage and monitor VPN connections, specifically for NordVPN on the Raspberry Pi (Raspbian)

dlnord - download openvpn configs, setup auto authentication and install,

vpn - without args will disconnect and reconnect to a random server from default country (USA). vpn countrycode (eg. vpn uk) to connect to a random server from that country 

cronvpn - runs as a cronjob, checking for connectivity. If no connectivity, will disconnect and reconnect to a random server from default country (USA)

proxy - will display current IP information, geolocation etc, usefull for monitoring/troubleshooting
