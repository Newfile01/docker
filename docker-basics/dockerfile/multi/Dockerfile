FROM nginx:stable-alpine3.24-slim AS build
RUN apk add --no-cache git \
    && git clone --depth 1 https://github.com/eazytraining/docker-training.git /src \
    && cp -r /src/02_lab-2/webapp/static-website-example/. /usr/share/nginx/html

FROM nginx:stable-alpine3.24-slim
LABEL org.opencontainers.image.authors="Nicolas MOREL" \
      description="Small webapp - fast deploy"
RUN sed -i \
        -e 's|pid\s*/run/nginx.pid;|pid /tmp/nginx.pid;|' \
        -e 's|error_log\s*/var/log/nginx/error.log notice;|error_log /dev/stderr notice;|' \
        -e 's|access_log\s*/var/log/nginx/access.log  main;|access_log /dev/stdout main;|' \
        /etc/nginx/nginx.conf \
    && sed -i 's/listen\s*80;/listen 8080;/' /etc/nginx/conf.d/default.conf \
    && mkdir -p /var/cache/nginx/client_temp \
        /var/cache/nginx/proxy_temp \
        /var/cache/nginx/fastcgi_temp \
        /var/cache/nginx/uwsgi_temp \
        /var/cache/nginx/scgi_temp \
    && chown -R nginx:nginx /var/cache/nginx \
    && rm -rf /usr/share/nginx/html/*
COPY --from=build --chown=root:root /usr/share/nginx/html/ /usr/share/nginx/html
USER nginx
EXPOSE 8080
ENTRYPOINT ["/usr/sbin/nginx", "-g", "daemon off;"]

#  Sécurisation de l'image → Utilisateur Nginx
#  logs erreurs & requêtes HTTP vers stderr & stdout → pas d'accumulation sur le disque conteneur
#  Exécution sur port non-root de nginx avec le bon utilisateur