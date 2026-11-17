
# Déploiement d'un registre d'images docker personnalisé
```bash
# Déploiement (volume à rajouter pour persistance + gestion https éventuelle)
docker network create regnet
docker run -d -p 5000:5000 --restart always --network regnet --name registry registry:3
docker run -d -p 8090:80 --restart always -e NGINX_PROXY_PASS_URL=http://registry:5000 -e REGISTRY_STORAGE_DELETE_ENABLED=true --network regnet --name reg_webui joxit/docker-registry-ui:latest
# Push local
docker pull nginx
docket tag nginx:latest localhost:5000/nginx:local
docker push localhost:5000/nginx:local
# Push distant
docker pull alpine
docket tag alpine:latest ip10-0-79-4-dbu6vq4mgm3g00ctdaqg-5000.direct.docker.labs.eazytraining.fr/alpine:remote
docker push ip10-0-79-4-dbu6vq4mgm3g00ctdaqg-5000.direct.docker.labs.eazytraining.fr/alpine:remote
```

> [!TIPS]
> Penser à rajouter un volume si l'on souhaite de la persistance
> 
> Quelques examples de docker-compose utiles : [Joxit WebUI + Docker Registry examples](https://github.com/Joxit/docker-registry-ui/tree/main/examples)