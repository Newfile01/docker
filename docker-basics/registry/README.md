# Gérer son registre d'image en dehors de DockerHub

> À des fins de configentialité il est possible de vouloir maîtriser son dépôt d'images personnalisées
> Ici nous déployons un registre privé sur une machine (considérée comme locale)
>
> Nous allons :
> 1) Pousser une image depuis la machine où se trouve le registre
> 2) Pousser une image depuis une machine distante


# Déploiement du registre

→ Frontend (Web UI) : [Joxit - Docker Registry UI](https://hub.docker.com/r/joxit/docker-registry-ui)
→ Backend (registre de dépôt d'image) :  [Docker Registry Officiel](https://hub.docker.com/_/registry)

```bash
docker network create regnet
docker run -d -p 5000:5000 --restart always --network regnet --name registry registry:3
docker run -d -p 8090:80 --restart always -e NGINX_PROXY_PASS_URL=http://registry:5000 -e REGISTRY_STORAGE_DELETE_ENABLED=true --network regnet --name reg_webui joxit/docker-registry-ui:latest
# Récupérer l'adresse du registre (via son URL du port 5000) : ip10-0-79-4-dbu6vq4mgm3g00ctdaqg-5000.direct.docker.labs.eazytraining.fr/
```

## Push LOCAL

`Depuis VM dockerlabs hébergeant le registry`
```bash
docker pull nginx
docket tag nginx:latest localhost:5000/nginx:local
docker push localhost:5000/nginx:local
```

## Push DISTANT

> Ce test à été effectué sur l'environnement de lab [dockerlabs](https://docker.labs.eazytraining.fr) fourni par Eazytraining
> + Permet de générer rapidement deux VM distinctes avec deux IP différentes
> 
> Possible de tester directement depuis sa propre machine (équipée de docker) et de push vers la VM éphémère créée sur dockerlabs

`Depuis seconde VM dockerlabs vers première VM dockerlabs`
```bash
# ↓↓ Sur la seconde machine (distante) ↓↓
docker pull alpine
docket tag alpine:latest ip10-0-79-4-dbu6vq4mgm3g00ctdaqg-5000.direct.docker.labs.eazytraining.fr/alpine:remote
docker push ip10-0-79-4-dbu6vq4mgm3g00ctdaqg-5000.direct.docker.labs.eazytraining.fr/alpine:remote
```
`Depuis mon propre PC vers VM dockerlabs`
```bash
docker images
docker tag net-tools-light:v1 ip10-0-79-4-dbu6vq4mgm3g00ctdaqg-5000.direct.docker.labs.eazytraining.fr/net-tools-light:remote
docker push ip10-0-79-4-dbu6vq4mgm3g00ctdaqg-5000.direct.docker.labs.eazytraining.fr/net-tools-light:remote
```