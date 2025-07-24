# Remote Home Assistant Setup with Docker, NGINX and DuckDNS

Personal project for deploying a secure remote-access instance of Home Assistant using Docker containers, reverse proxy (NGINX), and dynamic DNS via DuckDNS.

## 🔧 Technologies Used
- Docker + Docker Compose  
- NGINX (reverse proxy)  
- Let's Encrypt (SSL certificates)  
- DuckDNS (dynamic DNS service)  
- Home Assistant  
- YAML configuration  

## 🚀 Features
- Remote access to Home Assistant over HTTPS  
- Automated SSL certificate generation via Let's Encrypt  
- Secure reverse proxy with NGINX  
- Containerized service isolation using Docker Compose  
- Fully local and self-hosted deployment  

## 📷 Screenshots

## ⚠️ Important Note

This repository excludes private credentials, secrets and SSL certificates for security reasons.

If you want to replicate this project:
- Replace `secrets.yaml` with your own version
- Obtain your own DuckDNS token and configure `duckdns.conf`
- Set up Let's Encrypt manually following the official documentation
## ❔ Remote Home Assistant Setup with Docker, NGINX and DuckDNS

This guide describes the step-by-step process to deploy Home Assistant for remote access using Docker containers, NGINX as a reverse proxy, and Let's Encrypt SSL certificates via DuckDNS.

## 📁 Step 1: Initial Folder Structure

In your terminal (PowerShell or Bash):

```bash
mkdir certbot certbot/www certbot/conf nginx nginx/conf.d

