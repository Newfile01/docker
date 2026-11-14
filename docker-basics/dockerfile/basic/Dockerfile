FROM nginx:stable-alpine3.24-slim

# À la place de MAINTAINER → LABEL org.opencontainers.image.authors
LABEL org.opencontainers.image.authors="Nicolas MOREL" \
      description="Small webapp fast deploy"

RUN apk update \
    rm -rf /usr/share/nginx/html/* \
    apk cache purge

WORKDIR /usr/share/nginx/html/
COPY static-website-example/ .

EXPOSE 80

ENTRYPOINT ["/usr/sbin/nginx", "-g", "daemon off;"]