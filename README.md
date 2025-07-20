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
*(Add screenshots here if available)*

## 👨‍💻 Author
**Pablo Varela Mille**  
[LinkedIn Profile](https://www.linkedin.com/in/pvpeuve)

## ⚠️ Important Note

This repository excludes private credentials, secrets and SSL certificates for security reasons.

If you want to replicate this project:
- Replace `secrets.yaml` with your own version
- Obtain your own DuckDNS token and configure `duckdns.conf`
- Set up Let's Encrypt manually following the official documentation
