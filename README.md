# home-assistant-config

Personal Home Assistant setup running as a Docker container on a Linux VM (VirtualBox).

## Devices

- **Robot vacuum** — with vacuuming and mopping, room-based segment control
- **Smart plugs** — for controlling lights and appliances

## Automations

### Vacuum → Mop
After every vacuuming run, the robot automatically mops the same rooms:
- Detects which segments were vacuumed (via `input_text.last_segments`)
- Automatically switches to mop mode (intensity: intense, mode: deep_plus)
- An `input_boolean` prevents an automation loop when the robot docks after mopping

## Control via Telegram Bot

Devices can be controlled via Telegram message (e.g. "turn on bedroom light"). The bot runs via [Openclaw](https://openclaw.dev) and internally calls `ha_switch.sh`, a bash script that talks to the Home Assistant REST API.

## Security

API tokens are **not** stored in the repository. The Home Assistant Long-Lived Access Token is stored locally in `~/.secrets/ha_token` with restricted file permissions (`640`). The script reads it at runtime:

```bash
HA_TOKEN=$(cat ~/.secrets/ha_token)
```

This way the token never ends up in Git or API logs.

## Setup

### Requirements
- Docker + Docker Compose
- Home Assistant (via `docker-compose.yml`)

### Set up token
```bash
mkdir -p ~/.secrets
echo "YOUR_TOKEN" > ~/.secrets/ha_token
chmod 600 ~/.secrets/ha_token
```

### Start Home Assistant
```bash
docker compose up -d
```
