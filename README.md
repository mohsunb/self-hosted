# My self-hosted services
Tools:
- Tailscale (for network access behind CGNAT)
- Caddy (reverse proxy with first-class HTTPS support)
- Fedora Linux (my preference)

Note: Before starting, replace YOUR_HOSTNAME in the configurations with your Tailscale MagicDNS domain:
```sh
sed -i 's/YOUR_HOSTNAME/$YOUR_HOSTNAME/g'
```

1. Install `caddy` and `tailscale`:
```sh
sudo dnf install caddy tailscale
```

2. Enable and start `tailscaled`:
```sh
systemctl enable --now tailscaled
```

3. Edit `caddy.service` and switch to `root` user:
```sh
sudo env EDITOR=nvim systemctl edit caddy
```
Enter:
```ini
[Service]
User=root
Group=root
```
---
If running with SELinux enforced (default on Fedora):
```bash
for i in 10001 10002 10003; do
    sudo semanage port -a -t http_port_t -p tcp $i;
    sudo semanage port -a -t http_port_t -p udp $i;
done
```

4. Enable and start `caddy`:
```sh
systemctl enable --now caddy
```
# Podman secrets
Secrets allow us to avoid hardcoding credentials in a file and pushing it to a git server eventually

Create a secret:
```sh
read -s SECRET
echo -n $SECRET | podman secret create SECRET_NAME -
```
# Podman Quadlets
Populate `~/.config/containers/systemd` as necessary and run:
```sh
systemctl --user daemon-reload
```
---
To make sure the containers are not stopped when logging out, run:
```sh
sudo loginctl enable-linger mohsun
```
