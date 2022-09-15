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

## Usage
1. Create a script that will setup the crontab on startup

See Stackoverflow [here](https://stackoverflow.com/questions/878600/how-to-create-a-cron-job-using-bash-automatically-without-the-interactive-editor) 
`(crontab -l ; echo "00 09 * * 1-5 echo hello") | crontab -`