# Headscale
## Setup:
1. Fetch example configuration for your release (e.g. v0.28.0):
```bash
curl -SsfL https://raw.githubusercontent.com/juanfont/headscale/refs/tags/v0.28.0/config-example.yaml >> config.yaml
```
 **NOTE**: Following steps will modify the contents of `config.yaml`.

2. Set `server_url`;

 This is the URL used for connecting Tailscale clients to the Headscale server. Make sure to use HTTPS here (put the server behind a trusted proxy with TLS termination).
3. Configure `listen_addr` as needed;
4. Enable DERP server through `derp.server`:
 1. Set `enabled: true`;
 2. Set `ipv4` and `ipv6` to the public IP addresses pointing to your Headscale server;
5. Configure MagicDNS by setting `base_domain` under `dns` block;
6. Configure `extra_records` under `dns` for services not exposed publicly;
