# Dockerfile Multi-stage

## ❓ À quoi ça sert

Le multi-stage permet de réduire l'image finale en ne conservant que ce qui est utile à l'exécution du conteneur :

- Build final d'une application Python, Java, etc...
- Site web copié depuis un repo Git cloné (comme ici)
...

1) À l'aide de `FROM` et `AS` on créé des étapes de build pour Docker
2) Build l'image :
    A - Possibilité de créé uniquement l'étape qui nous intéresse avec `docker build --target <build stage> --tag <image_name:tag> <context Dockerfile>`
    B - Créer toute l'image avec `docker build -t <image_name:tag> <context Dockerfile>`

On remarque souvent ce comportement pour alléger au maximum l'image finale, tester les binaires créer avant l'exposition, injecter des éléments temporaires, etc...

---

## 🌟 Bonus

Ici nous avons rajouté dans l'étape `stage` la réduction de l'exécution de `nginx` par un utilisateur non-root.
Ceci permet d'empêcher un utilisateur mal intentionné ayant accès au docker engine de rentrer par défaut en tant que `root` dans le conteneur. Il ne pourra alors **pas supprimer, modifier ou ajouter** des fichiers dans notre serveur web. 

Par ailleurs, les logs d'erreur et de requêtes http sont redirigés vers les sorties standards (`stderr` & `stout`). Ceci dans l'unique but de ne pas accumuler des fichiers dans le conteneur. Si l'on souhaite les obeserver il faudra alors monitorer les sorties standard directement à travers docker engine (et donc avoir les droits).
