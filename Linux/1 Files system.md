
# List some files system

- **/home** : user's home directory
- **/bin** : binaries for user commands
- **/sbin** : binaries command for super user
- **/lib** : shared libraries that executables from /bin or /sbin
- **/urs** : user directory
- **/user/local** : available for all users  ( app split to child /bin and /lib )
- **/opt** : third-party programs ( doesn't split like **/user/local**) 
- **/etc** : configuration for system-wide
- **/boot** : files required for booting
- **/dev** : location of device files ( webcam, keyboard,...)
- **/var**  : contains files to which the system writes data during the course of it's operation
- **/var/log** : log files
- **/var/cache** : cache data from applications
- **/media** : subdirectories for removable mounted devices ( like usb, cd, ..)
- **/mnt** : temporary mount
- **/etc/hostname** : name of the server
- **/etc/fstab**: file system table( mount dev information)
- **/proc** : process storage

**/etc/hosts**
map ip and hosts name of the server


---

#### 1 File/ folder interaction
```bash
ln -s /path/to/original/file /path/to/link # create reference link soft
stat file-name # show inode: address
```

### 4 Troubleshooting
**fsck**
```bash
mount # list mounted
umoun /mnt # un mount before scan

cat /etc/fstab
vi /etc/sda1 # remove mount device line
lsblk
fsck /dev/sda1
```