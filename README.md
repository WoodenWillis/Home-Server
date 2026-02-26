# Home-Server

My modular home lab setup built around two specialized nodes. It handles AI workloads, media processing, security research, and network services. Each node has a defined role. I called them the Brain and the Muscle.

---

## Architecture

```
┌─────────────────────────────────────────────────────┐
│                   Cisco Catalyst                      │
│                                                     │
│   ┌──────────────────┐    ┌─────────────────────┐  │
│   │   Brain_node     │    │    Muscle_node      │  │
│   │  Raspberry Pi 5  │    │     Mac Pro         │  │
│   │  AI · Backup     │    │  Frigate · Compute  │  │
│   └──────────────────┘    └─────────────────────┘  │
└─────────────────────────────────────────────────────┘
```

---

## Nodes

### 🧠 Brain_node — Raspberry Pi 5 16GB
Orchestrateur and intelligence node. Handles lightweight AI tasks and  automates backkups.

**Primary responsibilities:**
- Local LLM / AI inference tasks
- Scheduled backup orchestration
- Low-power always on compute

### 💪 Muscle_node — Mac Pro Trashcan
The heavy compute node. Runs GPU-accelerated workloads that need more resources.

**Primary responsibilities:**
- [Frigate NVR](https://frigate.video/) — real-time object detection from camera feeds
- High-throughput data processing
- Movie streaming server. 

---

## Repository Structure

```
Home-Server/
├── Brain_node/       # Configs, scripts, and services for the Raspberry Pi 5
├── Muscle_node/      # Configs, scripts, and services for the Mac Pro
└── README.md
```

---

## Set-up

- Docker and Docker Compose
- SSH access
- Cisco Catalyst 3750 for VLAN isolation
  
### Deployment

Each node directory is self-contained. Navigate into the relevant node folder and apply configs directly:

```bash
deploy services on Brain_node and Muscle_node
cd ~/home-server
docker compose up -d
```

## Contributing

This is a personal infrastructure repo. Feel free to fork it and adapt it for your own setup. Fixes or improvements are welcome.

---
