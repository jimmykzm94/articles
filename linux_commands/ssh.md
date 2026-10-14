# Linux Command Cheatsheet: `ssh`

`ssh` securely connects to remote machines over a network.

It is commonly used for:

- remote shell access
- running remote commands
- port forwarding
- tunneling
- secure administration

## Basic Syntax

```bash
ssh [options] user@host
```

Example:

```bash
ssh user@host
```

---

## Connect to a Remote Host

```bash
ssh user@host
```

Using a hostname:

```bash
ssh user@example.com
```

Using an IP address:

```bash
ssh user@192.0.2.10
```

`192.0.2.0/24` is reserved for documentation and examples.

---

## Connect with Current Username

If the remote username is the same as the local username:

```bash
ssh host
```

---

## Specify Username

```bash
ssh -l user host
```

Equivalent to:

```bash
ssh user@host
```

---

## Specify SSH Port

SSH normally uses port `22`.

Connect to another port:

```bash
ssh -p 2222 user@host
```

Generic form:

```bash
ssh -p <port> user@host
```

---

## Run a Remote Command

```bash
ssh user@host "command"
```

Example:

```bash
ssh user@host "uname -a"
```

Check disk usage:

```bash
ssh user@host "df -h"
```

Check uptime:

```bash
ssh user@host "uptime"
```

---

## Run Multiple Remote Commands

```bash
ssh user@host "command1; command2; command3"
```

Example:

```bash
ssh user@host "uptime; free -h; df -h"
```

Run the next command only if the previous one succeeds:

```bash
ssh user@host "cd /path/to/project && ls -lh"
```

---

## Use SSH Key Authentication

Specify a private key:

```bash
ssh -i ~/.ssh/id_ed25519 user@host
```

Custom key:

```bash
ssh -i /path/to/private_key user@host
```

---

## Generate an SSH Key

Recommended modern key type:

```bash
ssh-keygen -t ed25519
```

RSA:

```bash
ssh-keygen -t rsa -b 4096
```

Keys are commonly stored under:

```text
~/.ssh/
```

Example:

```text
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
```

---

## Copy Public Key to Server

```bash
ssh-copy-id user@host
```

Custom SSH port:

```bash
ssh-copy-id -p 2222 user@host
```

Afterward:

```bash
ssh user@host
```

can usually authenticate using the SSH key.

---

## Verbose Debugging

```bash
ssh -v user@host
```

More detail:

```bash
ssh -vv user@host
```

Maximum common verbosity:

```bash
ssh -vvv user@host
```

Useful for troubleshooting:

- authentication failures
- key selection
- DNS issues
- connection problems
- SSH configuration

---

## Connection Timeout

```bash
ssh -o ConnectTimeout=10 user@host
```

Stops waiting after approximately 10 seconds if the connection cannot be established.

---

## Keep Connection Alive

```bash
ssh -o ServerAliveInterval=60 user@host
```

Send a keepalive every 60 seconds.

Limit missed responses:

```bash
ssh \
  -o ServerAliveInterval=60 \
  -o ServerAliveCountMax=3 \
  user@host
```

---

## Force Public-Key Authentication

```bash
ssh -o PasswordAuthentication=no user@host
```

---

## Force Password Authentication

```bash
ssh \
  -o PreferredAuthentications=password \
  -o PubkeyAuthentication=no \
  user@host
```

---

## SSH Configuration File

User configuration:

```text
~/.ssh/config
```

Example:

```text
Host dev
    HostName example.com
    User user
    Port 2222
    IdentityFile ~/.ssh/id_ed25519
```

Then connect with:

```bash
ssh dev
```

Instead of:

```bash
ssh -p 2222 -i ~/.ssh/id_ed25519 user@example.com
```

---

## Multiple SSH Profiles

```text
Host dev
    HostName dev.example.com
    User user

Host production
    HostName production.example.com
    User user
    IdentityFile ~/.ssh/id_ed25519
```

Connect with:

```bash
ssh dev
```

or:

```bash
ssh production
```

---

## System-Wide Configuration

SSH client configuration:

```text
/etc/ssh/ssh_config
```

SSH server configuration:

```text
/etc/ssh/sshd_config
```

---

# Port Forwarding

## Local Port Forwarding

Syntax:

```bash
ssh -L local_port:destination_host:destination_port user@host
```

Example:

```bash
ssh -L 8080:localhost:80 user@host
```

Then access locally through:

```text
localhost:8080
```

Traffic flow:

```text
Local machine
    ↓
SSH connection
    ↓
Remote destination
```

---

## Forward to Another Remote Host

```bash
ssh -L 3307:database.example.com:3306 user@gateway.example.com
```

Local application connects to:

```text
localhost:3307
```

Traffic is forwarded through the SSH gateway to:

```text
database.example.com:3306
```

---

## Remote Port Forwarding

Syntax:

```bash
ssh -R remote_port:local_host:local_port user@host
```

Example:

```bash
ssh -R 8080:localhost:3000 user@host
```

This forwards a remote-side port to a service on the local machine.

---

## Dynamic Port Forwarding

Create a SOCKS proxy:

```bash
ssh -D 1080 user@host
```

Applications can then use:

```text
SOCKS5
localhost:1080
```

---

## Tunnel Without Remote Shell

Use `-N`:

```bash
ssh -N -L 8080:localhost:80 user@host
```

`-N` means no remote command or shell is started.

---

## Run Tunnel in Background

```bash
ssh -f -N -L 8080:localhost:80 user@host
```

Common shorthand:

```bash
ssh -fN -L 8080:localhost:80 user@host
```

---

## Bind Local Tunnel to Localhost

```bash
ssh -L 127.0.0.1:8080:localhost:80 user@host
```

This keeps the forwarded port accessible only locally.

---

## Bind Tunnel to All Interfaces

```bash
ssh -L 0.0.0.0:8080:localhost:80 user@host
```

This may expose the forwarded service to other machines on the network.

Use carefully.

---

# Jump Hosts

## Connect Through a Jump Host

```bash
ssh -J user@gateway user@destination
```

Example structure:

```text
Local machine
    ↓
Gateway / Jump Host
    ↓
Destination
```

---

## Jump Host with Custom Port

```bash
ssh -J user@gateway:2222 user@destination
```

---

## Multiple Jump Hosts

```bash
ssh -J user@jump1,user@jump2 user@destination
```

---

## Configure Jump Host

```text
Host destination
    HostName destination.example.com
    User user
    ProxyJump gateway

Host gateway
    HostName gateway.example.com
    User user
```

Then:

```bash
ssh destination
```

---

# Terminal Options

## Force Pseudo-Terminal

```bash
ssh -t user@host
```

Useful for commands requiring an interactive terminal:

```bash
ssh -t user@host "sudo command"
```

Force terminal allocation more strongly:

```bash
ssh -tt user@host
```

---

## Disable Pseudo-Terminal

```bash
ssh -T user@host
```

Useful for automation and non-interactive commands.

---

## Enable Compression

```bash
ssh -C user@host
```

Can help on slow links when transferring compressible data.

---

# X11 Forwarding

Enable X11 forwarding:

```bash
ssh -X user@host
```

Trusted X11 forwarding:

```bash
ssh -Y user@host
```

Requires compatible X11 support on both systems.

---

# SSH Agent

## Start SSH Agent

```bash
eval "$(ssh-agent -s)"
```

---

## Add Private Key

```bash
ssh-add ~/.ssh/id_ed25519
```

---

## List Loaded Keys

```bash
ssh-add -l
```

---

## Remove a Key

```bash
ssh-add -d ~/.ssh/id_ed25519
```

---

## Remove All Keys

```bash
ssh-add -D
```

---

## Agent Forwarding

```bash
ssh -A user@host
```

This allows the remote machine to use keys from your local SSH agent.

Use only with machines you trust.

---

## Disable Agent Forwarding

```bash
ssh -a user@host
```

---

# Known Hosts

SSH stores known host keys in:

```text
~/.ssh/known_hosts
```

When connecting to a host for the first time, SSH asks you to verify its host-key fingerprint.

Always verify unfamiliar fingerprints when security matters.

---

## Remove Stored Host Key

```bash
ssh-keygen -R example.com
```

Using an IP address:

```bash
ssh-keygen -R 192.0.2.10
```

Non-default port:

```bash
ssh-keygen -R "[example.com]:2222"
```

---

## Automatically Accept New Hosts

```bash
ssh -o StrictHostKeyChecking=accept-new user@host
```

This automatically accepts previously unseen hosts but still rejects changed host keys.

---

## Disable Host-Key Checking

```bash
ssh -o StrictHostKeyChecking=no user@host
```

This weakens protection against man-in-the-middle attacks.

Avoid it unless the environment specifically requires it.

---

# Scripts and Pipes

## Run Local Script Remotely

```bash
ssh user@host 'bash -s' < script.sh
```

Pass arguments:

```bash
ssh user@host 'bash -s -- arg1 arg2' < script.sh
```

---

## Pipe Data to Remote Host

```bash
cat file.txt | ssh user@host "cat > /tmp/file.txt"
```

---

## Retrieve Remote Command Output

```bash
ssh user@host "cat /path/to/file" > local-file
```

---

## Stream `tar` over SSH

Send a directory:

```bash
tar -czf - directory/ |
  ssh user@host "tar -xzf -"
```

Create a local archive from a remote directory:

```bash
ssh user@host "tar -czf - directory/" > archive.tar.gz
```

---

# SSH Escape Commands

During an interactive session, SSH supports escape commands after a newline.

Terminate a stuck session:

```text
~.
```

Show available escape commands:

```text
~?
```

Suspend the SSH client:

```text
~Ctrl+Z
```

---

## Exit SSH Session

```bash
exit
```

Or:

```text
Ctrl+D
```

---

# Information and Diagnostics

## Show SSH Version

```bash
ssh -V
```

`-V` is uppercase.

---

## Show Effective Configuration

```bash
ssh -G host
```

Example:

```bash
ssh -G host | grep hostname
```

Useful for debugging settings inherited from:

```text
~/.ssh/config
/etc/ssh/ssh_config
```

---

## Use Custom Configuration File

```bash
ssh -F /path/to/config user@host
```

Ignore normal configuration:

```bash
ssh -F /dev/null user@host
```

---

## Common Options

| Option | Description |
|---|---|
| `-p PORT` | Specify SSH port |
| `-i FILE` | Specify private key |
| `-l USER` | Specify username |
| `-v` | Verbose debugging |
| `-C` | Enable compression |
| `-N` | Do not execute remote command |
| `-f` | Run SSH in background |
| `-t` | Force pseudo-terminal |
| `-T` | Disable pseudo-terminal |
| `-A` | Enable agent forwarding |
| `-a` | Disable agent forwarding |
| `-X` | Enable X11 forwarding |
| `-Y` | Enable trusted X11 forwarding |
| `-L` | Local port forwarding |
| `-R` | Remote port forwarding |
| `-D` | Dynamic SOCKS proxy |
| `-J` | Use jump host |
| `-F FILE` | Use custom config file |
| `-o OPTION` | Set SSH option |
| `-V` | Show SSH version |

---

## Common `-o` Options

Connection timeout:

```bash
ssh -o ConnectTimeout=10 user@host
```

Keepalive:

```bash
ssh -o ServerAliveInterval=60 user@host
```

Maximum missed keepalives:

```bash
ssh -o ServerAliveCountMax=3 user@host
```

Accept only new host keys:

```bash
ssh -o StrictHostKeyChecking=accept-new user@host
```

Disable password authentication:

```bash
ssh -o PasswordAuthentication=no user@host
```

Multiple options:

```bash
ssh \
  -o ConnectTimeout=10 \
  -o ServerAliveInterval=60 \
  user@host
```

---

## Common Command Patterns

```bash
# Connect
ssh user@host

# Custom port
ssh -p 2222 user@host

# Specify private key
ssh -i ~/.ssh/id_ed25519 user@host

# Run remote command
ssh user@host "uname -a"

# Debug connection
ssh -vvv user@host

# Keep connection alive
ssh -o ServerAliveInterval=60 user@host

# Local port forwarding
ssh -L 8080:localhost:80 user@host

# Remote port forwarding
ssh -R 8080:localhost:3000 user@host

# SOCKS proxy
ssh -D 1080 user@host

# Tunnel only
ssh -N -L 8080:localhost:80 user@host

# Background tunnel
ssh -fN -L 8080:localhost:80 user@host

# Jump host
ssh -J user@gateway user@destination

# Force remote terminal
ssh -t user@host "sudo command"

# Show effective configuration
ssh -G host

# Show SSH version
ssh -V
```

## Important Notes

- SSH normally uses TCP port `22`.
- Public-key authentication is generally preferred over passwords.
- Never share private SSH keys.
- Verify server host-key fingerprints before trusting unfamiliar hosts.
- `-L` creates local port forwarding.
- `-R` creates remote port forwarding.
- `-D` creates a SOCKS proxy.
- `-N` is useful when SSH is used only as a tunnel.
- `-J` connects through a jump or bastion host.
- `ssh -vvv` is useful for diagnosing connection and authentication problems.
- Use `~/.ssh/config` to simplify frequently used SSH connections.
- Keep examples and automation generic rather than hard-coding environment-specific usernames, IP addresses, or hostnames.