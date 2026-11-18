# Day 4 – Linux Ports & Networking

## 1. What is Networking?
Networking allows computers and servers to communicate
with each other.

## 2. What is an IP Address?
An IP address identifies a device on a network.

Example:
192.168.1.10

## 3. What is a Port?
A port is a communication endpoint used by an application
to receive network connections.

## 4. Common Ports

| Port | Purpose |
|---|---|
| 22 | SSH |
| 80 | HTTP |
| 443 | HTTPS |
| 3000 | Common React/Node.js development port |
| 5000 | Common backend development port |
| 27017 | MongoDB |

## 5. Important Networking Terms

- TCP: Reliable data communication.
- UDP: Faster communication without delivery guarantee.
- DNS: Converts domain names into IP addresses.
- SSH: Securely connects to remote servers.
- Firewall: Controls incoming and outgoing traffic.
- Localhost: Refers to your own computer (127.0.0.1).

## 6. Useful Commands

Check IP address:
ip addr

Test network connectivity:
ping google.com

Check listening ports:
sudo ss -tulnp

Check a specific port:
sudo ss -tulnp | grep :3000

Check DNS:
nslookup google.com

Test an HTTP endpoint:
curl http://localhost:3000

## 7. SSH (Secure Shell)

SSH allows secure remote access to Linux servers.

Connect to a server:
ssh username@server-ip

Connect using a custom port:
ssh -p 2222 username@server-ip

Exit SSH session:
exit

Default SSH port: 22

## Real-World Example

If your Node.js backend runs on port 5000:

sudo ss -tulnp | grep :5000

This checks whether an application is listening
on port 5000.

## Remember

IP = Address
Port = Communication endpoint
DNS = Domain to IP
Firewall = Network security
SSH = Secure remote access