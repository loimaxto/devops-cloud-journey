1. Booting usb: https://www.loom.com/share/9eb66d3bce43444eb2e22819461a565f

Command check arch boot is existed
```bash
efibootmgr
```
### 1 Setup network
```bash
iplink
iwctl
station list
station wlan0 scan
station wlan0 show # show connected network information
station wlan0 get-network
station wlan0 connect "You network"
exit


# set fastest server download
curl -s "https://archlinux.org/mirrorlist/?country=VN&protocol=https&use_mirror_status=on" | sed 's/^#Server/Server/' > /etc/pacman.d/mirrorlist
vim /etc/pacman.conf



pacman -S networkmanager
systemctl enable --now NetworkManager
pacman -S network-manager-applet wpa_supplicant wireless_tools dialog


nmtui # terminal interface ui connect wifi
```

Static ip: [0-setup-new-vm](../0-setup-new-vm.md)
### 2 Partition disk
**Attention**
- Always encrypt disk

```bash
lsblk
fdisk /dev/sda
# option order
# m help
# g - reset new partition table
# p - print partition table
# n - new partition > list partition type choose 

# t - change partition type
# w - save setting
```
**Partion options**
n - list partitions
- 1 efi
- ? Linux filesystem ext4
- 44 Linux LVM : allow to encrypt disk
!!! **Attention**
```bash
arch-chroot /mnt # get into new system root mode (not from usb)
```

Pacman package management:  [pacman](pacman.md)

### 3 boot loader
```bash
bootctl install
bootctl update

vim loader.conf
cd entries
ls
```
create file infor
```bash
vim arch.conf

title Arch Linux
linux /vm/linuz-linux
initrd /initramfs-linux.img
options rd.luks.name=<device-UUID>=volumegroup 
# get UUID: blkid 
root=/dev/volumegroup
```

---
```bash
pstree # list component of systemd
man dbus-broker # message broker : message queue for system
linux pam # security stuff
journallctl # log used for debug system
```

### 4 ssh config
```bash
ip addr show
nmcli device wifi connect "Your_SSID" password "Your_Password"

# config ssh
systemctl status sshd.service
cd .ssh
passwd # setup new pass


# disable beep ;))
sudo nano /etc/modprobe.d/nobeep.conf 
# add this line: blacklist pcspkr

## host
ssh root@ip-get-from-show
```
### 3 Install Desktop
Wayland- xorgs , hyperland

___
Checklist

- network
- bootloader
- option boot 