# ☁️ Full DevOps Cloud Project (AWS EC2 + Docker + Nginx + SSL + DNS)

A complete end-to-end DevOps project deployed on AWS EC2 demonstrating real production architecture using Docker, Nginx reverse proxy, DuckDNS domain, and HTTPS encryption.

---

# 🚀 1. PROJECT OVERVIEW

This project builds a fully cloud-based system with:

- AWS EC2 Ubuntu Server
- Docker containerized application
- Nginx reverse proxy
- DuckDNS free domain
- Let's Encrypt SSL (HTTPS)
- Public cloud access (NO localhost usage in production)

---

# 🏗️ 2. ARCHITECTURE
                    🌍 Internet User
                           │
                           │ HTTPS Request
                           ▼
        ┌──────────────────────────────────┐
        │  blue-ocean.duckdns.org (DNS)    │
        │  (DuckDNS Domain Resolver)       │
        └──────────────────────────────────┘
                           │
                           ▼
        ┌──────────────────────────────────┐
        │   AWS EC2 Public IP Server       │
        │   Ubuntu Cloud Instance          │
        └──────────────────────────────────┘
                           │
                           ▼
        ┌──────────────────────────────────┐
        │        Nginx Reverse Proxy       │
        │  - SSL Termination (HTTPS)       │
        │  - Routing Layer                 │
        └──────────────────────────────────┘
                           │
                           ▼
        ┌──────────────────────────────────┐
        │      Docker Container Layer       │
        │  nginx:alpine / web application   │
        │  Port: 8080                      │
        └──────────────────────────────────┘
                           │
                           ▼
                📄 Static Web Response
        (HTML page served from container)


---

# ☁️ 3. AWS EC2 SETUP

- Launch Ubuntu EC2 instance
- Open Security Group ports:

| Port | Service |
|------|--------|
| 80   | HTTP |
| 443  | HTTPS |
| 8080 | App |

---

# 🐳 4. DOCKER SETUP

### Install Docker
```bash
sudo apt update
sudo apt install docker.io -y
sudo systemctl enable docker
sudo systemctl start docker
Run Application Container
docker run -d --name web-demo -p 8080:80 nginx


Test Container (Cloud IP)
curl http://YOUR_EC2_PUBLIC_IP:8080

🌐 5. NGINX SETUP
Install Nginx
sudo apt install nginx -y
Create Config
sudo nano /etc/nginx/sites-available/myproject

Config Content
server {
    server_name blue-ocean.duckdns.org;

    location / {
        proxy_pass http://YOUR_EC2_PUBLIC_IP:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }

    listen 80;
}

Enable Site
sudo ln -s /etc/nginx/sites-available/myproject /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx

🌍 6. DNS (DUCKDNS)

Update IP
curl "https://www.duckdns.org/update?domains=blue-ocean&token=YOUR_TOKEN&ip="
Verify DNS
dig blue-ocean.duckdns.org +short

🔐 7. SSL (HTTPS)
Install Certbot
sudo apt install certbot python3-certbot-nginx -y

Generate SSL
sudo certbot --nginx -d blue-ocean.duckdns.org

Auto Renew Test
sudo certbot renew --dry-run

🧪 8. TESTING
Local Cloud Test
curl http://YOUR_EC2_PUBLIC_IP:8080
Domain Test
curl http://blue-ocean.duckdns.org
curl https://blue-ocean.duckdns.org

⚙️ 9. APPLICATION UPDATE
docker exec -it web-demo bash
echo "<h1>DevOps Cloud Production System</h1>" > /usr/share/nginx/html/index.html

🔁 10. REQUEST FLOW
User
→ DNS (DuckDNS)
→ AWS EC2 Public IP
→ Nginx Reverse Proxy
→ Docker Container
→ Response

📊 12. FEATURES
☁️ AWS EC2 deployment
🐳 Docker containerization
🌐 Nginx reverse proxy
🔐 HTTPS SSL encryption
🌍 Free DNS (DuckDNS)
⚙️ Linux server management
🧠 Production-level architecture

🌐 14. LIVE SYSTEM
https://blue-ocean.duckdns.org

