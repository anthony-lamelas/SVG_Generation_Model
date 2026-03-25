# Remote Access Setup (frienduser)


## Friend setup (The user is called frienduser)

### A. Install and sign in to Tailscale
- Install from: https://tailscale.com/download
- Sign in to the same tailnet/account.
- Confirm it works:
```bash
tailscale status
```

- Then give anthony your tailscale email so he can invite you.

### B. Create an SSH key (if they do not already have one)
macOS/Linux:
```bash
ssh-keygen -t ed25519 -C "friend-device"
cat ~/.ssh/id_ed25519.pub
```

Windows PowerShell:
```powershell
ssh-keygen -t ed25519 -C "friend-device"
type $env:USERPROFILE\.ssh\id_ed25519.pub
```

Friend sends you the single-line public key output (`.pub` key).

## Add friend's public key on host (FOR ANTHONY NOT BOLAJI)

Paste the received public key into `authorized_keys` for `frienduser`:

```bash
sudo mkdir -p /home/frienduser/.ssh
sudo nano /home/frienduser/.ssh/authorized_keys
```

Then set permissions:
```bash
sudo chown -R frienduser:frienduser /home/frienduser/.ssh
sudo chmod 700 /home/frienduser/.ssh
sudo chmod 600 /home/frienduser/.ssh/authorized_keys
```

## Friend connects

From friend device:
```bash
ssh frienduser@100.101.13.28
```


## VSCode Connect
- Make sure you have the **Remote - SSH** extension installed in VS Code.

### Update your SSH config

Use this host block:

```sshconfig
Host anthony-gpu
	HostName 100.101.13.28
	User frienduser
	IdentityFile ~/.ssh/id_ed25519
```

Save it in your SSH config file:

- Linux/macOS: `~/.ssh/config`
- Windows: `C:\Users\<your-user>\.ssh\config`

If the file does not exist, create it. 
Ask chat if your are confused.

### Test from terminal first

```bash
ssh anthony-gpu
```

If it asks to trust the host key, type `yes`.

### 3) Connect from VS Code

1. Open Command Palette (`Ctrl+Shift+P` or `Cmd+Shift+P`).
2. Run: `Remote-SSH: Connect to Host...`
3. Pick: `anthony-gpu`
4. Open the folder you want on the remote machine.