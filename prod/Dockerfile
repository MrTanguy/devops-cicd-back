# Étape 1 : image de base légère avec Python
FROM python:3.12-slim

# Étape 2 : Installation des dépendances système utiles
RUN apt-get update && apt-get install -y --no-install-recommends \
    openssl \
    ca-certificates \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# Étape 3 : Création d’un certificat SSL auto-signé
RUN mkdir -p /cert && \
    openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
    -keyout /cert/cert.key -out /cert/cert.crt \
    -subj "/CN=localhost"

# Étape 4 : Définir le dossier de travail
WORKDIR /app

# Étape 5 : Copier et installer les dépendances Python
COPY requirements.txt ./
RUN pip install --no-cache-dir --upgrade pip && \
    pip install --no-cache-dir -r requirements.txt

# Étape 6 : Copier le reste du code
COPY . .

# Étape 7 : Exposer le port pour l’extérieur
EXPOSE 8000

# Étape 8 : Démarrer l’app FastAPI en production (sans --reload)
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
