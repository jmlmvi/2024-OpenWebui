# OpenWebUI & Ollama avec Tunnel Cloudflare

Ce projet configure OpenWebUI et Ollama en utilisant Docker, avec un accès sécurisé via un Tunnel Cloudflare.

## Prérequis

- Docker et Docker Compose installés sur votre système.
- Un compte Cloudflare et un token de Tunnel Cloudflare.

## Structure du projet

```plaintext
.
├── docker-compose.yml
├── .env
├── open-webui-local/
└── ollama-local/
```

## Instructions d'installation

### 1. Cloner le dépôt

```sh
git clone https://github.com/votre-nom-utilisateur/votre-repo.git
cd votre-repo
```

### 2. Créer les répertoires nécessaires

Créez les répertoires pour les données de OpenWebUI et Ollama :

```sh
mkdir -p ./open-webui-local
mkdir -p ./ollama-local
```

### 3. Configurer les variables d'environnement

Créez un fichier `.env` à la racine du projet et ajoutez le contenu suivant :

```plaintext
# Ports
OPEN_WEBUI_PORT=4000
OLLAMA_PORT=11434

# Token du Tunnel Cloudflare
CF_TUNNEL_TOKEN=your_cloudflare_tunnel_token_here
```

Remplacez `your_cloudflare_tunnel_token_here` par votre véritable token de Tunnel Cloudflare.

### 4. Démarrer les services

Utilisez Docker Compose pour démarrer tous les services :

```sh
docker-compose up -d
```

### 5. Accéder aux services

- **OpenWebUI** : Accédez-y via `http://localhost:4000` ou via l'URL de votre Tunnel Cloudflare.
- **Ollama** : Accédez-y via `http://localhost:11434` ou via l'URL de votre Tunnel Cloudflare.

## Configuration de Docker Compose

Voici le fichier `docker-compose.yml` utilisé dans ce projet :

```yaml
version: '3.8'

services:
  openWebUI:
    image: ghcr.io/open-webui/open-webui:main
    restart: always
    ports:
      - "${OPEN_WEBUI_PORT}:8080"
    extra_hosts:
      - "host.docker.internal:host-gateway"
    volumes:
      - ./open-webui-local:/app/backend/data

  ollama:
    image: ollama/ollama:0.1.34
    ports:
      - "${OLLAMA_PORT}:${OLLAMA_PORT}"
    volumes:
      - ./ollama-local:/root/.ollama

  cf-tunnel:
    image: cloudflare/cloudflared:latest
    restart: unless-stopped
    command: tunnel --no-autoupdate run
    environment:
      - TUNNEL_TOKEN=${CF_TUNNEL_TOKEN}
```

## Informations supplémentaires

- Assurez-vous que Docker et Docker Compose sont correctement installés et fonctionnent sur votre machine.
- Si vous rencontrez des problèmes de permissions, assurez-vous que les répertoires ont les bons droits de propriété et de permission.

```sh
sudo chown -R $USER:$USER ./open-webui-local
sudo chown -R $USER:$USER ./ollama-local
```

## Dépannage

### Problèmes courants

- **Permission refusée** : Assurez-vous que les répertoires `./open-webui-local` et `./ollama-local` ont les bonnes permissions.
- **Connexion au Tunnel Cloudflare** : Assurez-vous que votre token de Tunnel Cloudflare est correct et que le tunnel est correctement configuré sur le tableau de bord Cloudflare.

Pour plus d'assistance, consultez la documentation officielle des services et de Docker.

## Licence

Ce projet est sous licence MIT. Voir le fichier [LICENSE](LICENSE) pour plus de détails.
```

### Instructions supplémentaires

1. **Copiez le contenu ci-dessus dans un fichier nommé `README.md` à la racine de votre projet.**
2. **Remplacez les éléments comme `https://github.com/votre-nom-utilisateur/votre-repo.git` et `your_cloudflare_tunnel_token_here` par les valeurs réelles.**
3. **Personnalisez toute section selon vos besoins spécifiques pour ce projet.**

Ce `README.md` fournit une guide clair et complet pour configurer et utiliser votre projet, facilitant ainsi la compréhension et le déploiement pour d'autres utilisateurs (et vous-même).
