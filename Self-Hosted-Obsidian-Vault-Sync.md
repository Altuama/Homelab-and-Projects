# Obsidian LiveSync Self-Hosted Setup

## Overview

- **Objective:** Real-time, cross-platform sync of an Obsidian vault (phone + desktop) without a third-party sync subscription
- **Category:** Self-hosted services / Homelab Quality of Life
- **Status:** Complete
- **Credit:** All credit for this project goes to [vrtmrz](https://github.com/vrtmrz/obsidian-livesync) and the open-source community. This documentation is a showcase of my implementation process and how it works in practice.

## Environment

| Component | Detail |
|-----------|--------|
| **Host** | Proxmox, existing Linux server VM (already running Tailscale) |
| **Container** | CouchDB via Docker Compose |
| **Network** | Tailscale (no port forwarding, no public exposure) |
| **Devices to sync** | Desktop (Windows/Linux/macOS) / Phone (iOS or Android) |

## Prerequisites

- [ ] Docker and Docker Compose installed on the Linux VM
- [ ] Tailscale already active on the VM (verify with `tailscale status`)
- [ ] MagicDNS enabled in the Tailscale admin console
- [ ] HTTPS Certificates enabled in the Tailscale admin console (DNS page)
- [ ] Cloudflared installed (for tunnel creation during setup)

## Implementation Steps

### Step 1 – Environment Variables

Set up the environment variables for your CouchDB instance:

```bash
export hostname=localhost:5984
export username=wayne          # Change as desired
export password=<your-password> # Change as desired
```

### Step 2 – Create Data Directories

Create the directories that will persist your CouchDB data and configuration:

```bash
mkdir couchdb-data
mkdir couchdb-etc
```

### Step 3 – Test Run

Run a temporary container to verify everything works correctly:

```bash
sudo docker run --name couchdb-for-ols --rm -it \
  -e COUCHDB_USER=${username} \
  -e COUCHDB_PASSWORD=${password} \
  -v ${PWD}/couchdb-data:/opt/couchdb/data \
  -v ${PWD}/couchdb-etc:/opt/couchdb/etc/local.d \
  -p 5984:5984 couchdb
```

This runs the container interactively. Once you confirm it starts without errors, press `Ctrl+C` to stop it.

### Step 4 – Deploy Persistent Container

Launch the container as a persistent background service:

```bash
sudo docker run --name couchdb-for-ols -d --restart always \
  -e COUCHDB_USER=${username} \
  -e COUCHDB_PASSWORD=${password} \
  -v ${PWD}/couchdb-data:/opt/couchdb/data \
  -v ${PWD}/couchdb-etc:/opt/couchdb/etc/local.d \
  -p 5984:5984 couchdb
```

**What this does:**
- `-d`: Runs the container in detached (background) mode
- `--restart always`: Automatically restarts the container if it crashes or the VM reboots
- `-v`: Mounts local directories for persistent storage

### Step 5 – Initialize CouchDB for LiveSync

Run the official initialization script to create the necessary database and configure CouchDB for use with LiveSync:

```bash
curl -s https://raw.githubusercontent.com/vrtmrz/obsidian-livesync/main/utils/couchdb/couchdb-init.sh | bash
```

This script:
- Creates the required databases
- Sets up proper permissions
- Configures CouchDB for LiveSync compatibility

### Step 6 – Create a Cloudflare Tunnel

To generate a connection URI without exposing your server to the internet, create a temporary Cloudflare Tunnel:

```bash
cloudflared tunnel --url http://localhost:5984
```

This outputs a public URL (e.g., `https://some-random-string.trycloudflare.com`). Leave it running in the background:

```bash
# Press Ctrl+Z to suspend the process
# Then resume it in the background
bg

# Detach it from the shell so it survives logout
disown -h
```

### Step 7 – Generate the Setup URI

Set up variables pointing to your Cloudflare tunnel and database:

```bash
export hostname=https://research-origin-retained-possess.trycloudflare.com  # Use your actual tunnel URL
export database=obsidian                                                    # Database name (change if desired)
export passphrase=<your-passphrase>                                         # Change as desired
export username=wayne
export password=<your-password>
```

Generate the setup URI:

```bash
deno run -A https://raw.githubusercontent.com/vrtmrz/obsidian-livesync/main/utils/flyio/generate_setupuri.ts
```

**Example output:**
```
your passphrase is: egg-dry
obsidian://setuplivesync?settings=%250a321d8792b...g%3D%3D
```

### Step 8 – Configure Obsidian LiveSync Plugin

1. Open Obsidian on your first device.
2. Navigate to **Settings → Community Plugins**.
3. Search for and install the **Self-hosted LiveSync** plugin by vrtmrz.
4. Enable the plugin.
5. Click the gear icon to open its settings.
6. Select "Use Setup URI" and paste the URI generated in Step 7.
7. Enter the passphrase when prompted.

**Repeat this process on every device you want to sync** (phone, tablet, second computer, etc.). Each device will connect to the same CouchDB backend.

### Step 9 – Fine-Tune Settings

Use the plugin's built-in tools to optimize the connection:

- Navigate to the plugin's settings menu.
- Look for the "Internal Tools" or "Diagnostics" section.
- Use the connection testing tools to identify the most suitable settings for your network environment.
- Adjust sync frequency, conflict resolution behavior, and database settings as needed.

## Access Information

| Service | Address / Method |
|---------|------------------|
| **CouchDB Admin** | `http://localhost:5984/_utils` (from VM) or `http://<tailscale-ip>:5984/_utils` (from other devices) |
| **Cloudflare Tunnel** | Temporary URL generated by `cloudflared` (changes each session) |
| **Tailscale Hostname** | `zapus-kingsnake.ts.net` (or your VM's Tailscale address) |

## Architecture Diagram

```
┌─────────────────────────────────────────────────────────┐
│                    Proxmox Host                         │
│                                                         │
│  ┌──────────────────────────────────────────────────┐   │
│  │          Linux Server VM (Tailscale)             │   │
│  │                                                  │   │
│  │  ┌──────────────────────────────────────────┐    │   │
│  │  │        Docker Container                  │    │   │
│  │  │                                          │    │   │
│  │  │  ┌──────────────────────────────────┐    │    │   │
│  │  │  │          CouchDB                 │    │    │   │
│  │  │  │  Port: 5984                      │    │    │   │
│  │  │  │  Database: obsidian              │    │    │   │
│  │  │  └──────────────────────────────────┘    │    │   │
│  │  └──────────────────────────────────────────┘    │   │
│  │                                                  │   │
│  │  ┌──────────────────────────────────────────┐    │   │
│  │  │    Cloudflared (temporary tunnel)        │    │   │
│  │  └──────────────────────────────────────────┘    │   │
│  └──────────────────────────────────────────────────┘   │
│                          │                              │
│                    Tailscale                            │
│                   (VPN Network)                         │
└──────────────────────────┬──────────────────────────────┘
                           │
         ┌─────────────────┼─────────────────┐
         │                 │                 │
         ▼                 ▼                 ▼
    ┌──────────┐      ┌──────────┐    ┌──────────┐
    │ Desktop  │      │   Phone  │    │  Tablet  │
    │ Obsidian │      │ Obsidian │    │ Obsidian │
    └──────────┘      └──────────┘    └──────────┘
```

## How It Works

1. **CouchDB** runs as a Docker container on the Linux VM, storing all vault notes and metadata.
2. **Tailscale** creates a secure, encrypted network connecting all devices without port forwarding.
3. Each **Obsidian client** uses the Self-hosted LiveSync plugin to connect to the CouchDB database.
4. **Real-time sync** propagates changes from any device to all others within seconds.
5. **Conflict resolution** is handled by the plugin, with options for automatic or manual conflict management.

## Future Improvements

- [ ] Set up persistent Cloudflare Tunnel with a custom domain for easier URI generation
- [ ] Configure automatic backups of the CouchDB database
- [ ] Explore plugin settings for better performance on mobile devices
- [ ] Document recovery procedures in case of database corruption

## Key Commands Reference

| Command | Purpose |
|---------|---------|
| `sudo docker ps \| grep couchdb` | Check if CouchDB container is running |
| `sudo docker logs couchdb-for-ols` | View container logs for troubleshooting |
| `sudo docker restart couchdb-for-ols` | Restart the CouchDB container |
| `curl http://localhost:5984/obsidian` | Test database connectivity |
| `tailscale status` | Verify Tailscale is running |
| `cloudflared tunnel --url http://localhost:5984` | Create temporary tunnel (run in background) |

---
