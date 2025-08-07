# Schnelle Deployment-Anleitung

## Für Benutzer 'adtle' auf Server 217.110.253.198

### 1. Repository klonen
```bash
ssh adtle@217.110.253.198
cd /var/www/
git clone [IHR-GITHUB-REPOSITORY-URL] tools-dashboard
cd tools-dashboard
```

### 2. Nginx konfigurieren
```bash
sudo nano /etc/nginx/sites-available/tools-dashboard
```

Inhalt:
```nginx
server {
    listen 80;
    server_name 217.110.253.198;
    root /var/www/tools-dashboard;
    index index.html;
    location / {
        try_files $uri $uri/ =404;
    }
}
```

### 3. Aktivieren
```bash
sudo ln -s /etc/nginx/sites-available/tools-dashboard /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```

### 4. Berechtigungen
```bash
sudo chown -R www-data:www-data /var/www/tools-dashboard
sudo chmod -R 755 /var/www/tools-dashboard
```

### 5. Zugriff testen
http://217.110.253.198/

### Updates
```bash
cd /var/www/tools-dashboard
git pull origin main
```

