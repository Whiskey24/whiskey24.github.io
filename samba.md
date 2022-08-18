# Samba

## Troubleshooting

### User not recognized/authorized when accessing from Windows.
* Remove /var/db/system/samba4/private/secrets.tdb
* Then re-sync the users:  `midclt call smb.synchronize_passdb -job`

For some more detail, see this [thread](https://www.truenas.com/community/threads/samba-problem-with-create-user.98010/post-676257) 

