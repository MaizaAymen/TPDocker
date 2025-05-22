# Application React avec Multi-stage Build

Ce projet utilise la technique du multi-stage build pour créer une image Docker légère et optimisée d'une application React servie par Nginx.

## Construction de l'image Docker

```sh
docker build -t react-app .
```

## Lancement du conteneur

```sh
docker run -d -p 80:80 --name react-frontend react-app
```

## Accès à l'application

Une fois le conteneur lancé, l'application est accessible à l'adresse: http://localhost

## Avantages du multi-stage build

Cette configuration offre plusieurs avantages:

1. **Image finale légère**: L'image de production ne contient que les fichiers statiques compilés et Nginx, sans les outils de développement
2. **Sécurité améliorée**: Aucun code source ni outil de build n'est présent dans l'image finale
3. **Performance optimisée**: Nginx est configuré pour servir efficacement les applications React single-page 
4. **Mise en cache**: La configuration inclut des en-têtes pour la mise en cache correcte des ressources statiques

## Instructions pour le développement

Pour développer l'application en local (sans Docker):

```sh
npm install
npm start
```

Pour construire l'application en local:

```sh
npm run build
```
