# Passenger - Full

Passenger by phusion seems to be a good Docker image for Python scripting and running cron tasks (also NodeJS and Ruby). See the [Docker Hub page](https://hub.docker.com/r/phusion/passenger-full) and [GitHub](https://github.com/phusion/passenger-docker) for more details.

Be aware that changes to users or crontab (e.g adding a user to a group or install a crontab entry) will be wiped when the container is restarted. Therefore any configuration needs to be put in a script and run upon startup.

## Preparations
The passenger image has an app user with UID 9999 and home directory /home/app.
1. Add a local user `passenge` with UID 9999 and put `apps` as primary group
1. Create a folder for passenger in the apps folder: `mkdir /mnt/Spinners/apps/passenger`
1. Set the Group ID bit on the folder: `chmod g+s /mnt/Spinners/apps/passenger`
1. Set the ownership to passenger and group apps: `chown -R passenge:apps /mnt/Spinners/apps/passenger`

## Installation & Configuration
1. Use the `launch Docker Image` button
1. Put in `phusion/passenger-full` as image repository, tag is `latest`
1. Add the following as a _Configure Container CMD_:  `/sbin/my_init`
1. Add the following as a _Container Environment Variable_:  Name: `ROOT` Value: `/root`
1. Add the following as a host path: `/mnt/Spinners/apps/passenger` with mount point `/home/app`
1. Set the _Upgrade Policy_ to `Kill existing pods before creating new ones`

## Create a script that will setup group for user app and the crontab on startup

1. Add the script below in a scripts folder (e.g. `/home/app/config/set_config.sh`)
1. Change the _Configure Container CMD_:  `/sbin/my_init` to your script name, e.g. `/home/app/config/set_config.sh`


```
#!/bin/bash

## Set timezone to CET / Amsterdam
ln -sf /usr/share/zoneinfo/Europe/Amsterdam /etc/localtime

## Configure group for user app
groupadd -g 568 apps
usermod -a -G apps app

## define cron commands

declare -a commands
declare -a schedule

#   * * * * * "command to be executed"
#   - - - - -
#   | | | | |
#   | | | | ----- Day of week (0 - 7) (Sunday=0 or 7)
#   | | | ------- Month (1 - 12)
#   | | --------- Day of month (1 - 31)
#   | ----------- Hour (0 - 23)
#   ------------- Minute (0 - 59)

commands[0]="python3 /home/app/scripts/arkserver-notify/arkserver-notify.py"
schedule[0]="* * * * *"    # every minute

#commands[1]="echo hello"
#schedule[1]="* * * * *"    # every minute

## get length of the array
len=${#commands[@]}
 
## Use bash for loop 
for (( i=0; i<$len; i++ )); do     
    echo "${schedule[$i]} app ${commands[$i]} > /dev/null 2>&1" >> /etc/crontab
done

echo "#" >> /etc/crontab


# start passenger init script
/sbin/my_init
```