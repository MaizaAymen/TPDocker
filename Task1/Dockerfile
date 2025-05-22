FROM python:3.9-slim

# Métadonnées (labels)
LABEL author="votre-nom"
LABEL version="1.0"
LABEL description="Application d'inventaire"

# Définir le répertoire de travail
WORKDIR /app

# Installation des dépendances
COPY requirements.txt .
RUN pip install --no-cache-dir --upgrade pip && \
    pip install --no-cache-dir -r requirements.txt

# Copier les fichiers de l'application
COPY . .

# Créer un utilisateur non-root
RUN groupadd -r appuser && useradd -r -g appuser -u 1000 appuser && \
    chown -R appuser:appuser /app

# Créer un volume pour la persistance des données
VOLUME /data

# Exposer le port utilisé par Flask
EXPOSE 5000

# Variables d'environnement
ENV FLASK_ENV=production

# Rendre le script d'entrée exécutable
RUN chmod +x /app/entrypoint.sh

# Configuration du healthcheck
HEALTHCHECK --interval=30s --timeout=30s --start-period=5s --retries=3 \
    CMD wget --no-verbose --tries=1 --spider http://localhost:5000/ || exit 1

# Changement d'utilisateur pour des raisons de sécurité
USER appuser

# Configuration de l'entrée
ENTRYPOINT ["/app/entrypoint.sh"]

# Commande par défaut
CMD ["gunicorn", "--bind", "0.0.0.0:5000", "app:app"]