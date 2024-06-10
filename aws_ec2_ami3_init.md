# Basic setup

### Docker
```bash
# install
sudo dnf update
sudo dnf install docker
# start docker
sudo systemctl start docker
# autostart
sudo systemctl enable docker
# enable without sudo 
sudo usermod -aG docker $USER
newgrp docker

# docker compose
sudo curl -L https://github.com/docker/compose/releases/v2.24.5/download/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
sudo curl -L https://github.com/docker/compose/releases/download/v2.23.3/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose version
```

### Nginx
```bash
# install
sudo yum install nginx
sudo systemctl enable nginx && sudo systemctl start nginx

# after changing config
sudo sudo nginx -t # test config
sudo systemctl restart nginx # reload
```

### Install Cerbot for SSL
```bash
# install
sudo python3 -m venv /opt/certbot/
sudo /opt/certbot/bin/pip install --upgrade pip
sudo /opt/certbot/bin/pip install certbot certbot-nginx
sudo ln -s /opt/certbot/bin/certbot /usr/bin/certbot
```

### Generate SSL with certbot
```bash
sudo certbot --nginx -d domain.com -d www.domain.com
```

### Install crontab for auto renew
```bash
sudo yum install cronie -y
sudo systemctl enable crond.service
sudo systemctl start crond.service
sudo crontab -e
# add line
0 0 * * * /usr/bin/certbot renew --quiet --post-hook "systemctl reload nginx"
```
