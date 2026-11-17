# Commandes de base pour manipuler docker compose

```bash
# Démarrer les services
docker compose up -d

# Vérifier les conteneurs
docker compose ps

# Consulter les logs
docker compose logs -f

# Vérifier la présence du secret dans Odoo
docker compose exec web ls -l /run/secrets/

# Lire le secret (à éviter en environnement réel)
docker compose exec web cat /run/secrets/postgresql_password

# Arrêter les services
docker compose down

# Arrêter et supprimer également les volumes
docker compose down -v
```