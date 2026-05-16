---
title: Linux and Networking
layout: default
parent: SRE
nav_order: 50
---

# Linux and Networking

## Areas

- Processes and signals
- Filesystems and disk pressure
- Systemd services
- DNS
- TCP and HTTP debugging
- TLS certificates
- Firewalls and routing

## Useful commands to document

```bash
systemctl status <service>
journalctl -u <service> --since "1 hour ago"
ss -tulpn
dig <name>
curl -v https://example.com
openssl s_client -connect example.com:443 -servername example.com
```
