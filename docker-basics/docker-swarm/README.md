# Docker Swarm

> Solution d'orchestration de conteneur de Docker permettant de déployer et automatiser l'orchestration des conteneurs sur un cluster de machines
> Principe : Déployer plusieurs machines hôtes, les ajouter au cluster et leur attribuer un rôle (Manager / Worker) puis déployer une stack de services conteneurisée
> Docker Swarm se chargera de déployer et redéployer les conteneurs selon les paramètres entrés dans `stack.yml` (networks, volumes, constraints [affinités], ...)

## TP - Déploiement application de vote avec Docker Swarm

Contexte :
→ Utilisation de la plateforme [dockerlabs](https://docker.labs.eazytraining.fr/)
→ Repository de l'application : [Example-voting-app](https://github.com/dockersamples/example-voting-app)
→ Déploiement d'un manager et un worker
→ Clonage repo & deploiement voting-app
→ Tests de fonctionnement

### Déploiement `voting-app`

1 - **Initialiser un master**
docker swarm init --advertise-addr 10.0.71.4

2 - **Enregistrer la commande de jonction avec token**
docker swarm join --token SWMTKN-1-6cqqcwlbecynmyydm3761mqju6vod1ere1xyxm6bkrwm0ykeut-22bs6mrpxnoxtfih3p31l6mbq 10.0.71.4:2377

3 - **Vérifier les noeuds du cluster**
docker node ls

3 - **Cloner repo**

4 - **Déployer la stack**
docker stack deploy --compose-file docker-stack.yml vote-stack
OU
docker stack deploy -c docker-stack.yml vote-stack

5 - Vérifier le déploiement
docker node ls
docker stack ls
docker stack ps vote-stack

### Résultats

![example-voting-app-scheme](https://github.com/dockersamples/example-voting-app/blob/main/architecture.excalidraw.png)

```bash
docker node ls

ID                            HOSTNAME   STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
zr7it6upy3i1vehcspea7an4j *   node1      Ready     Active         Leader           24.0.7
p83n5w7fb8hdy6k7sgznle78d     node2      Ready     Active                          24.0.7
[node1] (local) root@10.0.71.4 ~/example-voting-app
$ docker stack deploy --compose-file docker-stack.yml vote-stack
Creating network vote-stack_frontend
Creating network vote-stack_backend
Creating service vote-stack_redis
Creating service vote-stack_db
Creating service vote-stack_vote
Creating service vote-stack_result
Creating service vote-stack_worker
[node1] (local) root@10.0.71.4 ~/example-voting-app


$ docker node ls

ID                            HOSTNAME   STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
zr7it6upy3i1vehcspea7an4j *   node1      Ready     Active         Leader           24.0.7
p83n5w7fb8hdy6k7sgznle78d     node2      Ready     Active                          24.0.7
[node1] (local) root@10.0.71.4 ~/example-voting-app


$ docker stack ls

NAME         SERVICES
vote-stack   5
[node1] (local) root@10.0.71.4 ~/example-voting-app


$ docker stack ps vote-stack 

ID             NAME                  IMAGE                                          NODE      DESIRED STATE   CURRENT STATE                ERROR     PORTS
27ebveni8e0v   vote-stack_db.1       postgres:15-alpine                             node2     Running         Running about a minute ago             
nmowlnvthkqa   vote-stack_redis.1    redis:alpine                                   node1     Running         Running about a minute ago             
wjm088g7aeow   vote-stack_result.1   dockersamples/examplevotingapp_result:latest   node1     Running         Running about a minute ago             
sfvcxugsnfuy   vote-stack_vote.1     dockersamples/examplevotingapp_vote:latest     node2     Running         Running about a minute ago             
4if7w8mvxgr9   vote-stack_vote.2     dockersamples/examplevotingapp_vote:latest     node1     Running         Running about a minute ago             
jr1hrzdowaw8   vote-stack_worker.1   dockersamples/examplevotingapp_worker:latest   node2     Running         Running about a minute ago             
5asimrw8bzc8   vote-stack_worker.2   dockersamples/examplevotingapp_worker:latest   node1     Running         Running about a minute ago 


docker stack rm vote-stack
# Bien attendre que docker swarm supprime toute la stack (peut prendre un peu de temps et être encore visible avec un docker ps)
```