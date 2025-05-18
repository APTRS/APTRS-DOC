# SSL Certificate Management

## Default Configuration

APTRS uses HTTPS with a self-signed certificate by default. While this provides encryption, browsers will display security warnings because the certificate isn't issued by a trusted certificate authority (CA).

## Replacing the Self-Signed Certificate

You can replace the default self-signed certificate with your own certificate in one of two ways:

### Option 1: Replace Certificate Files in Docker

If you're using the Docker deployment method, you can replace the certificate files located at:

[https://github.com/APTRS/APTRS/tree/main/Certificate](https://github.com/APTRS/APTRS/tree/main/Certificate)

You'll need to replace both:
- `certificate.crt` - Your SSL certificate file
- `certificate.key` - Your private key file

After replacing these files, rebuild and restart your Docker containers:

```powershell
docker-compose down
docker-compose build
docker-compose up -d
```

### Option 2: Use Nginx as SSL Termination Proxy (Recommended)

For production environments, we recommend using Nginx as an SSL termination proxy in front of APTRS. This method allows you to:

1. Use professionally issued certificates (e.g., Let's Encrypt)
2. Manage certificate renewals outside of the APTRS containers
3. Configure advanced SSL settings for optimal security

#### Steps for Nginx SSL Configuration:

1. Obtain a valid SSL certificate (e.g., using Let's Encrypt certbot)
2. Configure Nginx with your certificate
3. Set up Nginx to proxy requests to APTRS

Example Nginx configuration with SSL:

```nginx
server {
    listen 80;
    server_name your-aptrs-domain.com;

    # Redirect HTTP to HTTPS
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl;
    server_name your-aptrs-domain.com;

    # SSL Certificate Configuration
    ssl_certificate     /path/to/your/fullchain.pem;
    ssl_certificate_key /path/to/your/privkey.pem;
    ssl_protocols       TLSv1.2 TLSv1.3;
    ssl_ciphers         HIGH:!aNULL:!MD5;

    # Proxy to APTRS
    location / {
        proxy_pass http://localhost:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

## Using Let's Encrypt for Free SSL Certificates

For production environments, [Let's Encrypt](https://letsencrypt.org/) provides free, trusted SSL certificates that are automatically renewable.

### Installing Certbot for Let's Encrypt:

```powershell
# For Ubuntu/Debian systems
sudo apt update
sudo apt install certbot python3-certbot-nginx

# For CentOS/RHEL systems
sudo dnf install certbot python3-certbot-nginx
```

### Obtaining a Certificate:

```powershell
sudo certbot --nginx -d your-aptrs-domain.com
```

This command will:
1. Obtain a certificate
2. Automatically configure your Nginx settings
3. Set up automatic renewal

!!! tip "Certificate Renewal"
    Let's Encrypt certificates expire after 90 days. Certbot installs a cron job or systemd timer to automatically renew your certificates before they expire.

## SSL Certificate Best Practices

1. **Use Strong Protocols**: Enable only TLSv1.2 and TLSv1.3
2. **Configure Forward Secrecy**: Use ECDHE and DHE cipher suites
3. **Implement HSTS**: Add Strict-Transport-Security header
4. **Test Your Configuration**: Use [SSL Labs](https://www.ssllabs.com/ssltest/) to verify your setup
5. **Automate Renewals**: Ensure certificates are automatically renewed before expiration