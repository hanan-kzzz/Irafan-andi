# Remote PC Shutdown

A simple guide for shutting down an Ubuntu/Linux or Windows PC remotely from another computer on the same local network.

> Only use these instructions on computers and networks you own or have permission to administer.

---

# Ubuntu / Linux

## Requirements

- Both devices must be connected to the same Wi-Fi/LAN.
- The Ubuntu PC must be powered on.
- SSH must be enabled on the Ubuntu PC.
- You need a valid Ubuntu username and password.

## 1. Enable SSH on Ubuntu

On the Ubuntu PC:

```bash
sudo apt update
sudo apt install openssh-server
sudo systemctl enable --now ssh
```

Check SSH:

```bash
systemctl status ssh
```

## 2. Find the Ubuntu PC IP address

On Ubuntu:

```bash
hostname -I
```

Example:

```text
192.168.1.105
```

You can also use:

```bash
ip addr
```

If you forgot the IP, from another Linux computer on the same network:

```bash
ip neigh
```

You can also check your router's connected-device list.

## 3. Test SSH from the laptop

Replace the username and IP:

```bash
ssh USERNAME@IP_ADDRESS
```

Example:

```bash
ssh hanan@192.168.1.105
```

## 4. Shut down the Ubuntu PC

After connecting through SSH:

```bash
sudo poweroff
```

Or:

```bash
sudo shutdown now
```

Enter the Ubuntu user's sudo password when prompted.

---

# Windows

## Requirements

- Both computers must be connected to the same Wi-Fi/LAN.
- You need an administrator account on the Windows PC.
- Remote shutdown must be allowed by Windows firewall/security settings.

## 1. Find the Windows PC IP address

On the Windows PC, open Command Prompt:

```cmd
ipconfig
```

Look for:

```text
IPv4 Address . . . . . . . . . . : 192.168.1.106
```

You can also use:

```powershell
Get-NetIPAddress -AddressFamily IPv4
```

## 2. Test the connection

From the laptop:

```bash
ping 192.168.1.106
```

Replace the IP with the Windows PC's actual IP.

## 3. Remote shutdown from Windows

From another Windows computer with the required permissions:

```cmd
shutdown /s /m \192.168.1.106 /t 0
```

Replace `192.168.1.106` with the Windows PC's IP address.

### Useful options

Shutdown:

```cmd
shutdown /s /m \IP_ADDRESS /t 0
```

Restart:

```cmd
shutdown /r /m \IP_ADDRESS /t 0
```

Abort a scheduled shutdown:

```cmd
shutdown /a
```

## 4. If Windows blocks the remote shutdown

Windows may block remote shutdown until the appropriate firewall rules and user permissions are configured.

On the Windows PC, open an **Administrator Command Prompt** and check the firewall configuration:

```cmd
netsh advfirewall firewall show rule name=all
```

For managed/home networks, make sure the network is configured appropriately and that your account has administrator rights.

If you use Windows Remote Management or another remote-management method, configure it according to Microsoft's security guidance rather than exposing remote administration directly to the internet.

---

# Finding a PC when you forgot its IP

If both computers are on the same network, you can inspect the local ARP/neighbour table.

### Linux

```bash
ip neigh
```

### Windows

```cmd
arp -a
```

You can also log in to your router and check its connected-device/DHCP client list.

---

# Troubleshooting

## Ubuntu: SSH connection refused

On Ubuntu:

```bash
sudo systemctl restart ssh
sudo systemctl status ssh
```

Check whether SSH is listening:

```bash
ss -tlnp | grep :22
```

## Ubuntu: Cannot connect

Check the firewall:

```bash
sudo ufw status
```

If UFW is enabled, allow SSH:

```bash
sudo ufw allow ssh
```

## Windows: Remote shutdown fails

Check:

- The target PC is powered on.
- Both PCs are on the same network.
- The IP address is correct.
- Your account has the required administrator permissions.
- Windows firewall/security policy allows the required remote-management traffic.
- The target PC is not blocking remote shutdown through local/group policy.

## Ping does not work

Some computers block ICMP/ping even when they are reachable. A failed ping does not always mean that the computer is offline.

---

# Security

- Use these commands only on systems you own or are authorized to administer.
- Do not expose Windows remote shutdown or SSH directly to the public internet.
- Prefer SSH keys instead of passwords for Linux where appropriate.
- Use strong passwords and keep the operating system updated.
- For remote access outside your home network, use a properly secured VPN rather than opening administration ports to the internet.

---

# Quick Reference

| System | Find IP | Remote shutdown |
|---|---|---|
| Ubuntu/Linux | `hostname -I` | `sudo poweroff` after SSH |
| Windows | `ipconfig` | `shutdown /s /m \\IP_ADDRESS /t 0` |

