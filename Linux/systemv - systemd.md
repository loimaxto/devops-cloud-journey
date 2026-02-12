**systemd** is default init system in linux
1. systemv
	- startup in order
2. systemd ( modern)
	- multiple process
	- multiple users


| runlevel-systemv | systemd  | description |
| ---------------- | -------- | ----------- |
| 0                | poweroff |             |
| 1                | recue    |             |
| 2                |          |             |
| 3                |          |             |
| 4                |          |             |
| 5                |          |             |
| 6                |          |             |

---
### Command
```bash
systemctl enable # automatically start  service
systemctl start # manually
systemctl stop
systemctl status


```