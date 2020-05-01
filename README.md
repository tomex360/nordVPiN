# nordVPiN
 scripts for NordVPN on the Pi

nordVPiN is a collection of scripts to setup, manage and monitor VPN connections, specifically for NordVPN on the Raspberry Pi (Raspbian Buster). nordVPiN does not use the nordvpn command line utility, and does not provide features such as internet kill switch. In most cases, the nordvpn utility is probably what you are looking for. 

**Heavily customised installations may run into issues. User "pi" must be present**

### Installation instructions:
```
cd /home/pi
git clone https://github.com/tomex360/nordVPiN
cd nordVPiN
sudo ./dlnord
```

### dlnord
Will setup and install. performs the following actions:
- check for installation of openVPN and install if required
- create ~/.config/nordVPiN, location for 2 config files mentioned below
	- pref.txt - stores a country code, eg, us,uk,de.. which will be the default server location if a location is not specified when connecting. The user provides this during installation, but can easliy be changed in a text editor at any time
	- log.txt - stores a counter and time stamp of every occasion that  cronvpn had to take action to resetablish the vpn connection
- download zip from nordVPN containing server connection configs
- unzip the zip
- modify the configs to support automatic authentication from credentials stored in a file
- create a file with user provided credentials (**Warning** this is stored in plain text)
- move modified server confs and auth file to /etc/openvpn/
- install scripts 'vpn', 'proxy' and 'cronvpn' to /usr/local/bin/
- add line 'AUTOSTART="current"' to /etc/default/openvpn, which will alllow systemd to start the openvpn daemon with last used config file on reboots etc. current.conf is resaved everytime  a connection is made via the accompanying 'vpn' script
- add a cron job to run accompanying script 'cronvpn' at 5 min intervals


### vpn
without args will disconnect and reconnect to a random server from users prefered location. vpn countrycode (eg. vpn uk) to connect to a random server from that country 
```
$ sudo vpn 
no country given, using prefered: de
Connecting to: /etc/openvpn/de628.nordvpn.com.udp1194.ovpn
Connection Information
  ip: 141.98.102.132
  hostname: client.m247.ro
  city: Frankfurt am Main
  region: Hesse
  country: DE
  loc: 50.10258.6299
  org: AS9009 M247 Ltd
  postal: 60326
  timezone: Europe/Berlin
```

```
$ sudo vpn pt
Connecting to random pt server
Connecting to: /etc/openvpn/pt50.nordvpn.com.udp1194.ovpn
Connection Information
  ip: 195.158.248.165
  city: Lisbon
  region: Lisbon
  country: PT
  loc: 38.7167-9.1333
  org: AS203020 HostRoyale Technologies Pvt Ltd
  postal: 1099-042
  timezone: Europe/Lisbon

```
### cronvpn
runs as a cronjob. Checks connection and  tries to reestablish connection depending on the mode used (see below) and makes a log in .config/nordVPiN/log.txt. There are two modes. 

#### Mode 1
Tests for nordVPN connection status (protected/unprotected). If the connection is up, but not through nordVPN, it will try to reconnect to a random nord server matching your prefered location (USA is default). Use this mode if you wish to be connected through nord at all times 


```
#Mode 1 
#Comment the line below if you wish to use Mode 2
if [  $status == "true" ]; then
#/

#Mode 2 
#uncomment line below, and comment out Mode 1 if statement to run in mode 2 (see README.md)
#if ping -q -c 1 -W 1 8.8.8.8 >/dev/null; then
#/ 
```
#### Mode 2
Simply tests for internet connectivity. Only if the connection is down (ping fails) will attempt to reconnect to a random nord server matching your prefered location (USA is default). Use this mode if you wish only to reconnect if the server has become unresponsive or the connection is hanging. This mode will not do anything if the connection has reverted to your ISP


```
#Mode 1 
#Comment the line below if you wish to use Mode 2
#if [  $status == "true" ]; then
#/

#Mode 2 
#uncomment line below, and comment out Mode 1 if statement to run in mode 2 (see README.md)
if ping -q -c 1 -W 1 8.8.8.8 >/dev/null; then
#/ 
```


### proxy
will display current IP information, geolocation etc, usefull for monitoring/troubleshooting
