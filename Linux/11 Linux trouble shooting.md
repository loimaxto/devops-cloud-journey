## Boot Failures:
* Use **dmesg** and **journalctl** to check kernel logs for errors during boot.
* Look for error messages related to hardware or missing files.
* Example: **journalctl -b** to see logs from the latest boot.

## Disk Space Issues:
* Use **df -h** to check available disk space:
    * Identify which partitions are filling up.
* Use **du -sh /path** to find large files or directories.

## System Crashes:
* Check for OOM (Out of Memory) errors using **dmesg** or **journalctl**.
* Look in **/var/log/messages** or **/var/log/syslog** for panic or segfault entries.
```bash
journalctl | grep -i "oom"
dmesg
```