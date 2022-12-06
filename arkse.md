# Ark Survival Evolved - Server

## Installation & Configuration (see table below for per server settings)

This is now updated to use a different IP per server, so each server can keep its default ports. You have to install MetalLB first for this.

1. First install MetalLB, its in the _enterprise_ track of True Charts. Follow the instructions here: [MetalLB Basic Setup](https://truecharts.org/docs/charts/enterprise/metallb/setup-guide) 
1. Install the _arksurvivalevolved_ chart from the TrueCharts Catalog for each game server you want to run (probably possible to share server files, but not looked into that yet).
1. Configure following the configuration in the tables below. You have to add two storage locations. If a setting is not mentioned, leave it to its default. Do *not* forget to up the RAM resource limit.

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
1. *The port forwarding needs to convert the external port number in the table below to the default port number and ip of the server.*


| App name             | Server name                  | Map              | IP address     | Game port 1 | Game port 2 | Steam query | RCON      |
|----------------------|------------------------------|------------------|----------------|-------------|-------------|-------------|-----------|
| ark01-theisland      | lvvdg24-ark01-theisland      | TheIsland        | 192.168.30.51  | 7777 UPD    | 7778 UDP    | 27015 UDP   | 27020 TCP |
| ark02-theisland      | lvvdg24-ark02-theisland      | TheIsland        | 192.168.30.52  | 7779 UPD    | 7780 UDP    | 27016 UDP   | 27021 TCP |
| ark03-fjordur        | lvvdg24-ark03-fjordur        | Fjordur          | 192.168.30.53  | 7781 UPD    | 7782 UDP    | 27017 UDP   | 27022 TCP |
| ark04-scorchedearth  | lvvdg24-ark04-scorchedearth  | ScorchedEarth_P  | 192.168.30.54  | 7783 UPD    | 7784 UDP    | 27018 UDP   | 27023 TCP |
| ark05-aberration     | lvvdg24-ark05-aberration     | Aberration_P     | 192.168.30.55  | 7785 UPD    | 7786 UDP    | 27019 UDP   | 27024 TCP |
| ark06-extinction     | lvvdg24-ark06-extinction     | Extinction       | 192.168.30.56  | 7787 UPD    | 7788 UDP    | 28016 UDP   | 27025 TCP |

Easier to create a single rule in the ISP router for ports 7777-7790 UDP, 27015-27019 UPD and 28016-28017 UDP. RCON ports do not need outside exposure.

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
1. Stop the pod/app and copy over the files in its Saved folder 

## Editing config files
Three options:
- stop the pod and edit in Windows via Samba or directly from the commandline (the Config folder is inside the Saved folder, which is outside the PVC)
- edit the files via the code editor via the browser and stop/start the Ark container after editing (or figure out how to restart the Ark server in the container)
- mount the PVC of the Ark pod with the TrueTool (will automatically stop the pod) and edit via SSH

## Backup
Ark regularly creates backups. These are in the Saved folder that we mounted as specified above. If you want to keep backups separately, just copy them over from the saved folder.

## Troubleshooting / Checking status
You can check if the server and accepting connections by checking the ports that listen for connections:
1. Install *iproute2* to use the *ss* utility: `apt update && apt install -y iproute2`
1. Check for open ports: `ss -tuplwn`
   - This should list 4 open ports, as specified above
   
