# Remote Ubuntu PC Shutdown

A simple guide for shutting down an Ubuntu PC remotely from another computer on the same local network.

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

After connecting:

```bash
sudo poweroff
```

Enter the Ubuntu user's sudo password when prompted.

## Troubleshooting

### SSH connection refused

On Ubuntu:

```bash
sudo systemctl restart ssh
sudo systemctl status ssh
```

### Check the IP address

```bash
ip addr
hostname -I
```

### Test network connectivity

From the laptop:

```bash
ping IP_ADDRESS
```

### SSH uses a different port

```bash
ssh -p PORT USERNAME@IP_ADDRESS
```

## Security

Only use this on computers and networks you own or have permission to administer. Avoid exposing SSH directly to the public internet unless it is properly secured.

For convenience, SSH keys can be configured so you don't need to enter the password every time.
