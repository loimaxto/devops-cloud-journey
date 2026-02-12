temporary storage support ram
### why is this important
- help system work consistantly

```bash
fallocate -l 1G /swapfile
mkswap /swapfile



sudo chmod 0600 swapfile # only owner can read
sudo swapoff /swapfile-name # unmount swap file
sudo swapon /swapfile # print swap info

mkdiw ~/swap-test
cd ~/swap-test
sudo fallocate -l 1G ./swapfile
ls # result show swapfile

free -h # show infor
```