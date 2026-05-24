# Ticketing Lab 01 — Peppermint Help Desk

**Brady Durham | Nashville, TN | IT Support & Sysadmin Portfolio**

A hands-on ticketing lab deploying Peppermint, an open-source help desk system, using Docker on Ubuntu. Includes a full ticket interrupt scenario worked through using OSI model troubleshooting methodology.

---

## 🎯 Objectives

- Deploy a real-world ticketing system using Docker and Docker Compose
- Configure Peppermint with users, clients, and ticket categories
- Practice creating, commenting, assigning, and escalating tickets
- Apply OSI model troubleshooting to a simulated help desk scenario

---

## 🛠️ Environment

| Component | Details |
|-----------|---------|
| **Host OS** | Ubuntu 24.04 Desktop (VM on Proxmox) |
| **Docker** | Docker Engine 29.1.3 |
| **Docker Compose** | v2.40.3 |
| **Ticketing System** | Peppermint v0.5.5 |
| **Database** | PostgreSQL 15 |

---

## 📦 Installation

### Step 1 — Install Docker

```bash
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
```

Verify installation:

```bash
sudo docker run hello-world
```

### Step 2 — Install Docker Compose v2

```bash
sudo apt install docker-compose-v2 -y
```

### Step 3 — Create Peppermint Directory and Download Compose File

```bash
mkdir peppermint && cd peppermint
curl -o docker-compose.yml https://raw.githubusercontent.com/Peppermint-Lab/peppermint/master/docker-compose.yml
```

### Step 4 — Fix PostgreSQL Version Compatibility

The default `postgres:latest` image causes volume format errors. Pin to PostgreSQL 15:

```bash
sed -i 's/postgres:latest/postgres:15/g' docker-compose.yml
```

### Step 5 — Start Peppermint

```bash
sudo docker compose up -d
```

Verify containers are running:

```bash
sudo docker compose ps
```

### Step 6 — Access Peppermint

Open a browser and navigate to:

```
http://localhost:1000
```

Default login credentials:
- **Email:** admin@admin.com
- **Password:** 1234

---

## ⚙️ Configuration

After logging in, the following was configured via the Admin panel:

- Created users: Jared (technician), Jian Yang
- Created clients: Monica, Gavin, Peter
- Explored ticket categories, email queues, and roles

---

## 🎟️ Ticket Interrupt — Internet Connectivity Issue

### Scenario

A simulated help desk ticket was received:

> **Ticket #5563284: Help! Internet won't work!**
> User's computer is connected via ethernet on the 10.0.0.0/24 network. The gateway is 10.0.0.1.

### Approach

Ticket was created in Peppermint and worked through using OSI model troubleshooting methodology:

**Layer 1 — Physical**
Had user check network cable is plugged in properly, checked for damage, confirmed link light was on the NIC.

**Layer 2 — Data Link**
Had user check if data light was flashing on NIC to confirm Layer 2 traffic. Ran `arp -a` to confirm gateway MAC address was present in ARP table.

**Layer 3 — Network**
Had user run `ipconfig` to verify IP address on correct subnet. Confirmed gateway at 10.0.0.1 was reachable via ping. User was unable to ping google.com. Ran `ipconfig /release`, `/flushdns`, `/renew`, and `/registerdns` — none resolved the issue. Ran `tracert` and confirmed connectivity dropped at the gateway.

**Escalation**
After reaching the end of the standard troubleshooting workflow, ticket was escalated to the next support level.

### Ticket Outcome

- Ticket assigned to technician
- All troubleshooting steps documented in ticket comments
- Status updated to In Progress, priority set to High
- Escalated with full documentation

---

## 🔧 Troubleshooting Notes

| Issue | Cause | Fix |
|-------|-------|-----|
| `docker-compose` Python error | Outdated package incompatible with Python 3.12 | Installed `docker-compose-v2` |
| PostgreSQL container restarting | Volume format conflict with `postgres:latest` | Pinned to `postgres:15` and cleared volume |
| Peppermint unreachable on port 3000 | Compose file maps to port 1000 on host | Used `http://localhost:1000` |

---

## 📁 Screenshots

```
ticketing-lab-01-peppermint/
├── peppermint-setup/
│   ├── docker-install/
│   ├── containers-running/
│   ├── peppermint-login/
│   ├── peppermint-dashboard/
│   └── admin-configuration/
└── ticket-interrupt/
    ├── ticket-scenario/
    ├── oSI-troubleshooting-steps/
    ├── ticket-creation/
    ├── ticket-comments/
    └── ticket-completed/
```

---

## 📚 What I Learned

- Docker and Docker Compose deployment of a multi-container application
- How to troubleshoot container startup failures using `docker compose logs`
- PostgreSQL version pinning to avoid volume compatibility issues
- Real-world ticketing workflow — create, comment, assign, escalate
- Applying OSI model layers to a structured troubleshooting process
- The difference between `docker-compose` (v1) and `docker compose` (v2)

---

*Part of my sysadmin portfolio at [github.com/brady-durham/sysadmin-portfolio](https://github.com/brady-durham/sysadmin-portfolio)*
