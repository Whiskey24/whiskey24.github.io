# TrueNAS Configuration

## Configure the System Dataset Pool
To avoid constant writes to the datadisks, change the system dataset pool to the bootdisk
1. System Settings > Advanced > System Dataset Pool > Configure
2. Change the pool to `boot-pool`