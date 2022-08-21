# Dataset configuration

## Pool and dataset structure

Pools in bold, data sets underneath:
* __rapid-store__
  * apps _storage for apps (containers and pods)_
  * users _home directories for users_
  * downloads _storage for torrents, nzb etc._
* __big-store__
  * media _photos, video, ebooks, etc._
  * storage _general storage and backup_

## Setup steps
We will set this up so that all files and folders created in these data sets will get set to the group of that data set.

1. Create the pools as mirrors
2. Create the data sets as _Share Type_ `Generic`, except `users`, make that `SMB` (for now)
3. Change group ownership of the created data sets on the commandline (make sure the groups are created first, `apps` is a built-in group), e.g. `chown .media media`
* media > group `media`
* apps > group `apps`
* storage > group `storage`
* downloads > group `apps`
4. Allow group write for each of these directories except `users`, e.g. `chmod g+w media`
5. Set the Group ID bit on each of these directories except `users`, e.g. `chmod g+s media`
6. Under _Shares_, add each dataset to _Windows (SMB) Shares_. Under _Advanced Options_ > _Auxiliary Parameters_ add `force group="groupname"`, e.g. `force group="media"`
  
  


