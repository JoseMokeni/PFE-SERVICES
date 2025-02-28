# Le Coursier App Infra

Before running `docker-compose up -d` make sure to create the lecoursier network with the following command:

```bash
docker network create lecoursier
```

After that, create the traefik-public network with the following command:

```bash
docker network create traefik-public
```

And then run the following command:

```bash
docker-compose up -d
```

## Hosts to add to your /etc/hosts file

```
127.0.0.1 lecoursier.local
127.0.0.1 traefik.lecoursier.local
```
