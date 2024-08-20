# Jailmaker

Instructions from this video: https://www.youtube.com/watch?v=S0nTRvAHAP8

## Install Jailmaker
Follow instructions in the video, but make subdirectories in the stacks directory for each jail you intend to create

## Create the jail
1. Create a subdirectory for your new jail in the stacks directory, easy to end with -jail to indicate this is jail related configuration data, e.g. `unifi-jail`
1. Make `apps` owner of the new directory (optional)
1. Copy the docker config from the Jailmaker Github repository: https://github.com/Jip-Hop/jailmaker/blob/main/templates/docker/config
1. (As root) do `jlmkr create` and confirm you want to create a jail from a config template
1. Set startup on the first line to `1` for auto starting the jail
1. Change the `systemd_nspawn_user_args` section to use a `macvlan=eno1` and necessary bindings as follows:
   
**Take care to change the line that ends with `/opt/stacks` with the subdirectory you created in the stacks directory**

   ```
   systemd_nspawn_user_args=--network-macvlan=eno1
              --resolv-conf=bind-host
              --system-call-filter='add_key keyctl bpf'
              --bind='/mnt/rapid-store/docker/data:/mnt/data'
              --bind='/mnt/rapid-store/docker/stacks/unifi-jail:/opt/stacks'
              --bind='/mnt/rapid-store/media:/mnt/r620media'
              --bind='/mnt/rapid-store/downloads:/mnt/r620downloads'
```

## Set a static ip for the jail
1. After starting the jail, open a shell with `jlmkr shell jailname`
1. Edit this section in the file `/etc/systemd/network/mv-dhcp.network ` with the static IP you want the jail to have accordingly (change the ip to what you need!)
1. After saving the file, exit the jail and restart it with `jlmkr restart jailname` 
```
[Network]
#DHCP=yes
#LinkLocalAddressing=ipv6
DHCP=false
Address=192.168.40.1/16
Gateway=192.168.1.1
```

## Install Dockge in the jail
1. Open a shell in the jail with `jlmkr shell jailname` 
1. Create a file `install dockge.sh`, open it, copy this from https://github.com/louislam/dockge and save the file
1. After saving run the file

```
# Create directories that store your stacks and stores Dockge's stack
mkdir -p /opt/stacks /opt/dockge
cd /opt/dockge

# Download the compose.yaml
curl https://raw.githubusercontent.com/louislam/dockge/master/compose.yaml --output compose.yaml

# Start the server
docker compose up -d

# If you are using docker-compose V1 or Podman
# docker-compose up -d
```

## Adopt the Dockge agent
1. Open the Dockge page of the jail at the ip with port 5001, e.g. `http://192.168.40.1:5001`
1. Configure a username and password
1. Go to your main/first Dockge instance and adopt the agent with the ip, username and password that you just created
