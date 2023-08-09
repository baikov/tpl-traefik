# Traefik 2.10

Other parts:

1. [Traefik 2.10 as revers-proxy in Docker (SSL in dev and prod)](https://github.com/baikov/tpl-traefik)
2. [Nuxt 3 production-ready template in Docker (SPA/SSR)](https://github.com/baikov/tpl-nuxt3)
3. [Django/DRF backend in Docker (based on django-cookiecutter)](https://github.com/baikov/drf-tpl)

## Improvement plan

- [ ] Add log rotation
- [ ] Integrate [Socket-Proxy](https://medium.com/@containeroo/traefik-2-0-paranoid-about-mounting-var-run-docker-sock-22da9cb3e78c) for security reason

## Global reverse-proxy for local development

> Creates an external network `dev-proxy` to which frontend and backend containers are connected.

### Mode 1: local development without https

1. Rename `.env.example` to `.env`
1. Set project name (the same for Traefik repo, Nuxt repo and Django repo if use all stack)
    ```env
    COMPOSE_PROJECT_NAME=your_proj_name
    ```
1. Uncomment `Mode 1` block:
    ```env
    # 1. As reverse-proxy for development on localhost with http
    COMPOSE_FILE=local.yml
    DOMAIN=localhost  # or another aliace for 127.0.0.1 declared in etc/hosts
    ```
1. Run `docker compose build` and `docker compose up -d`

Traefik dashboard is available at: http://localhost:8080/dashboard/#/

### Mode 2: local development with custom domain and https

1. Rename `.env.example` to `.env`
1. Set project name (the same for Traefik repo, Nuxt repo and Django repo if use all stack)
    ```env
    COMPOSE_PROJECT_NAME=your_proj_name
    ```
1. Uncomment `Mode 2` block:
    ```env
    # Mode 2: SSL-mode (with custom domain)
    COMPOSE_FILE=local.ssl.yml
    DOMAIN=tpl.local
    ```
1. Add local domain + subdomain for dashboard in `/etc/hosts`:
    ```vim
    127.0.0.1 localhost tpl.local tr.tpl.local
    ```
1. Apply changes:
    ```bash
    sudo killall -HUP mDNSResponder
    ```
1. Install `mkcert` (or another tolls for local certificates)
1. Go to `./compose/local-ssl/cert/`
1. Generate a wildcard certificate for a local domain:
    ```bash
    mkcert -cert-file tpl.local.pem -key-file tpl.local-key.pem tpl.local "*.tpl.local"  # * for subdomains
    ```
1. Install local CA certificate with the command:
    ```bash
    mkcert -install
    ```
1. If you use not `tpl.local` - go to `./compose/local-ssl/dynamic/` and edit `tls.yml`. Change the file names to your own
1. Run `docker compose build` and `docker compose up -d`

Traefik dashboard is available at: https://tr.tpl.local/dashboard/#/

## Production reverse-proxy in Docker (Mode 3)

> Using this mode assumes that the `your-domain.com` is already bound to your server (`A` records are configured).

> Creates an external network `global` to which frontend and backend containers are connected.

1. Rename `.env.example` to `.env`
1. Set project name (the same for Traefik repo, Nuxt repo and Django repo if use all stack)
    ```env
    COMPOSE_PROJECT_NAME=your_proj_name
    ```
1. Uncomment `Mode 3` block and change domain and email:
    ```env
    # Mode 3: As reverse-proxy for production
    COMPOSE_FILE=production.yml
    DOMAIN=your-domain.com
    EMAIL=emailfor@letsencrypt.com  # for Let's Encrypt resolver
    ```
1. Run `docker compose build` and `docker compose up -d`
1. If the domain is correctly bound to the server, the certificate will be issued automatically with Let's Encrypt

Traefik dashboard is available at: https://tr.your_domain.com/dashboard/#/

## Contributing

I made this template for myself, but it's awesom if it helps someone else. The settings are far from ideal, so fell free to make a pull request.
