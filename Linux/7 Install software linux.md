# Ubuntu
## 1. APT (Advanced Package Tool)

APT is the standard package manager for Debian-based systems like Ubuntu. It installs `.deb` packages from official repositories.

### Command Cheatsheet

| **Action**           | **Command**                       |
| -------------------- | --------------------------------- |
| Update package list  | `sudo apt update`                 |
| Upgrade all software | `sudo apt upgrade`                |
| Install a package    | `sudo apt install <package_name>` |
| Remove a package     | `sudo apt remove <package_name>`  |
| Search for a package | `apt search <keyword>`            |
| Clean package        | `sudo apt autoremove --purge`     |

---

## 2. Snap Packages

Developed by Canonical (the makers of Ubuntu), Snaps are containerized software packages that include all required dependencies.

- **Best for:** Modern desktop applications (Spotify, Slack, VS Code).
    
- **Pros:** Always up-to-date, works across different Linux distros, and runs in a secure "sandbox."
    

### Command Cheatsheet

|**Action**|**Command**|
|---|---|
|Install a snap|`sudo snap install <package_name>`|
|Remove a snap|`sudo snap remove <package_name>`|
|List installed snaps|`snap list`|
|Update all snaps|`sudo snap refresh`|

---

## 3. Flatpak

Flatpak is similar to Snap—a universal packaging system—but it is community-driven and focuses heavily on the Linux desktop ecosystem.

> **Note:** You must install the Flatpak framework first: `sudo apt install flatpak`.

- **Best for:** Getting the latest versions of GUI applications not yet in the official Ubuntu repos.
    
- **Pros:** Decentralized (primarily via [Flathub](https://flathub.org/)), excellent permissions management.
    

### Command Cheatsheet

|**Action**|**Command**|
|---|---|
|Add Flathub repo|`flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo`|
|Install an app|`flatpak install flathub <app_id>`|
|Run an app|`flatpak run <app_id>`|
|Update apps|`flatpak update`|

---

## 4. PPA (Personal Package Archives)

PPAs are unique repositories hosted on Launchpad. They allow developers to distribute software directly to Ubuntu users.

- **Warning:** Only add PPAs from sources you trust, as they are not vetted by Ubuntu.
    

### Installation Flow

1. **Add the repo:** `sudo add-apt-repository ppa:user/repo-name`
    
2. **Update:** `sudo apt update`
    
3. **Install:** `sudo apt install <package_name>`
    

---

## 5. AppImage

AppImages are "portable" Linux apps. They are single files that run without "installing" into the system directories.

- **How to use:**
    
    1. Download the `.AppImage` file.
        
    2. Right-click -> **Properties** -> **Permissions** -> Check **"Allow executing file as program."**
        
    3. Double-click to run.
        

---

## 6. Installing via `.deb` Files

Sometimes you download a package directly from a website (like Google Chrome or Discord).

- **Command:** `sudo apt install ./package_name.deb`
    
- _Tip: Using `apt` to install local files is better than `dpkg` because it automatically fetches missing dependencies._

## 7. Installing via `.tar.gz` (Manual Install)

This method is used when software is provided as a compressed archive rather than a package.

### Step-by-Step Installation

1. Extract the file:
    
    It is best practice to move these files to the /opt directory (intended for optional/third-party software).
	//Replace   'ideaIC-xxx.tar.gz' with your actual filename
    sudo tar -xzf ~/Downloads/ideaIC-*.tar.gz -C /opt/

1. Run the application:
    ./idea.sh

---
## 8 Extract file in linux
install tools : 

```bash
sudo apt install p7zip-ful
7z x filename.7z
```
### Command Cheatsheet (Manual)

|**Task**|**Command**|
|---|---|
|Extract archive|`tar -xzf <file>.tar.gz`|
|Extract to specific folder|`sudo tar -xzf <file>.tar.gz -C /opt/`|
|Run script|`./filename.sh`|
|Create Symlink (Optional)|`sudo ln -s /opt/folder/bin/idea.sh /usr/local/bin/idea`|

---

> 💡 Pro Tip for JetBrains Users:
> 
> If you find manual updates annoying, JetBrains provides the Toolbox App.1 You download it as a .tar.gz, run it once, and it handles the installation, updates, and shortcuts for all JetBrains IDEs automatically.

---

## Summary Comparison

|**Feature**|**APT**|**Snap**|**Flatpak**|**AppImage**|
|---|---|---|---|---|
|**Isolation**|None|High (Sandboxed)|High (Sandboxed)|Low|
|**Updates**|System-wide|Automatic|Manual/Scheduled|Manual|
|**Size**|Small|Large|Large|Medium|
|**Ease of Use**|Moderate|Easy|Easy|Easiest|

---
# Update
## APT
```bash
sudo apt update
apt list --upgradable | grep code
sudo apt install --only-upgrade code
# check current version
apt-cache policy code

```

# Delete
```bash
sudo apt purge openjdk-8-jdka # remove config history and apps
sudo apt autoremove && sudo apt autoclean # delete installed .deb

#remove binary file
sudo rm /usr/local/bin/starship
```