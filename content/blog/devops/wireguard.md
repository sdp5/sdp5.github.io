---
title: Multi-Client WireGuard VPN Server
type: blog
date: 2024-12-08 17:10:08 +0530
prev: blog/devops/
sidebar:
  open: true
---

WireGuard is a modern, fast, and secure VPN protocol that’s rapidly gaining popularity due to its simplicity and strong cryptography. In this guide, we’ll walk through setting up a WireGuard VPN server on Ubuntu, and more specifically, how to configure multiple clients on a single WireGuard interface—a common scenario for remote access or multi-device setups.

This post draws from the excellent Linuxiac guide and expands on it with real-world experience and a functioning example involving two clients on one WireGuard server.

### Understanding the Basics

WireGuard is built into the Linux kernel and behaves much like a standard network interface. When you create a WireGuard device (`wg0`), think of it as a virtual Ethernet interface. You do not create `wg1`, `wg2`, etc., for each client. Instead, **all peers connect to the same interface**, and each client has a unique internal VPN IP address.

### Network Design

Let’s assume this WireGuard VPN network:

- WG Subnet: `10.28.5.0/29` (6 usable IPs)
- Server IP: `10.28.5.1`
- Client 1 IP: `10.28.5.2` (e.g., a laptop)
- Client 2 IP: `10.28.5.3` (e.g., a phone)

Each peer uses `/32` to isolate routing to that specific peer. The server uses /29 so it can talk to all clients.

Additionally, clients route their entire traffic through the VPN using AllowedIPs = 0.0.0.0/0, while the server can route traffic back to the clients' LANs, such as 192.168.29.0/24 or 192.168.19.0/24.

### Server Configuration (`server_wg0.conf`)

```bash
[Interface]
Address = 10.28.5.1/29
ListenPort = 51820
PrivateKey = <wireguard-server-privatekey>
SaveConfig = true

PreUp = sysctl -w net.ipv4.ip_forward=1
PostUp = iptables -A FORWARD -i wg0 -j ACCEPT; iptables -A FORWARD -o wg0 -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
PostDown = iptables -D FORWARD -i wg0 -j ACCEPT; iptables -D FORWARD -o wg0 -j ACCEPT; iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE

[Peer]
PublicKey = <wireguard-laptop-public-key>
PresharedKey = <wireguard-presharedkey>
AllowedIPs = 10.28.5.2/32, 192.168.29.0/24

[Peer]
PublicKey = <wireguard-phone-public-key>
PresharedKey = <wireguard-presharedkey>
AllowedIPs = 10.28.5.3/32, 192.168.19.0/24
```

Note: Only use `[Peer]`, not `[Peer1]`, `[Peer2]`, etc.—WireGuard doesn't support named peer blocks. Each peer is identified by its unique public key.

### Client 1 Configuration (`laptop_wg0.conf`)

```bash
[Interface]
Address = 10.28.5.2/32
PrivateKey = <wireguard-laptop-private-key>
DNS = <server-dns-ip-address>

[Peer]
PublicKey = <wireguard-server-public-key>
PresharedKey = <wireguard-presharedkey>
Endpoint = <server-ip-address>:51820
AllowedIPs = 0.0.0.0/0
PersistentKeepalive = 15
```

### Client 2 Configuration (`phone_wg0.conf`)

```bash
[Interface]
Address = 10.28.5.3/32
PrivateKey = <wireguard-phone-private-key>
DNS = <server-dns-ip-address>

[Peer]
PublicKey = <wireguard-server-public-key>
PresharedKey = <wireguard-presharedkey>
Endpoint = <server-ip-address>:51820
AllowedIPs = 0.0.0.0/0
PersistentKeepalive = 15
```

### Routing & Communication

- The server uses `/29` in its `[Interface]` to talk to all peers.
- Each client uses `/32` to only receive packets specifically routed to them.
- Clients are configured with `/29` if they need to **talk to each other**. If peer-to-peer communication should be restricted, use firewall rules on the server.

For example, with these firewall rules, you can allow or block clients from accessing each other's LANs or VPN IPs.

### Advanced Notes

- `PersistentKeepalive = 15` or `24` is recommended for clients behind NAT or CGNAT. This ensures the connection stays alive and firewall/NAT mappings remain open.
- With `SaveConfig = true`, WireGuard may rewrite your config file based on runtime updates, such as public endpoint changes for roaming clients.
- If your clients roam (e.g., mobile phone), it’s okay to omit `Endpoint` on the server—WireGuard will auto-learn it from incoming packets.

### Debugging Tips

- Use `sudo wg show` to inspect live peer info and handshake status.
- If a peer never shows up, check firewall/NAT rules, the correct usage of IP subnets, and whether `AllowedIPs` is configured correctly on both ends.
- Verify `iptables` rules are active using `sudo iptables -L -v -n`.

### Security Best Practices

- Always use **Preshared Keys** in addition to public/private key pairs.
- Use a **strong DNS resolver** or internal DNS server to prevent leaks.
- Limit `AllowedIPs` to only what's needed. Avoid using wide ranges unless you route all traffic deliberately.

### Conclusion

WireGuard is a lightweight yet powerful VPN solution. Setting up multiple clients on a single WireGuard server is straightforward once you understand that all peers share the same `wg0` interface and are uniquely identified by their keys and IPs.

This architecture scales easily—just increment IP addresses within your chosen subnet and add new `[Peer]` sections to the server's config.

You can find the original guide for a fresh Ubuntu install here on Linuxiac, but this guide should help you run a real-world, production-grade setup with multiple clients and routing to client-side LANs.

**Happy tunneling!** 
