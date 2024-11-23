# Nginx Reverse Proxy with SSL Setup

This repository contains a Docker-based Nginx reverse proxy setup with SSL/TLS support using Let's Encrypt certificates.

## Directory Structure

```
nginx-docker-compose/
├── sites-available/          # Nginx site configurations
│   ├── default.conf
│   ├── marcus888.com.conf
│   └── autoresearch.marcus888.com.conf
├── sites-enabled/           # Symlinks to enabled configurations
├── html/                    # Static website content
│   ├── index.html          # Main site content
│   └── autoresearch/       # Autoresearch subdomain content
├── certbot/                # SSL certificates and challenges
│   ├── conf/               # Let's Encrypt configuration and certificates
│   └── www/               # ACME challenge files
├── nginx.conf              # Main Nginx configuration
└── docker-compose.yml      # Docker services configuration
```

## Features

- Secure SSL/TLS configuration with Let's Encrypt certificates
- Separate SSL certificates for each domain and subdomain
- Automatic HTTP to HTTPS redirection
- Modern SSL configuration with TLS 1.2 and 1.3 support
- Security headers including HSTS
- Gzip compression for better performance
- Automatic certificate renewal

## Initial Setup

1. Clone the repository:

```bash
git clone <repository-url>
cd nginx-docker-compose
```

2. Create required directories:

```bash
mkdir -p certbot/conf certbot/www
mkdir -p html/autoresearch
```

3. Start the services:

```bash
docker-compose up -d
```

## SSL Certificate Setup

1. Request certificates for main domain:

```bash
docker-compose exec certbot certbot certonly --webroot \
  -w /var/www/certbot \
  -d marcus888.com \
  -d www.marcus888.com \
  --email admin@marcus888.com \
  --agree-tos \
  --no-eff-email
```

2. Request certificate for subdomain:

```bash
docker-compose exec certbot certbot certonly --webroot \
  -w /var/www/certbot \
  -d autoresearch.marcus888.com \
  --email admin@marcus888.com \
  --agree-tos \
  --no-eff-email
```

## Configuration Details

### SSL Configuration

Each domain has its own SSL configuration with:

- Separate certificates
- Dedicated SSL session cache
- Modern cipher suites
- OCSP stapling
- Strict security headers

Example SSL configuration:

```nginx
ssl_certificate /etc/letsencrypt/live/domain/fullchain.pem;
ssl_certificate_key /etc/letsencrypt/live/domain/privkey.pem;
ssl_trusted_certificate /etc/letsencrypt/live/domain/chain.pem;
ssl_session_cache shared:SSL_domain:10m;
ssl_session_timeout 1d;
ssl_protocols TLSv1.2 TLSv1.3;
```

### Security Headers

All sites include security headers:

```nginx
add_header X-Frame-Options "SAMEORIGIN" always;
add_header X-XSS-Protection "1; mode=block" always;
add_header X-Content-Type-Options "nosniff" always;
add_header Referrer-Policy "no-referrer-when-downgrade" always;
add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
```

## Maintenance

### Certificate Renewal

Certificates are automatically renewed by Certbot container. Manual renewal if needed:

```bash
docker-compose exec certbot certbot renew
```

### Adding New Sites

1. Create site configuration in `sites-available/`:

```bash
nano sites-available/newsite.conf
```

2. Create symbolic link in `sites-enabled/`:

```bash
ln -s ../sites-available/newsite.conf sites-enabled/
```

3. Request SSL certificate:

```bash
docker-compose exec certbot certbot certonly --webroot \
  -w /var/www/certbot \
  -d newsite.com \
  --email admin@marcus888.com \
  --agree-tos
```

4. Reload Nginx:

```bash
docker-compose restart nginx
```

### Troubleshooting

1. Check Nginx configuration:

```bash
docker-compose exec nginx nginx -t
```

2. View Nginx logs:

```bash
docker-compose logs nginx
```

3. Check SSL certificates:

```bash
docker-compose exec certbot certbot certificates
```

## Security Best Practices

1. Keep certificates and private keys secure
2. Regularly update Docker images
3. Monitor certificate expiration
4. Use strong SSL configuration
5. Enable security headers
6. Regular security audits

## Backup

Backup these directories regularly:

- `certbot/conf/` - SSL certificates and configuration
- `sites-available/` - Nginx configurations
- `html/` - Website content

## License

[Your License Here]
