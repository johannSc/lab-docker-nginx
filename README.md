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

You should get the **Welcome to nginx** web page.

## Configuration

Configuration directives's hierachy:
- `main` context contains `events` and `http`
- `http` contains `server`
- `server` contains `location`

Loading a minimal configuration (see `./nginx.conf` for details):
```
docker run --rm -v $(pwd)/nginx.conf:/etc/nginx/nginx.conf:ro -v $(pwd)/data:/data:ro -p80:80 nginx
```

Then you should be able to get:
- http://localhost
- http://localhost/images/example.png

## Proxy server

See `nginx-proxy.conf`.

```
docker run --rm -v $(pwd)/nginx-proxy.conf:/etc/nginx/nginx.conf:ro -v $(pwd)/data:/data:ro -p80:80 nginx
curl localhost
```

## Requêtes locales

See `nginx-regex.conf`.

```
docker run --rm -v $(pwd)/nginx-regex.conf:/etc/nginx/nginx.conf:ro -v $(pwd)/data:/data:ro -p80:80 nginx
```

- http://localhost/example.png

## Load balancing

See `nginx-lb.conf`.

```
docker run --rm -d -v $(pwd)/nginx-lb.conf:/etc/nginx/nginx.conf:ro -v $(pwd)/data:/data:ro -p 80:80 nginx
```

Now if you keep reading from http://localhost, it will respond with these responses in round-robin order:

- hello from site 1
- hello from site 2
- hello from site 3
