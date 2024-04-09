# HA Smart mirror

This is a basic configuration for Raspberry Pi 4 to operate a kiosk Chrome browser running Home Assistant tab on BalenaOS with auth bypass and Browser API for automation and remote control.

## Hardware

- Raspberry Pi 4
- SD Card (4GB+)
- HDMI monitor or a [buy a pre made smart mirror with HDMI input and cables for RPI 4](https://amzn.to/4cIKq5o)

## Easy way

1. [![balena deploy button](https://www.balena.io/deploy.svg)](https://dashboard.balena-cloud.com/deploy?repoUrl=https://github.com/0x77dev/smart-mirror)


2. Flash your RPI4 with the balenaOS image and boot it up.

3. Set static IP for your RPI4 in your router DHCP server settings.

4. Patch your home assistant configuration to bypass auth for smart mirror

```yaml
homeassistant:
  auth_providers:
    # Important to keep this top on the list to bypass login page
    - type: trusted_networks
      trusted_networks:
        - 192.168.0.0/24
      trusted_users:
        # Set your smart mirror ip address and kiosk user id
        # Tip: to enhance security and avoid ip changes modify your router DHCP server to assign a static ip to your smart mirror
        {{ smart mirror ip address }}: {{ user uuid }}
      allow_bypass_login: true
    - type: homeassistant
```

5. Optional: Install [Kiosk mode](https://github.com/NemesisRE/kiosk-mode), and [Browser mod](https://github.com/thomasloven/hass-browser_mod) for better automation and UI experience.

## Manual way

1. Clone this repo

```bash
git clone https://github.com/0x77dev/smart-mirror
```

2. [Install balena-cli](https://docs.balena.io/reference/balena-cli/)

3. Create a new application in BalenaCloud and push the code to your application

```bash
balena login
balena push <app-name>
```

4. Flash your RPI4 with the balenaOS image and boot it up.

5. Set static IP for your RPI4 in your router DHCP server settings.

6. Patch your home assistant configuration to bypass auth for smart mirror as mentioned in the easy way.


## Additional docs

- [Browser REST API](https://github.com/balena-io-experimental/browser?tab=readme-ov-file#api)
- [Auth providers](https://www.home-assistant.io/docs/authentication/providers)
- [BalenaOS](https://docs.balena.io/learn/getting-started/raspberrypi5/nodejs/)

## Useful commands

### Refresh the page

```bash
curl -X POST {{ SMART MIRROR IP ADDRESS }}:5011/refresh
```