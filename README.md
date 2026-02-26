# Home-Server

This repository contains the configurations for my multi-node home lab. The setup is divided into two primary nodes to balance compute tasks and is managed through an isolated network controlled by an OpenWrt gateway and my Cisco Catalyst switch.

---

## Architecture
```
┌─────────────────────────────────────────────────────────┐                
│              Cisco Catalyst 3750 (Isolated)             │                :     :      -     :
│   (VLAN 10: ServerZone | VLAN 20: CamNet | No WAN)      │                 -    -      =    :.
│                                                         │                 ::   -      -   .:  
│   ┌───────────────────┐        ┌─────────────────────┐  │   Subnetwork     -   ..    :.   -  
│   │   🧠 Brain_node   │        │   💪 Muscle_node    │  │==================.===.= ─┐ =.  =.  
│   │  Raspberry Pi 5 16G │        │      Mac Pro        │  │   isolation     ***************##* 
│   │  Inference/Backup │        │   NVR/Transcoding   │  │                +**************####+
│   └─────────┬─────────┘        └──────────┬──────────┘  │                *####*++++=+++*####=
└─────────────┼─────────────────────────────┼─────────────┘                     Main Router 
              └───────[ ThinkCentre ]───────┘                                    (OpenWrt)
                 (Management & Sync Plane)
```
---

## Nodes

### Brain_node (Raspberry Pi 5)

- Role: Handles lightweight, always-on services.
- AI: Runs Ollama for local LLM inference (qwen2.5-coder:14b).
- Storage: Boots from an external NVMe SSD. It is mounted via UUID in /etc/fstab to ensure the system only boots when the data drive is present.
- Backup: Uses Duplicati for scheduled container data backups.

### Muscle_node — (Mac Pro Trashcan)

- Role: Handles resource-intensive tasks.
- Video: Runs Frigate NVR for camera object detection and Jellyfin for media.
- Hardware: Both services use GPU passthrough (/dev/dri/renderD128) for hardware-accelerated video processing.
- Network: Uses network_mode: "host" to simplify discovery and performance within the isolated segment.

### Network Configuration (OpenWrt & Cisco)

The network is partitioned into zones to keep traffic separated and secure.

- Subnetwork Isolation: The Cisco Catalyst switch is on a separate subnetwork that has no default gateway to the internet. This means the server nodes and cameras are physically     unable to reach the WAN.
- VLANs:
        - camnet (VLAN 20): Dedicated to cameras with isolate '1' enabled, preventing the cameras from talking to each other.
        - serverzone (VLAN 10): Where the Brain and Muscle nodes live.
- Traffic Management: I use SQM (Cake) on the OpenWrt WAN interface to prevent bufferbloat and keep management connections stable during high traffic.
- Remote Access: Secured via Tailscale running directly on the OpenWrt router.

---

---

## Workflow
Centralized Configs: Every configuration is done on my Thinkcentre where my nodes directories are mounted via sshfs.
Deployment: Any changes are immediately available on the nodes for a docker compose up -d.
  
## Technical Specs

- Router OS: OpenWrt
- Switch: Cisco Catalyst 3750 
- Inference: Ollama
- Containers: Docker / Docker Compose

## Contributing

This is a personal infrastructure repo. Feel free to fork it and adapt it for your own setup. Fixes or improvements are welcome.

---
