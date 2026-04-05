

# Enterprise Mail Server with Docker & Monitoring

**Full-stack containerized mail infrastructure with real-time monitoring**
**Author:** Rawen Zgarni
**Date:** April 2026

---

## Table of Contents

* Project Overview
* Technology Stack
* Architecture
* Port Configuration
* Technical Justifications
* Features
* Installation Guide
* Testing & Validation
* Project Timeline
* Future Improvements
* References
* Author & Acknowledgments

---

## Project Overview

This project implements a **production-ready enterprise mail server** using containerization technologies. The goals include:

* Full mail suite (SMTP, IMAP, POP3, Webmail)
* Security (SSL/TLS, firewall, anti-spam, antivirus)
* Monitoring (real-time metrics, dashboards)
* Remote administration (web-based management)
* High availability (persistent data, container orchestration)

### Objectives

* Deploy containerized mail server
* Reverse proxy with HTTPS encryption
* Isolated Docker networking
* Monitoring with Grafana & Prometheus
* Remote administration via Cockpit
* Firewall configuration
* Persistent volumes for data

---

## Technology Stack

| Component         | Technology     | Purpose                     |
| ----------------- | -------------- | --------------------------- |
| Mail Server       | poste.io       | Full mail suite             |
| Reverse Proxy     | Nginx          | HTTPS termination & routing |
| Orchestration     | Docker Compose | Multi-container management  |
| Monitoring        | Prometheus     | Metrics collection          |
| Visualization     | Grafana        | Dashboards & alerting       |
| Container Metrics | cAdvisor       | Docker monitoring           |
| Admin Panel       | Cockpit        | Web server management       |
| Firewall          | firewalld      | Port security               |
| OS                | Rocky Linux 9  | Base system                 |

---

## Architecture

**Docker Bridge Network: `mail-network`**

* Subnet: 172.20.0.0/24
* Driver: bridge
* Isolation: internal container communication

**High-Level Components:**

* Nginx: Reverse proxy, SSL termination
* poste.io: Mail server with Rspamd, ClamAV, Roundcube
* Grafana/Prometheus: Monitoring and metrics
* cAdvisor: Container metrics
* Cockpit: Server administration
* Persistent Docker volumes for mail data

**Data Flow:**

* SMTP → Spam/virus scan → Mail storage → User mailbox
* Webmail → HTTPS → Nginx → poste.io → Roundcube
* Metrics → cAdvisor → Prometheus → Grafana dashboards

---

## Port Configuration

| Port | Service         | Purpose                   |
| ---- | --------------- | ------------------------- |
| 22   | SSH             | Admin access (restricted) |
| 25   | SMTP            | Email sending             |
| 80   | HTTP            | Redirect to HTTPS         |
| 110  | POP3            | Email retrieval           |
| 143  | IMAP            | Email access              |
| 443  | HTTPS           | Secure web access         |
| 465  | SMTPS           | Secure SMTP               |
| 587  | SMTP Submission | Email submission          |
| 993  | IMAPS           | Secure IMAP               |
| 995  | POP3S           | Secure POP3               |
| 3000 | Grafana         | Dashboard                 |
| 4190 | ManageSieve     | Email filtering           |
| 8080 | cAdvisor        | Container metrics         |
| 9090 | Cockpit         | Admin panel               |
| 9091 | Prometheus      | Metrics                   |

Firewall allows only the listed ports.

---

## Technical Justifications

* **Docker Compose:** declarative, reproducible, network isolation, volume management
* **Nginx Proxy:** SSL termination, security headers, load balancing
* **Prometheus + Grafana:** real-time visibility, historical data, alerting
* **cAdvisor:** container-native metrics, lightweight
* **Cockpit:** web-based server administration
* **firewalld:** dynamic, zone-based firewall
* **Persistent Volumes:** data survives container restarts

---

## Features

**Mail Server:** SMTP, IMAP, POP3, ManageSieve, Roundcube, Rspamd, ClamAV, virtual domains, aliases, mailboxes, calendar & contacts

**Security:** SSL/TLS, firewall, SSH hardening, Docker network isolation, security headers

**Monitoring:** Grafana dashboards, Prometheus metrics, cAdvisor container tracking

**Administration:** Cockpit web access, service management, centralized logs, web terminal

---

## Installation Guide

**Prerequisites:** Rocky Linux 9.x, Docker & Docker Compose, 2+ cores, 4GB RAM, 20GB+ disk, static IP

**Quick Start:**

```bash
# Clone repository
git clone <repo-url>
cd <repo-dir>

# Create persistent data directory
sudo mkdir -p /opt/poste-data

# Configure firewall
sudo firewall-cmd --permanent --add-port={25,80,110,143,443,465,587,993,995,3000,4190,8080,9090,9091}/tcp
sudo firewall-cmd --reload

# Start containers
docker compose up -d

# Check running containers
docker compose ps
```

**Web Interfaces:**

* Mail Admin: `https://<server>/admin`
* Webmail: `https://<server>/webmail`
* Grafana: `http://<server>:3000`
* Prometheus: `http://<server>:9091`
* cAdvisor: `http://<server>:8080`
* Cockpit: `https://<server>:9090`

**Mail Domain Setup:**

```bash
docker exec poste-mail poste domain:create example.com
docker exec poste-mail poste email:create admin@example.com <secure-password>
docker exec poste-mail poste email:admin admin@example.com
```

---

## Testing & Validation

* Container health: `docker compose ps`
* Connectivity: HTTP → HTTPS redirect, SMTP tests, firewall validation
* Mail functionality: send/receive test emails
* Monitoring validation: Prometheus targets, Grafana dashboards
* SSL/TLS certificate verification
* Resource usage: `docker stats`

---

## Future Improvements

**Short-term:** Let's Encrypt SSL, real domain, automated backups, email alerts, custom dashboards

**Long-term:** High availability, load balancing, MFA, DKIM/DMARC, Kubernetes migration, CI/CD pipeline

---

## References

* poste.io official documentation
* Docker Compose, Nginx, Prometheus, Grafana guides
* Related projects: Mail-in-a-Box, Docker Mailserver

---

## Author & Acknowledgments

**Rawen Zgarni**
GitHub: [@zgarnirawen](https://github.com/zgarnirawen)

Acknowledgments: poste.io team, Docker community, Prometheus & Grafana communities

