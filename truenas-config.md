# TrueNAS Configuration

## Configure the System Dataset Pool
To avoid constant writes to the datadisks, change the system dataset pool to the bootdisk
1. System Settings > Advanced > System Dataset Pool > Configure
1. Change the pool to `boot-pool`


## Configure Kubernetes network settings
If you don't do this, your apps may stay stuck in "deploying" status (mentioned [ere](https://www.truenas.com/community/threads/apps-stuck-deploying.103716/post-714282)
1. Go to apps > Available Applications > Settings > Advanced Settings
1. Configure `Route v4 Interface` and `Route v4 Gateway`

## Setup additional time servers
This is good practice, but also to avoid the alert `NTP health check failed - No NTP peers`

Add additional time servers using the GUI under _Systems Settings > General 

There's a list of servers on GitHub [List of Top Public Time Servers](https://gist.github.com/mutin-sa/eea1c396b1e610a2da1e5550d94b0453)
* 0.debian.pool.ntp.org
* nl.pool.ntp.org
* time-a-g.nist.gov
* time.google.com
* time.cloudflare.com
* time.facebook.com
* time.windows.com
* time.apple.com
