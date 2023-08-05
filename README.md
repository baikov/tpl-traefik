# Traefik 2.10

Other parts:

1. [Traefik 2.10 as revers-proxy in Docker (SSL in dev and prod)](https://github.com/baikov/tpl-traefik)
2. [Nuxt 3 production-ready template in Docker (SPA/SSR)](https://github.com/baikov/tpl-nuxt3)
3. [Django/DRF backend in Docker (based on django-cookiecutter)](https://github.com/baikov/drf-tpl)

## Global reverse-proxy for local development

### Local development without https

1. Rename `.env.example` to `.env` and change `COMPOSE_FILE` variable to `COMPOSE_FILE=local.yml`
1. Run `docker compose build` and `docker compose up -d`

Traefik dashboard: http://127.0.0.1:8080/dashboard/#/

### Local development with custom domain and https

#### MacOS

1. Add local domain + subdomain for dashboard in `/etc/hosts`:
    ```vim
    127.0.0.1 localhost tpl.local tr.tpl.local
    ```
1. Apply changes:
    ```bash
    sudo killall -HUP mDNSResponder
    ```
1. Install `mkcert` (or another tolls for local certificates)
1. Generate local CA certificate with the command:
    ```bash
    mkcert -install
    ```
1. Go to `./compose/local-ssl/cert/`
1. Generate a wildcard certificate for a local domain:
    ```bash
    mkcert "*.tpl.local"  # * for wildcard
    ```
1. Go to `./compose/local-ssl/dynamic/` and edit `tls.yml` and change the file names to your own
1. Rename `.env.example` to `.env` and change variables:
    - `COMPOSE_FILE=local.ssl.yml`
    - `COMPOSE_PROJECT_NAME=tpl` - just name in Docker Desktop
    - `DOMAIN=${COMPOSE_PROJECT_NAME}.local` - same as your domain from previouse step
1. Run `docker compose build` and `docker compose up -d`

Traefik dashboard: https://tr.tpl.local/dashboard/#/

#### Windows

...in process


## Production reverse-proxy in Docker

1. Rename `.env.example` to `.env` and change variables:
    - `COMPOSE_FILE=production.yml`
    - `COMPOSE_PROJECT_NAME=prod-domain` - just name in Docker Desktop
    - `DOMAIN=${COMPOSE_PROJECT_NAME}.com` - same as your production domain
    - generate new login:password for dashboard: `DASHBOARD_LOG_PSW=test:$$apr1$$ozzsQDHl$$wBwHPFJtpA9UQkIE4mXh2/`
1. Run `docker compose build` and `docker compose up -d`
1. If the domain is correctly bound to the server, the certificate will be issued automatically with Let's Encrypt

Traefik dashboard: https://tr.prod-domain.com/dashboard/#/
