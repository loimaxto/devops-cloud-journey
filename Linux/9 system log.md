
- Navigating logs in /var/log
- Using `journalctl`


**Key log files**
- /var/log/messages : system logs (non-systemd)
- /var/log/syslog: system messages/logs from services
- /var/log/auth.log: authentication and login attempts
- /var/log/dmesg: kernel ring buffer logs

### 1 journalctl

```bash
tail -n 50 file-name
tail -f /var/log # real time monitoring
tail -f /var/log/syslog | grep -i "error" # filter keywords, ignore upcase
```

**systemd journal**

```bash
journal -ex # show all logs
journalctl --since "2025-1-13" untile "2025-1-16"
journalctl -u "nginx" -f
```


### 2 logrotate
- Managing log
- Compress old logs to save space

#### command
```bash
ls /etc/logrotate.d/

vi /etc/logrotate.conf
```

---
### best practice
#### Use Compression:
* Compressing logs saves disk space, especially for logs that contain a lot of text data.
* Use **compress** in the configuration to automatically gzip old logs.

#### Keep Logs Manageable:
* Rotate logs daily for high-activity logs (e.g., web server logs).
* Use weekly or monthly rotation for less active logs.

#### Adjust Retention Based on Needs:
* For compliance: Retain logs for longer periods (e.g., **rotate 30** for a month).
* For space management: Retain fewer logs to prevent disks from filling up.

#### Monitor Log Rotation:
* Review **/var/lib/logrotate/status** to see the status of rotated logs.
* Check logs for logrotate activity in **/var/log/cron** or **/var/log/messages**.