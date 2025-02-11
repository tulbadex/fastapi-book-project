## Enable Docker Auto-Start on AWS EC2
```bash
sudo systemctl enable docker
```

## Add Your User to the docker Group

```bash
sudo usermod -aG docker $USER
newgrp docker  # Refresh group permissions
```

## If you're using Docker Compose as a systemd service, create a service file:
```bash
sudo nano /etc/systemd/system/docker-compose-app.service
```

```bash
[Unit]
Description=Docker Compose Application
Requires=docker.service
After=docker.service

[Service]
WorkingDirectory=/home/ubuntu/fastapi-book-project
ExecStart=/usr/local/bin/docker-compose up --build -d
ExecStop=/usr/local/bin/docker-compose down
Restart=always

[Install]
WantedBy=multi-user.target
```

## for docker, use this nginx
```nginx
server {
    listen 80;

    location / {
        proxy_pass http://fastapi:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

## Remove nginx on AWS EC2 instance
```bash
sudo rm /etc/nginx/sites-enabled/fastapi
sudo nginx -t
sudo systemctl restart nginx
sudo systemctl status nginx
```