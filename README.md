# Tools Dashboard - Köln Branchen Guide

Eine einfache HTML-Dashboard-Seite, die Links zu drei wichtigen Tools bereitstellt:
- Content Generator
- Teaser Verwaltung  
- PLZ Check

## Deployment auf Linux Server (217.110.253.198)

### Voraussetzungen
- Linux Server mit SSH-Zugang
- Nginx oder Apache Webserver
- Git installiert

### Deployment-Schritte

#### 1. Repository auf den Server klonen
```bash
# Als Benutzer 'adtle' auf dem Server einloggen
ssh adtle@217.110.253.198

# In das gewünschte Verzeichnis wechseln (z.B. /var/www/ oder /home/adtle/)
cd /var/www/

# Repository klonen
git clone https://github.com/[IHR-GITHUB-USERNAME]/tools-dashboard.git

# In das Projektverzeichnis wechseln
cd tools-dashboard
```

#### 2. Nginx Konfiguration (empfohlen)
Erstellen Sie eine neue Nginx-Konfigurationsdatei:

```bash
sudo nano /etc/nginx/sites-available/tools-dashboard
```

Fügen Sie folgende Konfiguration hinzu:
```nginx
server {
    listen 80;
    server_name 217.110.253.198;
    
    root /var/www/tools-dashboard;
    index index.html;
    
    location / {
        try_files $uri $uri/ =404;
    }
    
    # Optional: Gzip Kompression für bessere Performance
    gzip on;
    gzip_types text/html text/css application/javascript;
}
```

Aktivieren Sie die Konfiguration:
```bash
sudo ln -s /etc/nginx/sites-available/tools-dashboard /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```

#### 3. Apache Konfiguration (Alternative)
Falls Sie Apache verwenden:

```bash
sudo nano /etc/apache2/sites-available/tools-dashboard.conf
```

Fügen Sie folgende Konfiguration hinzu:
```apache
<VirtualHost *:80>
    ServerName 217.110.253.198
    DocumentRoot /var/www/tools-dashboard
    
    <Directory /var/www/tools-dashboard>
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
```

Aktivieren Sie die Konfiguration:
```bash
sudo a2ensite tools-dashboard
sudo systemctl reload apache2
```

#### 4. Berechtigungen setzen
```bash
# Stellen Sie sicher, dass der Webserver die Dateien lesen kann
sudo chown -R www-data:www-data /var/www/tools-dashboard
sudo chmod -R 755 /var/www/tools-dashboard
```

### Updates durchführen

Um die Seite zu aktualisieren, führen Sie folgende Befehle auf dem Server aus:

```bash
cd /var/www/tools-dashboard
git pull origin main
```

### Firewall-Konfiguration

Stellen Sie sicher, dass Port 80 (HTTP) geöffnet ist:
```bash
sudo ufw allow 80
sudo ufw status
```

### Zugriff testen

Nach dem Deployment sollte die Seite unter folgender URL erreichbar sein:
- http://217.110.253.198/

### Troubleshooting

#### Nginx Logs überprüfen:
```bash
sudo tail -f /var/log/nginx/error.log
sudo tail -f /var/log/nginx/access.log
```

#### Apache Logs überprüfen:
```bash
sudo tail -f /var/log/apache2/error.log
sudo tail -f /var/log/apache2/access.log
```

#### Webserver-Status überprüfen:
```bash
sudo systemctl status nginx
# oder
sudo systemctl status apache2
```

## Projektstruktur

```
tools-dashboard/
├── index.html          # Haupt-HTML-Datei
├── README.md           # Diese Datei
└── todo.md            # Entwicklungs-Aufgabenliste
```

## Tool-Links

Die Dashboard-Seite verlinkt zu folgenden Tools:
- **Content Generator**: http://content-generator.greven.de:5000/
- **Teaser Verwaltung**: http://217.110.253.198:3000/
- **PLZ Check**: https://grevenmedien.app.n8n.cloud/webhook/7610d874-ae00-4b50-804d-7d5e153de452/chat

## Design

Das Design basiert auf dem "Köln Branchen Guide" Layout mit:
- Rotem Header mit Logo
- Drei blaue Karten-Layout
- Responsive Design für Desktop und Mobile
- Hover-Effekte für bessere Benutzerinteraktion

## Support

Bei Problemen oder Fragen zum Deployment kontaktieren Sie das Entwicklungsteam.

