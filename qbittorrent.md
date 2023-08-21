# QBittorrent

NOTE: Use Gluetun now for VPN, see here: https://truecharts.org/manual/SCALE/guides/vpn-setup

## Installation & Configuration
1. Create a new user `qtorrent` with _uid_ `2002`
1. Create a folder `torrents` in the `downloads` folder (make sure its owned by the `apps` group)
1. Create a folder `_vpn-config` in the `apps` folder (make sure its owned by the `apps` group)
1. Create a wireguard configuration file in your vpn provider's portal. 
1. Strip out any IPv6 related entries, e.g. starting with `00` or `::`, it should look like:

```
[Interface]
PrivateKey = blahblahblah
Address = 10.64.208.51/32
DNS = 10.64.0.1

[Peer]
PublicKey = blahblahblah
AllowedIPs = 0.0.0.0/0
Endpoint = 92.60.40.224:51820
```

6. Store the vpn config file in the `_vpn-config` folder
1. Leave all the default settings, except Add one _Configure Additional App Storage_ with _Host Path_ set to the `torrents` directory (e.g. `/mnt/Spinners/downloads/torrents`) and the _Mount Path_ set to `/downloads`
1. Under _Security and Permissions_, set the _runAsUser_ to `2002` (leave _runAsGroup_ and _fsGroup_ set to `568`)
1. Under _Addons_, select `Wireguard` and tick yes for _Enable Killswitch_
1. Add two _Killswitch Excluded IPv4 networks_: `192.168.0.0/16` (internal LAN network) and `172.16.0.0/16` (Kubernetes network)
1. Specify for _Full Path to File_ the location of the vpn config file as used in step 6, e.g. `/mnt/Spinners/apps/_vpn-config/mullvad-nl.conf`
1. See screen shot below for the vpn section of the configuration

## Port Forwarding
1. In your VPN provider's portal, setup a port for forwarding
2. Note down the port (some providers like Mullvad do not allow you to chose a specific port)
3. Specify this port in the QBittorrent interface: Options > Connections > Listening Port
4. Check if the port is open, by running `curl ip.me` in the shell of the pod, note the ip address and test using a port check tool website (e.g. [you get signal](https://www.yougetsignal.com/tools/open-ports/))

## Screen shot VPN section of the configuration

![VPN Wireguard settings](./images/vpn_wireguard_settings.png "VPN Wireguard settings")


