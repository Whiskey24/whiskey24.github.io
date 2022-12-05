# Ark Survival Evolved - Server

## Installation & Configuration (see table below for per server settings)
1. Install the _arksurvivalevolved_ chart from the TrueCharts Catalog
1. Make sure put this as _GAME_PARAMS_: `?RCONPort=27020?RCONEnabled=True`. This may be added by default when the chart leaves incubation status.
1. If you're hosting multiple servers, you have to change the game ports, query port and RCON port. Change the _GAME_PARAMS_ to: `?RCONPort=27021?RCONEnabled=True?QueryPort=27016?Port=7779?bRawSockets` and adjust to what's needed. Then specify the same ports in the settings, but also set the _Target Port_ in the *advanced settings* for each port. The bRawSockets should improve performance.
1. Add `-crossplay` to the _GAME_PARAMS_EXTRA_ and specify the cluster, so you get: `-server -log -crossplay -NoTransferFromFiltering -ClusterDirOverride=/clusterdata -clusterid=cluster1`
1. Leave _Networking and Services_ on the default settings
1. Leave _Storage and Persistence_ on the default settings, i.e. with PVC
1. For _Configure Additional App Storage_ add 
   1. the Saved folder (where Ark keeps all config and save files) as a mounted folder: _host path (simple)_ = `/mnt/rapid-store/apps/ark-server/ark01-theisland`", _Mount Path_ = `/serverdata/serverfiles/ShooterGame/Saved`
   1. The Clusterdata folder (where Ark stores data transferred between servers): _host path (simple)_ = `/mnt/rapid-store/apps/ark-server/clusterdata`", _Mount Path_ = `/clusterdata`
1. Under **Resources and Devices***, tick `Set Custom Resource Limits/Requests (Advanced).` By default the memory limit is 8Gb and Ark will go over that after a few hours
   - For `Advanced Limit Resource Consumption`, set RAM to 32 Gi (this should be plenty)
   - For `Minimum Resource Required (request)`, set RAM to 8 Gi
1. With _Addons_, enable "CodeServer" if you want to edit configuration files directly in the browser

## Configuration details

| Section                 | Parameter                                   | Value                                                                                                 |
|-------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------|
| Application Name        | Application Name                            | _see table_                                                                                           |
| App Configuration       | SRV_PWD                                     | _keep secret_                                                                                         |
| App Configuration       | SRV_ADMIN_PWD                               | _keep secret_                                                                                         |
| App Configuration       | MAP                                         | _see table_                                                                                           |
| App Configuration       | SERVER_NAME                                 | _see table_                                                                                           |
| App Configuration       | GAME_PARAMS                                 | ?Port=7777?QueryPort=27015?RCONPort=27020?RCONEnabled=True?bRawSockets                                |
| App Configuration       | GAME_PARAMS_EXTRA                           | -server -log -crossplay -NoTransferFromFiltering -ClusterDirOverride=/clusterdata -clusterid=cluster1 |
| Networking and Services | Main Service                                | LoadBalancer IP: _see table_ Port: 7777                                                               |
| Networking and Services | udp2 service                                | LoadBalancer IP: _see table_ Port: 7778                                                               |
| Networking and Services | udpsteam service                            | LoadBalancer IP: _see table_ Port: 27015                                                              |
| Networking and Services | rcontcp service                             | LoadBalancer IP: _see table_ Port: 27020                                                              |
| Storage and Persistence | Integrated Persistent Storage - steamcmd    | Size quotum of Storage: 25Gi                                                                          |
| Storage and Persistence | Integrated Persistent Storage - serverfiles | Size quotum of Storage: 25Gi                                                                          |
| Storage and Persistence | Configure Additional App Storage            | Host Path: _see table_ Mount Path: /serverdata/serverfiles/ShooterGame/Saved                          |
| Storage and Persistence | Configure Additional App Storage            | Host Path: /mnt/rapid-store/apps/ark-servers/clusterdata Mount Path: /clusterdata                     |
| Resources and Devices   | Resource Limits - RAM                       | 32Gi                                                                                                  |



| App name            | Server name                 | Map             | IP address    | Additional app storage - host path                   | Additional app storage - mount path       |
|---------------------|-----------------------------|-----------------|---------------|------------------------------------------------------|-------------------------------------------|
| ark01-theisland     | lvvdg24-ark01-theisland     | TheIsland       | 192.168.30.51 | /mnt/rapid-store/apps/ark-server/ark01-theisland     | /serverdata/serverfiles/Shootergame/Saved |
| ark02-theisland     | lvvdg24-ark02-theisland     | TheIsland       | 192.168.30.52 | /mnt/rapid-store/apps/ark-server/ark02-theisland     | /serverdata/serverfiles/Shootergame/Saved |
| ark03-fjordur       | lvvdg24-ark03-fjordur       | Fjordur         | 192.168.30.53 | /mnt/rapid-store/apps/ark-server/ark03-fjordur       | /serverdata/serverfiles/Shootergame/Saved |
| ark04-scorchedearth | lvvdg24-ark04-scorchedearth | ScorchedEarth_P | 192.168.30.54 | /mnt/rapid-store/apps/ark-server/ark04-scorchedearth | /serverdata/serverfiles/Shootergame/Saved |
| ark05-aberration    | lvvdg24-ark05-aberration    | Aberration_P    | 192.168.30.55 | /mnt/rapid-store/apps/ark-server/ark05-aberration    | /serverdata/serverfiles/Shootergame/Saved |
| ark06-extinction    | lvvdg24-ark06-extinction    | Extinction      | 192.168.30.56 | /mnt/rapid-store/apps/ark-server/ark06-extinction    | /serverdata/serverfiles/Shootergame/Saved |


## Port forwarding
See also this [wiki page on dedicated server setup](https://ark.fandom.com/wiki/Dedicated_server_setup)
1. Make sure you forward the ports in both the internet provider router (unless its in bridge mode) and your own router
1. Ports need to differ for each server
   1. Game port: by default `7777`, each next server is *+2*, e.g. `7779`
   1. Rawsockets port: by default `7778`, each next server is *+2*, e.g. `7780`
   1. Query port: by default `27015`, each next server is *+1*, e.g. `27016`. Used by Steam and also the port to specify when adding the server in steam
   1. RCON port: by default `27020`, each next server is *+1*, e.g. `27021`. Used for remote administration. Optional, not needed for gameplay.
   
When adding a server in steam, specify it with the query/udpsteam port, e.g. 192.168.1.17:27016

## Map names

|       Map       | Name for dedicated servers  |
|:---------------:|:---------------------------:|
| The Island      | TheIsland                   |
| The Center      | TheCenter                   |
| Scorched Earth  | ScorchedEarth_P             |
| Ragnarok        | Ragnarok                    |
| Aberration      | Aberration_P                |
| Extinction      | Extinction                  |
| Valguero        | Valguero_P                  |
| Genesis: Part 1 | Genesis                     |
| Crystal Isles   | CrystalIsles                |
| Genesis: Part 2 | Gen2                        |
| Lost Island     | LostIsland                  |
| Fjordur         | Fjordur                     |

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
   
