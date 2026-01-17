# My self-hosted services
## Tools:
- Caddy (first-class HTTPS support, very simple configuration)
- Fedora CoreOS (SELinux, immutable, container oriented)

## Replacements
Replace example.com in the configurations with your domain:
```sh
cd self-hosted
YOUR_DOMAIN='example.com'
find . -type f -exec sed -i "s/example.com/$YOUR_DOMAIN/g" {} \;
```
Set Vaultwarden admin token:
```sh
echo -n "MySecretPassword" | argon2 "$(openssl rand -base64 32)" -e -id -k 65540 -t 3 -p 4 | sed 's#\$#\$\$#g' | read VAULTWARDEN_ADMIN_TOKEN
sed -i "s/VAULTWARDEN_ADMIN_TOKEN/$VAULTWARDEN_ADMIN_TOKEN/g"
```

## Caddy
1. Install `caddy`:
```sh
rpm-ostree install caddy
```
2. Enable and start `caddy`:
```sh
systemctl enable --now caddy
```

## Podman secrets
Secrets allow us to avoid hardcoding credentials in a file and pushing it to a git server eventually.

Create a secret:
```sh
read -s SECRET
echo -n "$SECRET" | podman secret create SECRET_NAME -
```

## Podman Quadlets
Populate `~/.config/containers/systemd` as necessary and run:
```sh
systemctl --user daemon-reload
```
---
To make sure the containers are not stopped when logging out, run:
```sh
loginctl enable-linger mohsun
```
