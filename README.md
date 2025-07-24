# Remote Home Assistant Server with Docker, NGINX and DuckDNS

Personal project for deploying a secure remote-access instance of Home Assistant using Docker containers, reverse proxy (NGINX), and dynamic DNS via DuckDNS.

## ⚠️ Important Note
This repository excludes private credentials, secrets and SSL certificates for security reasons.

If you want to replicate this project:
- Replace `secrets.yaml` with your own version
- Obtain your own DuckDNS token and configure `duckdns.conf`
- Set up Let's Encrypt manually following the official documentation

## 🔧 Technologies Used
- Docker (Container service) 
- NGINX (reverse proxy)  
- Let's Encrypt (SSL certificates)  
- DuckDNS (dynamic DNS service)  
- Home Assistant (main app)

## 🚀 Features
- Remote access to Home Assistant over HTTPS  
- Automated SSL certificate generation via Let's Encrypt  
- Secure reverse proxy with NGINX  
- Containerized service isolation using Docker Compose  
- Fully local and self-hosted deployment  

## 📷 Screenshots


## ❔ Setup Guide
This guide describes the step-by-step process to deploy Home Assistant for remote access using Docker containers, NGINX as a reverse proxy, and Let's Encrypt SSL certificates via DuckDNS.

### Step 1: Initial Folder Structure (*/*)

```powershell
mkdir certbot certbot/www certbot/conf nginx nginx/conf.d
```

### Step 2: Compose File Configuration (*docker-compose.yaml*)

```yaml
services:
  homeassistant:
    image: homeassistant/home-assistant:stable
    container_name: homeassistant
    volumes:
      - ./homeassistant:/config
    networks:
      - {network}
    restart: unless-stopped

  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx/conf.d:/etc/nginx/conf.d
      - ./certbot/www:/var/www/certbot
      - ./certbot/conf:/etc/letsencrypt
    depends_on:
      - homeassistant
    networks:
      - {network}
    restart: unless-stopped

  duckdns:
    image: linuxserver/duckdns
    environment:
      - SUBDOMAINS={subdomain}
      - TOKEN={token}
    networks:
      - {**network}
    restart: unless-stopped

  certbot:
    image: certbot/certbot
    volumes:
      - ./certbot/www:/var/www/certbot
      - ./certbot/conf:/etc/letsencrypt
    entrypoint: "/bin/sh -c 'trap exit TERM; while :; do certbot renew; sleep 12h & wait $${!}; done;'"
    networks:
      - {network}
    restart: unless-stopped

networks:
  {network}:
    driver: bridge
```

### Step 3: Pre-SSL NGINX Configuration (*nginx/conf.d/default.conf*)

```nginx
server {
    listen 80;
    server_name {subdomain}.duckdns.org;
    
    location /.well-known/acme-challenge/ {
        root /var/www/certbot;
    }
    
    location / {
        return 301 https://$host$request_uri;
    }
}
```

### Step 4: Start the Stack

```powershell
docker-compose up --build -d
```

### Step 5: Obtain SSL Certificates

```powershell
docker exec certbot certbot certonly --webroot -w /var/www/certbot -d {subdomain}.duckdns.org --email {email} --agree-tos --non-interactive
```

### Step 6: Stop Everything

```powershell
docker-compose down
```

### Step 7: Post-SSL NGINX Configuration (*nginx/conf.d/default.conf*)

```nginx
server {
    listen 80;
    server_name {subdomain}.duckdns.org;

    location /.well-known/acme-challenge/ {
        root /var/www/certbot;
    }

    location / {
        return 301 https://$host$request_uri;
    }
}

server {
    listen 443 ssl;
    server_name {subdomain}.duckdns.org;

    ssl_certificate /etc/letsencrypt/live/{subdomain}.duckdns.org/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/{subdomain}.duckdns.org/privkey.pem;

    location / {
        proxy_pass http://homeassistant:8123;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

### Step 8: Home Assistant Configuration (*homeassistant/configuration.yaml*)

```yaml
# Loads default set of integrations. Do not remove.
default_config:

# Load frontend themes from the themes folder
frontend:
  themes: !include_dir_merge_named themes

automation: !include automations.yaml
script: !include scripts.yaml
scene: !include scenes.yaml

http:
  use_x_forwarded_for: true
  trusted_proxies:
    - 172.18.0.5
```

## 👨‍💻 Author
**Pablo Varela Mille (pvpeuve)**  
IoT Technician & Junior Python Developer focused on smart automation, edge computing and secure home infrastructure.  
🔗 [LinkedIn](https://www.linkedin.com/in/pvpeuve)  
💻 [GitHub](https://github.com/pvpeuve)  
📧 [Mail](userandroidsp@gmail.com)  
📱 [Mobile](+346026046086)  

*Feel free to fork this project or reach out for collaboration ideas.*
