
## 1. **Install NGINX**

```bash
sudo apt update
sudo apt install nginx -y
```

Check NGINX status:

```bash
sudo systemctl status nginx
```

Enable it to start on boot:

```bash
sudo systemctl enable nginx
```

---

## 2. **Adjust Firewall for NGINX**

Since you already have UFW:

```bash
sudo ufw allow 'Nginx Full'
sudo ufw status
```

* This allows HTTP (80) and HTTPS (443) traffic.

---

## 3. **Create NGINX Server Block**

Create a configuration file for your domain:

```bash
sudo nano /etc/nginx/sites-available/itsrighttime.group
```

Paste the following basic config:

```nginx
server {
    listen 80;
    listen [::]:80;

    server_name itsrighttime.group www.itsrighttime.group;

    root /var/www/itsrighttime.group/html;
    index index.html index.htm index.nginx-debian.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

---

### 3.1 Create Web Root Directory

```bash
sudo mkdir -p /var/www/itsrighttime.group/html
sudo chown -R $USER:$USER /var/www/itsrighttime.group/html
sudo chmod -R 755 /var/www/itsrighttime.group
```

Add a test HTML page:

```bash
echo "<h1>itsRIGHTtime Server is Live</h1>" > /var/www/itsrighttime.group/html/index.html
```

---

### 3.2 Enable Site

```bash
sudo ln -s /etc/nginx/sites-available/itsrighttime.group /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```

Now visiting `http://itsrighttime.group` should show the test page.

---

## 4. **Optional: Reverse Proxy for Node.js App**

If you have a Node.js app running on port `3000` via PM2:

Edit the server block:

```bash
sudo nano /etc/nginx/sites-available/itsrighttime.group
```

Update the `location` section:

```nginx
location / {
    proxy_pass http://localhost:3000;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_cache_bypass $http_upgrade;
}
```

Test & reload NGINX:

```bash
sudo nginx -t
sudo systemctl reload nginx
```

---

## 5. **Enable HTTPS with Let’s Encrypt**

Install Certbot:

```bash
sudo apt install certbot python3-certbot-nginx -y
```

Run Certbot to generate SSL certificates:

```bash
sudo certbot --nginx -d itsrighttime.group -d www.itsrighttime.group
```

* Follow prompts to configure HTTPS automatically.
* Certbot sets up automatic certificate renewal.

Verify renewal:

```bash
sudo certbot renew --dry-run
```

---

## 6. **Optional NGINX Optimization**

* Enable Gzip compression:

```bash
sudo nano /etc/nginx/nginx.conf
```

Uncomment/add:

```
gzip on;
gzip_types text/plain text/css application/javascript application/json image/svg+xml;
```

* Enable caching and security headers in server block:

```nginx
add_header X-Frame-Options SAMEORIGIN;
add_header X-Content-Type-Options nosniff;
add_header X-XSS-Protection "1; mode=block";
```

