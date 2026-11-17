```bash
# Clustering des hôtes
docker swarm init --advertise-addr 10.0.71.4
docker swarm join --token SWMTKN-1-6cqqcwlbecynmyydm3761mqju6vod1ere1xyxm6bkrwm0ykeut-22bs6mrpxnoxtfih3p31l6mbq 10.0.71.4:2377
docker node ls

# Récupération & déploiement des ressources
git clone https://github.com/eazytraining/docker-training.git
cp -r docker-training/06_lab-6/example-voting-app/ .
rm -rf docker-training/
cd example-voting-app/
docker stack deploy --compose-file docker-stack.yml vote-stack

# Vérification du déploiement
docker node ls
docker stack ls
docker stack ps vote-stack
```