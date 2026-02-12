
### 1. Connect Using Username & Password (Password Authentication)
```bash
ssh username@remote_host
```
- **Note**: This method requires entering your password on each connection.  
- **Security Tip**: Avoid using root for routine tasks; use sudo after logging in instead.

---

### 2. Connect Using SSH Keys (Key-Based Authentication)

#### Step 1: Generate an SSH Key Pair (on the **client** machine)
```bash
ssh-keygen -t ed25519 -C "your_email@example.com"  # Recommended: ed25519 (more secure)
# Or for legacy systems:
# ssh-keygen -t rsa -b 4096
```
- **Output Files**:
  - **Private Key**: `~/.ssh/id_ed25519` (KEEP THIS SECURE!)
  - **Public Key**: `~/.ssh/id_ed25519.pub`

#### Step 2: Add Public Key to Remote Server
**Method A: Using `ssh-copy-id` (Recommended)**
```bash
ssh-copy-id username@remote_host
```

**Method B: Manual Copy**
1. Display the public key:
   ```bash
   cat ~/.ssh/id_ed25519.pub
   ```
2. On the **remote server**, append the key to `~/.ssh/authorized_keys`:
   ```bash
   # Server terminal
   mkdir -p ~/.ssh
   chmod 700 ~/.ssh
   nano ~/.ssh/authorized_keys  # Paste the public key
   chmod 600 ~/.ssh/authorized_keys
   ```

#### Step 3: Connect Using SSH Key
```bash
ssh username@remote_host
```
- **Key Location Storage**: The server's fingerprint is stored in `~/.ssh/known_hosts` to verify identity and prevent MITM attacks.

---

## Advanced SSH Commands & Tips

### Specify a Specific Key
```bash
ssh -i ~/.ssh/id_ed25519 username@remote_host
```

### Secure Copy (SCP)
```bash
# Copy FROM local TO remote
scp local_file.txt username@remote_host:/remote/path/

# Copy FROM remote TO local
scp username@remote_host:/remote/file.txt ./local_path/
```

### Common Extra Commands

#### Change Port Number
```bash
ssh -p 2222 username@remote_host
```

#### Enable SSH Agent (Avoid Entering Passphrase Repeatedly)
```bash
# Start the agent
eval "$(ssh-agent -s)"

# Add your key
ssh-add ~/.ssh/id_ed25519
```

#### Check Connection Status
```bash
ssh -v username@remote_host  # Verbose output
```

#### Disable Password Authentication (After Setting Up Keys)
Edit `/etc/ssh/sshd_config` on the **server**:
```bash
PasswordAuthentication no
```
Then restart SSH:
```bash
sudo systemctl restart sshd
```

---

## Best Practices

1. **Use Strong Keys**: Prefer `ed25519` over RSA for better security.
2. **Permissions**:
   - `~/.ssh` directory: `700` (owner only)
   - Private key: `600` (owner only)
   - Public key: `644`
3. **Avoid Root Login**: Disable `PermitRootLogin` in `/etc/ssh/sshd_config`.
4. **Use SSH Agent**: Store keys securely and avoid passphrase prompts.
5. **Verify Host Keys**: Never accept unknown host keys in production environments.
---
### Quick Reference Cheat Sheet

| Task                     | Command                                                                 |
|--------------------------|-------------------------------------------------------------------------|
| Generate Key             | `ssh-keygen -t ed25519`                                                 |
| Copy Public Key          | `ssh-copy-id user@host`                                                 |
| Connect with Key         | `ssh user@host`                                                         |
| Specify Key File        | `ssh -i ~/.ssh/id_ed25519 user@host`                                    |
| Secure Copy              | `scp file user@host:/path/` or `scp user@host:/path/file .`            |
| Check Connection Details | `ssh -v user@host`                                                      |
| Use SSH Agent            | `eval "$(ssh-agent -s)" && ssh-add ~/.ssh/id_ed25519`                  |