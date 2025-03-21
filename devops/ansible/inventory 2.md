**Ansible Inventory Configuration**

### Overview

Ansible can work with one or multiple systems simultaneously within an infrastructure. It establishes connectivity to these systems using SSH for Linux and PowerShell Remote (WinRM) for Windows. This agent-less architecture eliminates the need to install additional software on the target machines.

### What is an Inventory File?

The inventory file in Ansible stores information about target systems. If no custom inventory file is specified, Ansible uses the default inventory file located at `/etc/ansible/hosts`.

### Inventory File Format

The inventory file follows an INI-like format, listing servers one after another. Servers can be grouped together using square brackets `[ ]`.

#### Example:

```ini
[web]
web1 server1.company.com ansible_host=server1.company.com ansible_connection=ssh

[db]
db1naem server2.company.com ansible_host=server2.company.com ansible_connection=winrm

[mail]
mail1name server3.company.com ansible_host=server3.company.com ansible_connection=ssh

[web2]
webname server4.company.com ansible_host=server4.company.com ansible_connection=winrm

[localhost]
localhost ansible_connection=localhost

#group the groups
[parent_group:children]
child_group1
child_group2
```

### Inventory Parameters

1. **ansible\_host**: Specifies the FQDN or IP address of the server.
2. **ansible\_connection**: Defines how Ansible connects to the target machine. Possible values:
   - `ssh` (for Linux servers)
   - `winrm` (for Windows servers)
   - `localhost` (for local execution)
3. **ansible\_port**: Defines the SSH/WinRM port (default: 22 for SSH, 5986 for WinRM).
4. **ansible\_user**: Specifies the remote user to use for connections (default: `root` for Linux, `Administrator` for Windows).
5. **ansible\_ssh\_pass**: Stores the SSH password (not recommended; use SSH keys instead).

### Setting Up SSH Key-Based Authentication

Using SSH keys is a best practice for secure authentication. Follow these steps to set up key-based authentication:

#### 1. Generate an SSH Key on the Control Node

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

This generates two files:

- **Private key**: `~/.ssh/id_rsa` (Keep this secure!)
- **Public key**: `~/.ssh/id_rsa.pub`

#### 2. Copy the Public Key to the Target Server

```bash
ssh-copy-id user@server1.company.com
```

This appends the public key to the target server’s `~/.ssh/authorized_keys` file.

#### 3. Test SSH Connection

```bash
ssh user@server1.company.com
```

If successful, SSH will no longer prompt for a password.

### Using SSH Keys in Ansible

In the inventory file, specify the private key:

```ini
[web]
server1.company.com ansible_user=user ansible_ssh_private_key_file=~/.ssh/id_rsa ansible_connection=ssh
```

### Testing Ansible Connectivity

Run a simple ping test:

```bash
ansible all -i inventory.ini -m ping
ansible web -i inventory.ini -m shell -a "uptime"    //shell command running

```

Expected output:

```json
server1.company.com | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

### Best Practices

✅ Use SSH keys instead of storing passwords in plain text.\
✅ Organize inventory files into logical groups.\
✅ Secure sensitive data using Ansible Vault.\
✅ Keep the inventory file updated as infrastructure changes.

---

By following these steps, you can efficiently manage your infrastructure using Ansible. Happy automating! 🚀

