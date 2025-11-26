# Secure Hybrid-Cloud Infrastructure

![Docker](https://img.shields.io/badge/Containerization-Docker-blue)
![Traefik](https://img.shields.io/badge/Ingress-Traefik_v3-orange)
![Cloudflare](https://img.shields.io/badge/Security-Zero_Trust-F38020)
![Status](https://img.shields.io/badge/Status-Production-green)

A comprehensive, containerized microservices infrastructure designed for automated content aggregation, secure remote access, and IoT orchestration. 

This project demonstrates a production-grade approach to self-hosting, bridging local hardware with cloud-edge security policies.

## 🏗 Architecture Overview

The infrastructure follows a **Service-Oriented Architecture (SOA)**, deployed via Docker Compose. It leverages a hybrid networking model to expose services securely without opening firewall ports.

*   **Ingress Controller:** Traefik v3 handles internal routing, load balancing, and SSL termination.
*   **Edge Security:** Cloudflare Tunnel (via `cloudflared`) provides a secure ingress path, bypassing CGNAT issues and closing all inbound firewall ports.
*   **Access Control:** Cloudflare Zero Trust serves as the Identity-Aware Proxy (IAP), enforcing SSO (GitHub/Email) for administrative dashboards while utilizing Bypass Rules for specific API endpoints (e.g., streaming clients).
*   **Identity Management:** Authentik handles internal authentication for services supporting OIDC/SAML.

## 🧩 Stack Breakdown

### 1. Edge & Identity
*   **Traefik:** Reverse proxy with dynamic configuration discovery.
*   **Cloudflared:** Secure tunneling daemon.
*   **Authentik:** Self-hosted Identity Provider (IdP).

### 2. Content Aggregation & Delivery
*   **"Arr" Stack:** Automated metadata management (Sonarr, Radarr, Lidarr, Prowlarr).
*   **Riven & Zilean:** Next-gen scraping and debrid content management ecosystem.
*   **AIOStreams:** Specialized API gateway for aggregating media sources into a unified stream.
*   **Plex & Overseerr:** Media server and centralized request management system.

### 3. IoT & Home Automation
*   **Home Assistant:** Central hub for smart home devices (running in Host Networking mode for mDNS discovery).
*   **Scrypted:** High-performance NVR and HomeKit Secure Video bridge.
*   **Homebridge:** Legacy device integration layer.

### 4. Cloud Utilities
*   **Nextcloud:** Self-hosted cloud storage and collaboration platform.
*   **AltServer:** iOS application sideloading utility.
*   **FreshRSS:** Aggregated news feed reader.

## 🚀 Key Technical Challenges Solved

### The "Loop" Issue
**Problem:** Combining Cloudflare's Flexible SSL with Traefik's internal HTTPS redirection caused infinite redirect loops.
**Solution:** Implemented strict origin rules where Cloudflare handles Edge SSL, communicating with the Traefik tunnel over HTTP/1.1, while Traefik manages internal routing without forcing redundant redirects.

### Hybrid Access Policies
**Problem:** Admin dashboards need strict security, but media clients (TVs) cannot perform 2FA.
**Solution:** Configured Cloudflare Access policies to enforce "Admin Access" (SSO) for root domains while creating specific "Bypass" rules for API subdomains used by headless clients, secured via API keys.

### Database Orchestration
**Problem:** Multiple services (Riven, Bitmagnet, Authentik) required Postgres.
**Solution:** Deployed a central Postgres 16 instance with initialization scripts to manage distinct databases and users for each microservice, reducing resource overhead compared to isolated DB instances.

## 🛠 Tech Stack
*   **Infrastructure:** Docker, Docker Compose, Linux (Ubuntu)
*   **Networking:** Traefik, DNS, Cloudflare Tunnel
*   **Scripting:** Bash (Automation), Python
*   **Databases:** PostgreSQL, Redis, SQLite

---
*Note: This repository contains sanitized configuration files. Secrets and sensitive environment variables have been replaced with placeholders.*
