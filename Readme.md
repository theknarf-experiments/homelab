# Ansible setup for my homelab

Install Ansible and direnv, and then to test the setup run:

```
ansible proxmox -m "ping"
ansible proxmox -a "cat /etc/os-release"
```

Now to run a playbook, ex. to install Vim run the following:

```
ansible-playbook src/vim.yml
```

You may need to install some ansible-galaxy collections, run:

```
ansible-galaxy install -r requirements.yml
```

# Windows WinRM

To connect to windows using Ansible you need WinRM working.

1. Install the Qemu guest agent inside the Windows VM (https://pve.proxmox.com/wiki/Qemu-guest-agent)
   This will make it so that the Proxmox host can windows its own IP, that you'll later use to connect to it using Ansible over WinRM

2. Configure up WinRM.

2.1. Ensure that network is set to "private" and not "public"

2.2. Open up WinRM in the firewall

2.3. In PowerShell with admin run the following:

```
Add-LocalGroupMember -Group "Remote Management Group" -Member "Knarf"
winrm quickconfig
winrm set winrm/config/service/auth @{Basic="true"}
winrm set winrm/config/service "@{AllowUnencrypted=”true”}’
winrm configSDDL default
```

3. Verify setup:

```
ansible windows -m win_ping -vvvv --ask-pass
```
