# Hybrid Cloud Homelab Infrastructure

A containerized microservices infrastructure designed for automated content aggregation, secure remote access, and IoT orchestration. 

This project demonstrates a production-grade approach to self-hosting, utilizing **Zero Trust networking**, **Reverse Proxies**, and **Infrastructure-as-Code** (Docker Compose).

## 🏗 Architecture Overview

*   **Ingress & Security:** 
    *   **Traefik v3:** Dynamic reverse proxy handling SSL/TLS termination and load balancing.
    *   **Cloudflare Tunnel:** Exposes services securely without opening firewall ports (CGNAT friendly).
    *   **Cloudflare Zero Trust:** Implements Identity-Aware Proxy (IAP) policies, requiring SSO for admin dashboards while allowing public bypass for specific API endpoints.
*   **Identity Management:**
    *   **Authentik:** Centralized Identity Provider (IdP) for internal services.
*   **Content Operations:**
    *   Automated metadata aggregation stack (Sonarr/Radarr/Prowlarr).
    *   Centralized request management (Overseerr).
*   **IoT & Automation:**
    *   Home Assistant & Scrypted running in host networking mode for local device discovery (mDNS).

## 🚀 Key Features

*   **Hybrid Networking:** Solved "Redirect Loop" issues between Cloudflare Edge and local Traefik instances by implementing strict HTTP/HTTPS origin rules.
*   **Security hardening:** Admin services are isolated behind GitHub/Email SSO. API endpoints utilize IP-based rate limiting and caching.
*   **Automated Maintenance:** Scripts included for automated container updates and health checks.

## 🛠 Tech Stack
*   **Containerization:** Docker, Docker Compose
*   **Networking:** Traefik, Cloudflare Tunnel, DNS
*   **Scripting:** Bash, Python
*   **Database:** PostgreSQL, Redis
