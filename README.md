# nginx lab 

- [Récupération de l'image](#récupération-de-l'image)
- [Lancement Docker Run](#lancement-docker-run)
- [Configuration](#configuration)
- [Proxy server](#proxy-server)
- [Requêtes locales](#requêtes_locales)
- [Load balancing](#load-balancing)

## Récupération de l'image

```
docker pull nginx
```

## Lancement Docker Run

```
docker run --rm -d -p 80:80 nginx
curl localhost
```

Vous devriez vois la page d'accueil **Welcome to nginx**

## Configuration

Les répertoires de config:
- `main` context contains `events` and `http`
- `http` contains `server`
- `server` contains `location`

Lancement de la configuration minimale (voir `./nginx.conf` pour plus de details):
```
docker run --rm -v $(pwd)/nginx.conf:/etc/nginx/nginx.conf:ro -v $(pwd)/data:/data:ro -p80:80 nginx
```

Vous devriez avoir accès aux urls suivantes:
- http://localhost
- http://localhost/images/example.png

## Proxy server

Voir `nginx-proxy.conf`.

```
docker run --rm -v $(pwd)/nginx-proxy.conf:/etc/nginx/nginx.conf:ro -v $(pwd)/data:/data:ro -p80:80 nginx
curl localhost
```

## Requêtes locales

Voir `nginx-regex.conf`.

```
docker run --rm -v $(pwd)/nginx-regex.conf:/etc/nginx/nginx.conf:ro -v $(pwd)/data:/data:ro -p80:80 nginx
```

- http://localhost/example.png

## Load balancing

Voir `nginx-lb.conf`.

```
docker run --rm -d -v $(pwd)/nginx-lb.conf:/etc/nginx/nginx.conf:ro -v $(pwd)/data:/data:ro -p 80:80 nginx
```
Maintenant si vous rechargez la page http://localhost, devrait répondre de manière aléatoire (via du round-robin):

- hello from site 1
- hello from site 2
- hello from site 3
