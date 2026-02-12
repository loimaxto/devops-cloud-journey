# Linux User, Group, and Permission Reference

This document provides a technical overview of user management, group configuration, and the Linux permissions model.

---

## 1. Core Configuration Files
System identity and authentication details are stored in the following flat-file databases:

| File          | Description                    | Field Breakdown                                      |
| :------------ | :----------------------------- | :--------------------------------------------------- |
| `/etc/passwd` | User account metadata.         | `user:pass:UID:GID:GECOS:home:shell`                 |
| `/etc/shadow` | Secure authentication storage. | `user:hash:last_change:min:max:warn:inactive:expire` |
| `/etc/group`  | Group membership data.         | `group_name:password:GID:user_list`                  |

---

## 2. User and Group Management Commands

The following table details the utilities used to manage system identities and active sessions.

| Command   | Action                     | Use Case                                                               |
| :-------- | :------------------------- | :--------------------------------------------------------------------- |
| `adduser` | Interactive user creation. | High-level wrapper; automates home directory and skeleton files.       |
| `useradd` | Low-level user creation.   | Standard for scripting; requires `-m` flag to create home directory.   |
| `usermod` | Modify existing account.   | Used for changing shells, locking accounts, or group assignment.       |
| `id`      | Display identity details.  | Shows UID, GID, and all supplementary group memberships.               |
| `whoami`  | Display effective user.    | Confirms the current username in the active shell.                     |
| `su -`    | Switch user context.       | The `-` flag invokes a login shell, loading the target user's profile. |
| `newgrp`  | Refresh group membership.  | Log into a new group without requiring a full system logout.           |

> **Note on Safety:** When using `usermod` to add groups, always use the `-a` (append) flag with `-G`. Executing `usermod -G <group> <user>` without `-a` will remove the user from all groups not listed in the command.

---

## 3. Ownership and Access Control

Linux utilizes a Discretionary Access Control (DAC) model based on ownership and permission bits.

### 3.1 Ownership Management
Ownership is defined for a specific **User** and **Group**.

| Operation                  | Command Syntax                | Description                                                                                          |
| :------------------------- | :---------------------------- | :--------------------------------------------------------------------------------------------------- |
| **Change Owner/Group**     | `chown <user>:<group> <path>` | Updates both the user and group ownership of the specified file or directory.                        |
| **Change Group Only**      | `chgrp <group> <path>`        | Updates only the group ownership of the specified file or directory.                                 |
| **Recursive Modification** | `chmod -R <mode> <directory>` | Applies permission changes (e.g., `777`) to a directory and all its nested subdirectories and files. |

---
### 3.2 The `chmod` Command (Change Mode)
The `chmod` utility modifies the access bits of a file system object. Permissions are divided into three scopes: **User (u)**, **Group (g)**, and **Others (o)**.

`chmod u+/-/=wrx file`
#### A. Calculation Logic (Numeric Mode)

| Binary | Value | Permission | Symbol |
| :----- | :---- | :--------- | :----- |
| `100`  | **4** | Read       | `r`    |
| `010`  | **2** | Write      | `w`    |
| `001`  | **1** | Execute    | `x`    |
| `000`  | **0** | None       | `-`    |

**Common Permission Profiles:**
* **755 (`rwxr-xr-x`):** Standard for executables and directories. Owner has full access; others can read and traverse.
* **644 (`rw-r--r--`):** Standard for data files. Owner can edit; others can only read.
* **600 (`rw-------`):** Sensitive data (e.g., SSH keys). Accessible only by the owner.
#### B. Symbolic Mode
Symbolic mode uses operators to modify specific bits without calculating the total sum:
* `chmod u+x <file>`: Add execute permission for the owner.
* `chmod g-w <file>`: Remove write permission for the group.
* `chmod a+r <file>`: Add read permission for **all** (user, group, and others).


---
## 4. Advanced Group Management

This section details the utilities and methodologies used to maintain group structures and audit memberships within the system.



### 4.1 Group Administration Utilities
The following commands provide the interface for modifying group definitions and managing administrative access.

| Command    | Action                      | Use Case                                                                 |
| :--------- | :-------------------------- | :----------------------------------------------------------------------- |
| `groupadd` | Create a new group.         | Define a new security scope for project-based file sharing.              |
| `groupmod` | Modify group metadata.      | Perform administrative changes like renaming (`-n`) or changing GIDs.    |
| `groupdel` | Remove a group.             | Permanently delete a group entry from the system configuration.          |
| `gpasswd`  | Manage group members.       | A versatile tool for adding (`-a`) or removing (`-d`) users from groups. |

---

### 4.2 Membership Inspection
Auditing who belongs to which group is critical for maintaining the principle of least privilege.

#### A. Identifying Group Members
To extract a list of all users assigned to a specific group, utilize these methods:

* **Database Lookup:** `getent group <group_name>`  
    *Returns the full entry including the GID and a comma-separated list of members.*
* **Manual File Parsing:** `grep "^<group_name>" /etc/group`  
			    *Directly filters the group configuration file for the specific group line.*

#### B. Identifying User Memberships
To verify the access levels of a specific user account:

* **Standard List:** `groups <username>`  
    *Provides a simple list of all groups associated with the account.*
* **UID/GID Overview:** `id <username>`  
    *Displays the effective User ID, Primary Group ID, and Supplementary Group IDs.*
* **Primary Group Check:** `getent passwd <username>`  
    *Identifies the primary GID defined in the user's main account metadata.*