# VM-Tracker

# Getting Started

Complete documentation: [Docs EN](https://vm-tracker.kuepper.nrw/docs/en/)

## Overview

VM Tracker is a system for automated monitoring and management of server and virtual machines. It consists of two main components:

### 🖥️ API Server

The **VM Tracker API Server** is a central REST API that serves as the central management instance.

**Main Functions:**
- Receives and stores VM registrations (hostname, IP address, interface)
- Manages the status of all VMs (online/offline)
- Provides a web interface for overview
- Offers REST API endpoints for VM management
- Automatically monitors VM availability (status updates)
- Automatically cleans up inactive VMs

**Deployment:**
The API Server typically runs as a central service in your infrastructure (Docker, Kubernetes, or as a binary).

### 📡 Client

The **VM Tracker Client** is a lightweight agent installed on each VM to be monitored.

**Main Functions:**
- Automatically detects the VM's hostname
- Determines the IP address of the configured network interface
- Sends regular updates (heartbeats) to the API Server
- Performs automatic retry attempts on connection problems
- Supports graceful shutdown

**Deployment:**
The client is installed on each VM and runs as a background service (systemd, Docker, or as a binary).

## Architecture

```
+-------------+     +-------------+     +-------------+
|   VM #1     |     |   VM #2     |     |   VM #3     |
|             |     |             |     |             |
|  +--------+ |     |  +--------+ |     |  +--------+ |
|  | Client | |     |  | Client | |     |  | Client | |
|  +---+----+ |     |  +---+----+ |     |  +---+----+ |
+------+------+     +------+------+     +------+------+
       |                   |                   |
       |   POST /api/register (Heartbeat)      |
       +-------------------+-------------------+
                           |
                           v
                  +-----------------+
                  |   API Server    |
                  |                 |
                  |  +-----------+  |
                  |  | REST API  |  |
                  |  +-----------+  |
                  |                 |
                  |  +-----------+  |
                  |  |  Web UI   |  |
                  |  +-----------+  |
                  |                 |
                  |  +-----------+  |
                  |  |  Storage  |  |
                  |  +-----------+  |
                  +-----------------+
```

[More architecture informations](https://vm-tracker.kuepper.nrw/docs/en/architecture.md)

## Quick Start

### 1. Start API Server

```bash
# With Docker
docker run -p 8080:8080 vm-tracker-api:latest

# Or as Binary
./vm-tracker-api
```

API Server is accessible at: `http://localhost:8080`

### 2. Install Client on VMs

```bash
# On each VM
./vm-tracker-client -api "http://API_SERVER_IP:8080" -interface "eth0"
```

### 3. Open Web Interface

Open in your browser: `http://API_SERVER_IP:8080`

You will now see an overview of all registered VMs with:
- Hostname
- IP address
- Status (online/offline)
- Last contact
- Network interface

## Typical Use Cases

### Homelab / Self-hosted Infrastructure
Keep track of all VMs in your homelab without manually checking each server.

### Kubernetes Clusters
Monitor worker nodes and their network configuration.

### Cloud VMs
Track dynamic IP addresses in DHCP-based infrastructures.

### Development Environments
Quick overview of test VMs and their current IP addresses.

## Next Steps

- **API Server Installation:** See [API Installation Guides](https://vm-tracker.kuepper.nrw/docs/en/api/)
- **Client Installation:** See [Client Installation Guides](https://vm-tracker.kuepper.nrw/docs/en/client/)
- **Configuration:** See [Configuration](https://vm-tracker.kuepper.nrw/docs/en/configuration.md)
- **Troubleshooting:** See [FAQ](https://vm-tracker.kuepper.nrw/docs/en/faq.md)
