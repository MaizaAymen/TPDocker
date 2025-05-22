# Application d'inventaire Flask

## Construction de l'image Docker

```sh
docker build -t flask-inventory .
```

## Lancement avec MySQL 

### Avec une base de données MySQL existante

```sh
docker run --rm -it \
  -e DB_HOST=host.docker.internal \
  -e DB_USER=inventoryuser \
  -e DB_PASSWORD=secret \
  -e DB_NAME=inventory \
  -e INIT_DB=True \
  -p 5000:5000 \
  -v $(pwd)/Static:/app/Static \
  -v flask-inventory-data:/data \
  flask-inventory
```

### Avec MySQL dans un conteneur Docker

Il est recommandé d'utiliser Docker Compose pour gérer à la fois l'application et la base de données:

```sh
# Créer un réseau Docker pour la communication entre conteneurs
docker network create inventory-network

# Lancer le conteneur MySQL
docker run -d --name db \
  --network inventory-network \
  -e MYSQL_ROOT_PASSWORD=rootpassword \
  -e MYSQL_DATABASE=inventory \
  -e MYSQL_USER=inventoryuser \
  -e MYSQL_PASSWORD=secret \
  -v mysql-data:/var/lib/mysql \
  mysql:8.0

# Lancer l'application Flask
docker run -d --name flask-app \
  --network inventory-network \
  -e DB_HOST=db \
  -e DB_USER=inventoryuser \
  -e DB_PASSWORD=secret \
  -e DB_NAME=inventory \
  -e INIT_DB=True \
  -p 5000:5000 \
  -v flask-inventory-data:/data \
  flask-inventory
```

## Accès à l'application

Une fois lancée, l'application est accessible à l'adresse: http://localhost:5000
```

- Le healthcheck est accessible sur : http://localhost:5000/health
- Le volume `/data` permet de persister les données si besoin.
- Le script `entrypoint.sh` attend que MySQL soit prêt avant de lancer l'application.

## Variables d'environnement utiles
- `DB_HOST`, `DB_USER`, `DB_PASSWORD`, `DB_NAME` : connexion MySQL
- `INIT_DB=True` : initialise la base au démarrage (optionnel)

## Arrêt et suppression du conteneur

```sh
docker stop <container_id>
docker rm <container_id>
```
