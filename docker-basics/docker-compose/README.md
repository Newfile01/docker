# Docker Compose

Exemple de construction d'une IaC à l'aide de Docker Compose :
+ Permet de centraliser toute une infrastructure multiservices
+ La rend facilement repoductible
+ Permet de la versionner

Ici nous déployons l'ERP Odoo permettant de gérer un certain nombre d'activités d'une entreprise (inventaire, vente, site web, etc...).
Il contient un serveur web Odoo et une base de donnée Postgres.

Structure nécessaire pour le répertoire de travail :

```texte
📂odoo-project/
│
├── compose.yaml
├── odoo_pg_pass    # Mdp écris directement dans le fichier, ce n'est ni une variable d'environnement ni du YAML
├── config/
│   └── odoo.conf   # Paramétrages du serveur Odoo (documentation officielle)
└── addons/
    └── (modules Odoo personnalisés)
```

Le `docker-compose.yml`utilisé ici permet de déployer deux services reliés : un serveur Odoo et une BDD Postgres
Les instructions sont disponibles sur : [Dockerhub Odoo](https://hub.docker.com/_/odoo)

## Notions importantes

**Découpage** : 
- Services : Les différents microservices gérés par le fichier docker-compose en tant qu'IaC
- Volumes : Les différents volume pour persister les données entre deux redémarrage d'un même conteneur
- Secrets : Un volume particulier qui permet ici d'aller chercher un fichier et de le monter en lecture seule dans le conteneur

> [!WARNING]
> Secrets ≠ chiffrement
> Le fichier partagé par les conteneurs pour gérer le mdp d'accès à la BDD n'est pas chiffré.
> Il faut bien faire attention à ne pas le versionner et dans un environnement de production à le protéger correctement.

Dans un fichier YAML l'indentation compte. Chaque service doit être indenté de manière identique, chaque niveau correspond à un bloc ou un sous-bloc.
La partie `environment`correspond à des variables d'environnements interprétables par le conteneur qui les utilises.
→ `web`peut utiliser `PASSWORD_FILE`tandis que `db`utilisera `POSTGRES_PASSWORD_FILE`


**Observations supplémentaires**

Trois types de volumes exploités ici :
- Volume : `odoo-web-data` et `odoo-db-data`
- Bind Mount : `./config:/etc/odoo` et `./addons:/mnt/extra-addons`
- Secrets : équivalent d'un bind mount mais monté en lecture seule (pas de chiffrement automatique) et obligation d'autoriser l'accès via `secrets`dans le service
