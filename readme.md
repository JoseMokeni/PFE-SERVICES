# Le Coursier App Infrastructure

This repository contains the Docker infrastructure setup for the Le Coursier application, including PostgreSQL, Redis, pgAdmin, Redis Commander, Mailhog, and Traefik reverse proxy configuration.

## Prerequisites

Before starting the services, you need to create the required Docker networks that allow communication between containers and the reverse proxy.

## Setup Instructions

### 1. Create Docker Networks

First, create the application network for internal service communication:

```bash
docker network create lecoursier
```

Then, create the Traefik public network for reverse proxy routing:

```bash
docker network create traefik-public
```

### 2. Configure Domain Names

The services are configured to use specific domain names. You have two options:

#### Option A: For Local Development

Add the following entries to your `/etc/hosts` file for local development:

```
127.0.0.1 pgadmin.josemokeni.cloud
127.0.0.1 redis.josemokeni.cloud
127.0.0.1 mailhog.josemokeni.cloud
```

#### Option B: Update Domain Configuration

To use your own domains, update the Traefik labels in the `docker-compose.yaml` file:

**For pgAdmin:**

```yaml
- "traefik.http.routers.pgadmin.rule=Host(`pgadmin.your-domain.com`)"
```

**For Redis Commander:**

```yaml
- "traefik.http.routers.redis-commander.rule=Host(`redis.your-domain.com`)"
```

**For Mailhog:**

```yaml
- "traefik.http.routers.mailhog.rule=Host(`mailhog.your-domain.com`)"
```

### 3. Start the Services

Run the following command to start all services:

```bash
docker-compose up -d
```

## Service Access

Once the services are running, you can access them at:

- **pgAdmin**: https://pgadmin.josemokeni.cloud (admin@lecoursier.app / admin)
- **Redis Commander**: https://redis.josemokeni.cloud
- **Mailhog**: https://mailhog.josemokeni.cloud
- **PostgreSQL**: Available on the `lecoursier` network (postgres / postgres)
- **Redis**: Available on the `lecoursier` network
- **Soketi WebSocket Server**: Available on ports 6001 and 9601

## Traefik Configuration

The Traefik reverse proxy is currently commented out in the docker-compose.yaml file. To enable it:

1. Uncomment the traefik service section in `docker-compose.yaml`
2. Update the email address in the ACME configuration for Let's Encrypt certificates
3. Ensure the `acme.json` file has correct permissions:

```bash
chmod 600 docker/traefik/acme.json
```

### SSL Certificates

The configuration uses Let's Encrypt for automatic SSL certificate generation. Make sure to:

- Update the email address in the Traefik configuration
- Ensure your domains point to your server's public IP for certificate validation

## Network Architecture

- **lecoursier**: Internal network for service-to-service communication
- **traefik-public**: Public network for Traefik reverse proxy routing
- **default**: Default Docker network for services that don't need external access
