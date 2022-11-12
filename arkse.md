# Ark Survival Evolved - Server

## Installation & Configuration
1. Install the _arksurvivalevolved_ chart from the TrueCharts Catalog
1. Make sure put this as _GAME_PARAMS_: `?RCONPort=27020?RCONEnabled=True`. This may be added by default when the chart leaves incubation status.
1. If you're hosting multiple servers, you have to change the game ports, query port and RCON port. Change the _GAME_PARAMS_ to: `?RCONPort=27021?RCONEnabled=True?QueryPort=27016?Port=7779?bRawSockets` and adjust to what's needed. Then specify the same ports in the settings, but also set the _Target Port_ in the *advanced settings* for each port. The bRawSockets should improve performance.
1. Add `-crossplay` to the _GAME_PARAMS_EXTRA_ so you get: `-server -log -crossplay`
1. Leave _Networking and Services_ on the default settings
1. Leave _Storage and Persistence_ on the default settings, i.e. with PVC
1. For _Configure Additional App Storage_ add the Saved folder (where Ark keeps all config and save files) as a mounted folder: _host path (simple)_ = `/mnt/Spinners/apps/ark-server/arkserver-01`", _Mount Path_ = `/serverdata/serverfiles/ShooterGame/Saved`
1. With _Addons_, enable "CodeServer" if you want to edit configuration files directly in the browser

## Port forwarding
See also this [wiki page on dedicated server setup](https://ark.fandom.com/wiki/Dedicated_server_setup)
1. Make sure you forward the ports in both the internet provider router (unless its in bridge mode) and your own router
1. Ports need to differ for each server
   1. Game port: by default `7777`, each next server is *+2*, e.g. `7779`
   1. Rawsockets port: by default `7778`, each next server is *+2*, e.g. `7780`
   1. Query port: by default `27015`, each next server is *+1*, e.g. `27016`. Used by Steam and also the port to specify when adding the server in steam
   1. RCON port: by default `27020`, each next server is *+1*, e.g. `27021`. Used for remote administration. Optional, not needed for gameplay.
   
When adding a server in steam, specify it with the query port, e.g. 192.168.1.17:27016

## Migration of data
1. Collect all data in the old server in the Saved folder of the world(s) you need to migrate, e.g: `/home/arkserver/serverfiles/ShooterGame/Saved/TheIsland`
   1. The `.ark` file, e.g. `TheIsland.ark` (this is the latest save of the world)
   1. All the `.arktribe` files
   1. All the `.arkprofile` files
   1. The complete `ServerPaintingsCache` folder
1. Mount the PVC of the Ark pod using the TrueCharts [TrueTool](https://github.com/truecharts/truetool) (must run as root with environment, i.e. `su -`!)
1. Copy over all the files into the same location in the new server / Ark pod

## Editing config files
Two options:
- edit the files via the code editor via the browser and stop/start the Ark container after editing (or figure out how to restart the Ark server in the container)
- mount the PVC of the Ark pod with the TrueTool (will automatically stop the pod) and edit via SSH

## Backup
Ark regularly creates backups. These are in the Saved folder that we mounted as specified above. If you want to keep backups separately, just copy them over from the saved folder.

## Troubleshooting / Checking status
You can check if the server and accepting connections by checking the ports that listen for connections:
1. Install *iproute2* to use the *ss* utility: `apt update && apt install -y iproute2`
1. Check for open ports: `ss -tuplwn`
   - This should list 4 open ports, as specified above
   
