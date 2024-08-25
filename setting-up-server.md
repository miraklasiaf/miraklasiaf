# Setting up a server

## Pre-requisites:

1. Buy a domain
2. Generate an ssh key

```
ssh-keygen -t ed25519 -C "miraklasiaf@gmail.com" -f ~/.ssh/ed25519_azure
```

## Initial setup:

1. Register on Azure Portal and buy a server
   - Copy the IP Address
2. Set up on Cloudflare
   - add new site and use the the domain
   - Add an A record with the name "@" and the IP you copied from Azure earlier
   - Under SLL/TLS settings, make sure encryption mode is set to "Full"

## Configure your domain with nginx:

ssh into your server.

```
ssh -i ~/.ssh/ed25519_azure <user>@<server-ip-address>
```

1. Install nginx

```
sudo apt update
sudo apt install nginx
```

2. Enable on boot

```
sudo systemctl start nginx
sudo systemctl enable nginx
```

3. Add domain
   - create a config `sudo nano /etc/nginx/sites-available/your_domain.com`
   - And paste the following config (note: port is set to 3000, you might want to change that):

```
server {
  listen 80:
  server_name your_domain.com www.your_domain.com;

  location / {
    proxy_pass http://localhost:YOUR_APP_PORT;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_cache_bypass $http_upgrade;
  }
}
```

## Enable HTTPS

1. Install Certbot
2. Obtain SSL cert

`sudo certbot --nginx -d your_domain.com -d www.your_domain.com`

3. Verify

`sudo certbot renew --dry-run`

## Integrate a Github Repo / CI CD

-- wip
