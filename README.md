# NTS Radio for Raspberry Pi Zero 2W

Stream NTS Radio (live channels + infinite mixtapes) on a Pi Zero 2W with a Pimoroni Pirate Audio Line-out HAT. Deployed via Balena Cloud.

## Hardware

- Raspberry Pi Zero 2W
- [Pimoroni Pirate Audio Line-out](https://shop.pimoroni.com/products/pirate-audio-line-out) — ST7789 240x240 display, 4 buttons, I2S DAC

## Deploy with Balena Cloud

### 1. Create a fleet

Create a new fleet on [Balena Cloud](https://dashboard.balena-cloud.com) for device type **Raspberry Pi Zero 2 W**.

The `balena.yml` already declares all the hardware config (SPI, I2S DAC overlay, GPU memory) and default environment variables — no manual dashboard setup needed.

### 2. Push

```bash
balena login
balena push <fleet-name>
```

### Environment variables

These are set as fleet defaults in `balena.yml` and can be overridden per-device in the dashboard:

| Variable | Default | Description |
|----------|---------|-------------|
| `NTS_DEFAULT_CHANNEL` | `1` | Start on channel 1 or 2 |
| `NTS_DISPLAY_BRIGHTNESS` | `80` | Backlight brightness 0-100 |
| `NTS_BUTTON_DEBOUNCE_MS` | `200` | Button debounce in ms |

## Controls

| Button | Normal | Menu |
|--------|-------------|-----------|
| A (top-left) | Prev channel | Scroll up |
| B (bottom-left) | Next channel | Scroll down |
| X (top-right) | Play/Pause | Select |
| Y (bottom-right) | Open menu | Back |

## Screens

- **Live** — NTS 1 / NTS 2 with show artwork, title, and progress bar
- **Mixtapes** — Browse and play all NTS infinite mixtapes
- **Menu** — Switch between modes, adjust brightness

## Project structure

```
nts-radio/
├── nts/
│   ├── api.py         # NTS API client (live, mixtapes)
│   ├── player.py      # mpv wrapper with IPC socket control
│   ├── display.py     # ST7789 240x240 display rendering
│   ├── buttons.py     # GPIO button handler with debounce
│   └── app.py         # Main app state machine
├── Dockerfile.template
├── docker-compose.yml
├── balena.yml
└── requirements.txt
```

## Local development (without Balena)

For testing on a Pi directly, the `install.sh` script and `nts-radio.service` are also included as an alternative to the Balena deployment.
