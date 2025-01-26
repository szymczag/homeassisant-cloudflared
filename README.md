# Cloudflared Tunnel for Home Assistant :cloud: :lock:

[![Release][release-badge]][release-link]
[![License: MIT][license-badge]][license-link]
[![Changelog][changelog-badge]][changelog-link]

Securely expose your Home Assistant instance via **Cloudflare Tunnel**—no port forwarding, no extra SSL configurations! This add-on handles the Cloudflare connector automatically once you provide an ephemeral tunnel token from [Cloudflare Zero Trust](https://dash.teams.cloudflare.com/).

---

## Features :rocket:
- **Secure** end-to-end tunnel to your Home Assistant.
- **Ephemeral tokens**: no need to manage separate credential files.
- Minimal configuration required: just supply your **tunnel token** from Cloudflare.

---

## Installation :wrench:

1. **Add this repository** to your Home Assistant Supervisor.  
   - In Home Assistant, go to **Settings → Add-ons → Add-on Store**.  
   - Click the **3-dot menu** in the top right → **Repositories** → paste:  
     ```
     https://github.com/szymczag/homeassisant-cloudflared
     ```
   - Click **Add**.  
2. **Install the add-on**:  
   - Search for “**Cloudflared Tunnel for Home Assistant**” in the Add-on Store and install.  
3. **Configure**:  
   - Go to the add-on’s **Configuration** tab.  
   - Paste your **Cloudflare Tunnel Token** (see below).  
4. **Start** the add-on.  

---

## How to Get a Cloudflare Tunnel Token :clipboard:

1. Go to [Cloudflare Zero Trust](https://dash.teams.cloudflare.com/) and create a new tunnel (e.g., `ha-tunnel`).  
2. **Add a public hostname** (e.g. `ha.example.com`) pointing to `http://homeassistant:8123` (or your local IP).  
3. In the Cloudflare tunnel page, click **Install Connector** (or similar).  
4. Copy only the token portion from the `cloudflared tunnel run --token <TOKEN>` command.  
5. In Home Assistant, paste it into the **Cloudflare Tunnel Token** field.  

Once started, your add-on should connect to Cloudflare.  
You can access Home Assistant externally via `https://ha.example.com` (or whatever custom domain you set up).

---

## Configuration Changes Required in configuration.yaml :wrench:
To ensure proper operation with Cloudflare Tunnel, you must add the following settings to your Home Assistant `configuration.yaml` file:
```
http:
  ip_ban_enabled: true
  trusted_proxies:
    - 172.30.33.0/24
  cors_allowed_origins:
    - https://your-local-ha-url
    - https://your-cloudflare-url
```
Replace `https://your-local-ha-url` with your local Home Assistant URL (e.g., `https://homeassistant.local:8123`) and `https://your-cloudflare-url` with your Cloudflare Tunnel URL (e.g., `https://ha.example.com`). Check the `trusted_proxies` IP range if it matches your Home Assistant Supervisor network.

### Explanation of Each Setting:
- `ip_ban_enabled: true` → Enables automatic banning of malicious IP addresses.
- `trusted_proxies` → Allows Cloudflare Tunnel (running in the Home Assistant add-on environment) to act as a trusted reverse proxy.
- `cors_allowed_origins` → Specifies the URLs that are allowed to interact with Home Assistant through the browser.


## Changelog :scroll:
Check out our [CHANGELOG.md](CHANGELOG.md) for details on each release.  
(Or view all releases and tags on the [GitHub Releases][release-link].)

---

## Contributing :handshake:
- **Issues**: Please open a [GitHub Issue](https://github.com/szymczag/homeassisant-cloudflared/issues) for bug reports or feature requests.  
- **Pull Requests**: Fork the repository and open a PR with a clear description of changes. I welcome contributions, but they must be reviewed before merging.

Before that, please test it locally and ensure it works as expected.
```
docker build -t cloudflared-test .
docker run --rm -e CLOUDFLARE_TUNNEL_TOKEN="xxx" cloudflared-test
```
and then:
```
ha addons validate .
```

---

## License
This project is licensed under the [MIT][license-link].  
See the [LICENSE](LICENSE) file for details.

---

## Author :writing_hand:
**@szymczag**  
- GitHub: [github.com/szymczag](https://github.com/szymczag/homeassisant-cloudflared)

---

[release-badge]: https://img.shields.io/github/v/release/szymczag/homeassisant-cloudflared.svg?logo=github
[release-link]: https://github.com/szymczag/homeassisant-cloudflared/releases

[license-badge]: https://img.shields.io/github/license/szymczag/homeassisant-cloudflared?color=blue
[license-link]: https://github.com/szymczag/homeassisant-cloudflared/blob/main/LICENSE

[changelog-badge]: https://img.shields.io/badge/changelog-click%20here-blue
[changelog-link]: https://github.com/szymczag/homeassisant-cloudflared/blob/main/CHANGELOG.md
