# Volumes

> Permettent de rendre persistant après exécution du conteneur

## Types de volumes

### Bind Mount
Monter un dossier local dans le conteneur lors de son exécution. Si le dossier de destination (dans le conteneur) est déjà existant, son contenu sera écrasé
Permet notamment de modifier les fichiers en local avant de les exploiter sur le conteneur.

### Volume
Créer un volume persistant dans Docker afin de persister les donner lors d'un prochain démarrage. Le volume pourra ainsi être partagé entre plusieurs conteneurs, répliqué, déporté, etc...
C'est utile lorsqu'on a des architectures multi-service qui repose sur un même point de données.

Correspond à un dossier caché dans docker.

### Tmpfs Mount
Permet d'avoir du stockage temporaire et de le partager entre plusieurs conteneurs

Usages : login / mdp partagés pour un frontend et un backend

---

## Exemple Bind Mount

Monter un site web directement dans un [serveur web Apache](https://hub.docker.com/_/httpd)

```bash
docker run -dit --name webserver -p 8080:80 -v ${PWD}/static-website-example:/usr/local/apache2/htdocs/ httpd:2.4
```
Connexion sur : http://localhost:8080
```bash
# Modifier le fichier index.html dans le dossier static-website-example/
docker restart webserver
# Actualiser la page web
```

→ Il est donc très facile de se représenter les modifications d'un site web sur le serveur Apache en cours d'exécution