# Nginx Docker Compose Setup with SSL

This repository contains a Docker Compose configuration for running Nginx with automatic SSL certificate management using Certbot.

## Features

- Nginx web server with SSL support
- Automatic SSL certificate management with Certbot
- Docker Compose setup for easy deployment
- Support for multiple domains and services

## Prerequisites

- Docker
- Docker Compose
- Domain name pointed to your server

## Directory Structure

```
.
├── docker-compose.yml    # Docker compose configuration
├── nginx.conf           # Nginx server configuration
├── html/               # Static website files
├── certbot/            # SSL certificates and challenges
    ├── conf/          # Certificate storage
    └── www/           # ACME challenge files
```

## Setup Instructions

1. Clone this repository:

   ```bash
   git clone https://github.com/marcus888-lab/nginx-docker-compose.git
   cd nginx-docker-compose
   ```

2. Configure your domain in `nginx.conf`

3. Start the services:

   ```bash
   docker-compose up -d
   ```

4. SSL certificates will be automatically obtained and renewed by Certbot

## Configuration

### Docker Compose

The `docker-compose.yml` file defines two services:

- `nginx`: Web server with SSL support
- `certbot`: Automatic SSL certificate management

### Nginx Configuration

The `nginx.conf` file contains the server configuration including:

- SSL settings
- Domain configurations
- Proxy settings for other services

## SSL Certificates

SSL certificates are automatically managed by Certbot:

- Initial certificate acquisition
- Automatic renewal every 12 hours
- Storage in `certbot/conf/`

## Adding New Services

To add a new service:

1. Uncomment and modify the example service in `docker-compose.yml`
2. Add the corresponding network configuration
3. Update the Nginx configuration to proxy to the new service

## License

MIT License
