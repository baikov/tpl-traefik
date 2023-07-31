# Traefik 2.10

Other parts:

1. [Traefik 2.10 as revers-proxy in Docker (SSL in dev and prod)](https://github.com/baikov/tpl-traefik)
2. [Nuxt 3 production-ready template in Docker (SPA/SSR)](https://github.com/baikov/tpl-nuxt3)
3. [Django/DRF backend in Docker (based on django-cookiecutter)](https://github.com/baikov/drf-tpl)

## Global reverse-proxy for local development

...

## Production reverse-proxy in Docker

...

## Features
- A+ on ssllabs
- Redirect 301 for SEO from www to non-www and from http to https without redirect chains:
    - http://domain.com -> https://domain.com
    - http://www.domain.com -> https://domain.com
    - https://www.domain.com -> https://domain.com
