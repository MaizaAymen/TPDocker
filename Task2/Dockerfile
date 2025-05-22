# Stage 1: Build de l'application React
FROM node:19-alpine AS build

# Définition du répertoire de travail
WORKDIR /app

# Copie des fichiers de dépendances
COPY package.json package-lock.json* ./

# Installation des dépendances
RUN npm ci --only=production

# Copie du reste des fichiers du projet
COPY . .

# Build de l'application React
RUN npm run build

# Stage 2: Image de production légère avec Nginx
FROM nginx:alpine AS production

# Copie de la configuration Nginx personnalisée
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Copie des fichiers de build depuis le stage précédent
COPY --from=build /app/build /usr/share/nginx/html

# Configuration des permissions
RUN chmod -R 755 /usr/share/nginx/html

# Suppression des fichiers inutiles
RUN rm -rf /var/cache/apk/*

# Exposer le port 80
EXPOSE 80

# Commande pour démarrer Nginx en premier plan
CMD ["nginx", "-g", "daemon off;"]
