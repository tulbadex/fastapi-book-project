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
worker_processes auto;
events {
    worker_connections 1024;
}

http {
    server {
        listen 80;

        location / {
            proxy_pass http://fastapi_app:8000;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }

        error_log /var/log/nginx/error.log warn;
        access_log /var/log/nginx/access.log;
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

## Creating a public key of the ssh key
```bash
# Open Git Bash and navigate to your .pem file location
cd /path/to/your/key

# Generate public key from private key

ssh-keygen -y -f hng12-2025.pem > hng12-2025.pub
```

## Display and copy the public key content:
```bash
cat hng12-2025.pub
```

## Connect to your EC2 instance using your .pem file:
```bash
ssh -i hng12-2025.pem ubuntu@your-ec2-ip
```

## Once connected to EC2, set up the authorized_keys:
```bash
# Create .ssh directory
mkdir -p ~/.ssh
chmod 700 ~/.ssh

# Create authorized_keys file
touch ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys

# Add your public key
# Copy the content from hng12-2025.pub and paste it into this command:
echo "paste-your-public-key-here" >> ~/.ssh/authorized_keys
```