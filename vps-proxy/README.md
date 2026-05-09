# VPS proxy setup

## Configure a Wireguard tunnel:
1. Install `wireguard-tools` package;
2. Prepare WireGuard config file:
```bash
umask 077 && printf "[Interface]\nPrivateKey = " | sudo tee /etc/wireguard/wg0.conf > /dev/null
```
3. Generate a public and private key pair:
```bash
wg genkey | sudo tee -a /etc/wireguard/wg0.conf | wg pubkey | sudo tee /etc/wireguard/publickey
```
4. Configure WireGuard server:
```ini
[Interface]
PrivateKey = <YOUR_VPS_PRIVATE_KEY>
ListenPort = 51820
Address = 192.168.3.1/24

[Peer]
PublicKey = <YOUR_HOME_SERVER_PUBLIC_KEY>
AllowedIPs = 192.168.3.2/32
```
5. Configure WireGuard client:
```ini
[Interface]
PrivateKey = <YOUR_HOME_SERVER_PRIVATE_KEY>
Address = 192.168.3.2/24

[Peer]
PublicKey = <YOUR_VPS_PUBLIC_KEY>
AllowedIPs = 192.168.3.1/32
Endpoint = <VPS_PUBLIC_IP:51820>
PersistentKeepalive = 25
```
6. Enable and start WireGuard configurer on both server and client:
```bash
sudo systemctl enable --now wg-quick@wg0
```
7. Verify the connection:
```bash
sudo wg show
```

## Configure HAProxy
**Note**: On SELinux enabled distributions HAProxy can only bind to ports that are tagged as `http_port_t`.

Add non-traditional HTTP ports:
```bash
sudo semanage port -t http_port_t -p tcp -a <YOUR_CUSTOM_HTTP_PORT>
```
