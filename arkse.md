# Ark Survival Evolved - Server

## Installation & Configuration
1. Install the _arksurvivalevolved_ chart from the TrueCharts Catalog
1. Make sure put this as _GAME_PARAMS_: `?RCONPort=27020?RCONEnabled=True`. This may be added by default when the chart leaves incubation status.
1. Add `-crossplay` to the _GAME_PARAMS_EXTRA_ so you get: `-server -log -crossplay`
1. Leave _Networking and Services_ on the default settings
1. Leave _Storage and Persistence_ on the default settings, i.e. with PVC
1. For _Configure Additional App Storage_ add backup to the backup folder on storage, e.g: _host path (simple)_ = `/mnt/Spinners/storage/server-backups/ark-se`", _Mount Path_ = `/backup`
1. With _Addons_, enable "CodeServer" if you want to edit configuration files directly in the browser


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
todo
