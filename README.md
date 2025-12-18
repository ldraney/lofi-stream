# lofi-stream

24/7 lofi beats live stream on YouTube, powered by a static HTML page and a cheap VPS.

**Live:** [YouTube Stream](https://youtube.com/live) *(update with your actual stream URL)*

## How It Works

```
GitHub Pages (static HTML)     Hetzner VPS (€4.50/mo)      YouTube Live
┌─────────────────────┐       ┌──────────────────────┐    ┌─────────────┐
│  Rain animation     │       │  Xvfb (virtual       │    │             │
│  City skyline       │──────▶│    display)          │───▶│  24/7 RTMP  │
│  Web Audio API      │       │  Chromium            │    │   stream    │
│  (generative lofi)  │       │  ffmpeg              │    │             │
└─────────────────────┘       └──────────────────────┘    └─────────────┘
```

## Quick Start

### View the page locally
```bash
open docs/index.html
```

### Manage the stream
```bash
# SSH to server
ssh -i ~/api-secrets/hetzner-server/id_ed25519 root@5.78.42.22

# Check status
systemctl status lofi-stream

# Restart stream
systemctl restart lofi-stream

# View live logs
journalctl -u lofi-stream -f

# Stop stream
systemctl stop lofi-stream
```

## Features

- **Visuals:** CSS-only rain, glowing moon, city skyline, cozy windowsill with plant & coffee
- **Audio:** Generative lofi using Web Audio API (no copyright issues!)
  - Brown noise (rain ambience)
  - Chord progression (C - Am - F - G)
  - Sub bass drone
  - Vinyl crackle
- **Streaming:** 1080p @ 30fps, ~3Mbps to YouTube RTMP
- **Reliability:** systemd auto-restart on crash or reboot

## Architecture

| Component | Tech | Cost |
|-----------|------|------|
| Frontend | HTML/CSS/JS on GitHub Pages | Free |
| Server | Hetzner CX22 (2 vCPU, 2GB RAM) | €4.50/mo |
| Streaming | Xvfb + Chromium + ffmpeg | - |
| Platform | YouTube Live | Free |

## Performance

Single stream maxes out the CX22:
- CPU: ~91%
- RAM: ~50%
- Network: ~1 Mbps

See [CLAUDE.md](CLAUDE.md) for scaling options.

## Files

```
lofi-stream/
├── docs/                 # GitHub Pages site
│   ├── index.html        # Lofi page with visuals + audio
│   └── style.css         # All the CSS animations
├── server/               # VPS scripts
│   ├── stream.sh         # Main streaming script
│   ├── setup.sh          # Server setup script
│   └── lofi-stream.service  # systemd unit
├── CLAUDE.md             # Project roadmap & docs
└── README.md             # This file
```

## Setup From Scratch

1. **Deploy the page** - Push `docs/` to GitHub Pages
2. **Create VPS** - Hetzner CX22 with Ubuntu 24.04
3. **Install deps** - `apt install xvfb chromium-browser ffmpeg pulseaudio xdotool`
4. **Copy scripts** - `scp server/* root@your-server:/opt/lofi-stream/`
5. **Set stream key** - Edit `/etc/systemd/system/lofi-stream.service`
6. **Enable service** - `systemctl enable --now lofi-stream`

## License

MIT - Do whatever you want with it.
