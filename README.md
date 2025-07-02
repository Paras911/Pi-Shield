# Pi-Shield

# Pi-hole + PiVPN Home Network Setup Guide

This guide walks you through creating a virtualized Ubuntu Server with Pi-hole and WireGuard VPN (via PiVPN) to block ads at the DNS level and enable secure remote access. By integrating Pi-hole with WireGuard, you can enjoy ad-blocking even when you're away from your local network.

**Target Audience:** Intermediate users with basic Linux and networking knowledge.

---

## Step 1: Create a Virtualized Ubuntu Server (VirtualBox)

- Enable virtualization in BIOS.
- Install VirtualBox and download the latest Ubuntu Server ISO.
- Create a new virtual machine:
  - OS: Ubuntu (64-bit)
  - Memory: 1-2 GB
  - Disk: 10-20 GB dynamically allocated
- Networking setup:
  - Go to Settings > Network > Adapter 1
  - Enable and set Attached to: Bridged Adapter
- Start the VM and install Ubuntu Server.
- Create a user account, set hostname, and complete the installation.

---

## Step 2: Assign a Static IP Address

Assigning a static IP ensures the server is reachable at the same address consistently.

- Recommended method: DHCP reservation in router settings:
  - Log into your router (e.g., http://192.168.1.1)
  - Find connected devices and locate your VM by MAC address
  - Bind a static IP (e.g., 192.168.1.250)

---

## Step 3: Install and Configure Pi-hole

Pi-hole acts as a network-wide DNS-level ad blocker.

### Install Pi-hole:
```bash
curl -sSL https://install.pi-hole.net | bash
```

### During setup:
- Choose the default interface (e.g., eth0).
- Select Upstream DNS provider (e.g., Cloudflare or Google).
- Enable blocklists (default is fine).
- Choose static IP (use the one set via router).
- Set web admin interface and log options.

### Access Pi-hole dashboard:
http://your_server_ip/admin
(Default password provided at end of install or use `pihole -a -p` to reset)

---

## Step 4: Install WireGuard VPN with PiVPN

PiVPN simplifies WireGuard VPN installation.

### Install PiVPN:
```bash
curl -L https://install.pivpn.io | bash
```

### During setup:
- Choose WireGuard.
- Confirm static IP.
- Use default port 51820 (UDP).
- Choose a DNS provider (e.g., Cloudflare).
- When prompted, select Use Custom DNS and enter: 127.0.0.1 (to use Pi-hole)

This integrates Pi-hole with the VPN so all clients get DNS-level ad blocking even when remote.

---

## Step 5: Port Forwarding for VPN Access

To connect from outside your network, forward the WireGuard port.

- Access your router settings.
- Go to Port Forwarding / NAT / Virtual Server settings.
- Add a new rule:
  - Protocol: UDP
  - External & Internal Port: 51820
  - Internal IP: Your Ubuntu Server static IP
  - Save and reboot if necessary

---

## Step 6: Add VPN Clients

Add a new client using:
```bash
pivpn add
```
Follow prompts to generate the client config.

Export the client config file (e.g., .conf or QR code) and import it into your mobile or desktop WireGuard app.

---

## Step 7: Test Everything

1. Connect to VPN using client config.
2. Visit Pi-hole dashboard to verify DNS queries from VPN clients.
3. Try browsing websites to check for ad blocking.

---

## Summary

You now have a virtualized Ubuntu Server running:
- Pi-hole for network-wide ad blocking
- WireGuard (via PiVPN) for encrypted remote access
- Router static IP and port forwarding for stability

With this setup, all your devices—at home and remotely—can benefit from ad-free browsing through your secure DNS gateway.
