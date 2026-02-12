### setup network

#### 1 vmware
root
- vi /etc/netplan/00-installer-config.yaml
- only 1 file yaml in /etc/netplan

```yaml
network:
	ethernets:
		ens33:
			dhcp: false
			addresses: [122.122.1.100/24]
			gateway4: 192.164.192.199 #Like nat gateway/ use router for bridge
			routes:
		        - to: default
		          via: 192.168.1.1
			nameservers:
				addresses: [8.8.8.8, 8.8.8.4]
```


#### 2 bare machine

```bash
network:
  version: 2
  renderer: networkd
  wifis:
    wlan0:             # Replace with your interface name
      dhcp4: no
      addresses:
        - 192.168.1.100/24    # Your desired Static IP / Subnet Mask
      routes:
        - to: default
          via: 192.168.1.1    # Your Gateway/Router IP
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4] # DNS servers (Google DNS)
      access-points:
        "Your-SSID-Name":     # Your Wi-Fi Network Name
          password: "Your-Password"
```


apply
```bash
sudo netplan try # test connect
sudo netplan apply # use it
ip  addr show wlan0
```
### setup hostnamer

nano /etc/hostname
netplan apply
command
	"ip a" : show network info
### Cursor movement in terminal 
- **`Ctrl + A`**: Move the cursor to the beginning of the line.
- **`Ctrl + E`**: Move the cursor to the end of the line.
- **`Ctrl + B`** or **Left Arrow**: Move the cursor backward one character.
- **`Ctrl + F`** or **Right Arrow**: Move the cursor forward one character.
- **`Alt + B`** or **`Ctrl + Left Arrow`**: Move backward one word.
- **`Alt + F`** or **`Ctrl + Right Arrow`**: Move forward one word.
- **`Ctrl + XX`**: Toggle between the start of the line and the current cursor
- **CTRL+U** to erase everything from the current cursor position to the beginning of the line.
- **CTRL+K** erases everything from the current cursor position to the end of the line.