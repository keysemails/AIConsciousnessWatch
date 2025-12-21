# AI Consciousness Watch

Build and run the webapp.

```
cd frontend
npm run build
PORT=4173 pm2 start npm --name "acw" -- run preview
```

Nginx configuration.

Create `/etc/nginx/sites-available/react-apps` and add following:
```nginx
# AI Consciousness Watch - acw.gixia.org
server {
    listen 80;
    server_name acw.gixia.org;
    
    location / {
        proxy_pass http://localhost:4173;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_cache_bypass $http_upgrade;
    }
}
```

Use Certbot to automatically enable the HTTPS connection.
```bash
sudo apt update && sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d acw.gixia.org
```

Enable Nginx server.

```bash
sudo ln -s /etc/nginx/sites-available/react-apps /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```