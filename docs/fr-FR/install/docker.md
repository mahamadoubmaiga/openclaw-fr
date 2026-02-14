---
summary: "Configuration optionnelle basée sur Docker et intégration pour OpenClaw"
read_when:
  - Vous voulez une passerelle conteneurisée au lieu d'installations locales
  - Vous validez le flux Docker
title: "Docker"
---

# Docker (optionnel)

Docker est **optionnel**. Utilisez-le uniquement si vous voulez une passerelle conteneurisée ou pour valider le flux Docker.

## Docker est-il fait pour moi ?

- **Oui** : vous voulez un environnement de passerelle isolé et jetable ou exécuter OpenClaw sur un hôte sans installations locales.
- **Non** : vous l'exécutez sur votre propre machine et voulez juste la boucle de développement la plus rapide. Utilisez le flux d'installation normal à la place.
- **Note sur le bac à sable** : l'isolation en bac à sable de l'agent utilise également Docker, mais elle ne **nécessite pas** que la passerelle complète s'exécute dans Docker. Voir [Bac à sable](/gateway/sandboxing).

Ce guide couvre :

- Passerelle conteneurisée (OpenClaw complet dans Docker)
- Bac à sable d'agent par session (passerelle hôte + outils d'agent isolés Docker)

Détails sur le bac à sable : [Bac à sable](/gateway/sandboxing)

## Prérequis

- Docker Desktop (ou Docker Engine) + Docker Compose v2
- Suffisamment d'espace disque pour les images + logs

## Passerelle conteneurisée (Docker Compose)

### Démarrage rapide (recommandé)

Depuis la racine du dépôt :

```bash
./docker-setup.sh
```

Ce script :

- construit l'image de la passerelle
- exécute l'assistant d'intégration
- affiche des conseils optionnels de configuration du fournisseur
- démarre la passerelle via Docker Compose
- génère un token de passerelle et l'écrit dans `.env`

Variables d'environnement optionnelles :

- `OPENCLAW_DOCKER_APT_PACKAGES` — installer des packages apt supplémentaires pendant la construction
- `OPENCLAW_EXTRA_MOUNTS` — ajouter des montages bind hôte supplémentaires
- `OPENCLAW_HOME_VOLUME` — persister `/home/node` dans un volume nommé

Après la fin :

- Ouvrez `http://127.0.0.1:18789/` dans votre navigateur.
- Collez le token dans l'UI de Contrôle (Paramètres → token).
- Besoin de l'URL à nouveau ? Exécutez `docker compose run --rm openclaw-cli dashboard --no-open`.

Il écrit la config/l'espace de travail sur l'hôte :

- `~/.openclaw/`
- `~/.openclaw/workspace`

Vous exécutez sur un VPS ? Voir [Hetzner (Docker VPS)](/install/hetzner).

### Assistants Shell (optionnel)

Pour une gestion Docker quotidienne plus facile, installez `ClawDock` :

```bash
mkdir -p ~/.clawdock && curl -sL https://raw.githubusercontent.com/openclaw/openclaw/main/scripts/shell-helpers/clawdock-helpers.sh -o ~/.clawdock/clawdock-helpers.sh
```

**Ajoutez à votre config shell (zsh) :**

```bash
echo 'source ~/.clawdock/clawdock-helpers.sh' >> ~/.zshrc && source ~/.zshrc
```

Ensuite, utilisez `clawdock-start`, `clawdock-stop`, `clawdock-dashboard`, etc. Exécutez `clawdock-help` pour toutes les commandes.

Voir [README de l'assistant `ClawDock`](https://github.com/openclaw/openclaw/blob/main/scripts/shell-helpers/README.md) pour les détails.

### Flux manuel (compose)

```bash
docker build -t openclaw:local -f Dockerfile .
docker compose run --rm openclaw-cli onboard
docker compose up -d openclaw-gateway
```

Note : exécutez `docker compose ...` depuis la racine du dépôt. Si vous avez activé
`OPENCLAW_EXTRA_MOUNTS` ou `OPENCLAW_HOME_VOLUME`, le script de configuration écrit
`docker-compose.extra.yml` ; incluez-le lors de l'exécution de Compose ailleurs :

```bash
docker compose -f docker-compose.yml -f docker-compose.extra.yml <commande>
```

### Token UI de Contrôle + appairage (Docker)

Si vous voyez "unauthorized" ou "disconnected (1008): pairing required", récupérez un
nouveau lien de tableau de bord et approuvez le périphérique du navigateur :

```bash
docker compose run --rm openclaw-cli dashboard --no-open
docker compose run --rm openclaw-cli devices list
docker compose run --rm openclaw-cli devices approve <requestId>
```

Plus de détails : [Tableau de bord](/web/dashboard), [Périphériques](/cli/devices).

### Montages supplémentaires (optionnel)

Si vous voulez monter des répertoires hôte supplémentaires dans les conteneurs, définissez
`OPENCLAW_EXTRA_MOUNTS` avant d'exécuter `docker-setup.sh`. Cela accepte une
liste séparée par des virgules de montages bind Docker et les applique à la fois à
`openclaw-gateway` et `openclaw-cli` en générant `docker-compose.extra.yml`.

Exemple :

```bash
export OPENCLAW_EXTRA_MOUNTS="$HOME/.codex:/home/node/.codex:ro,$HOME/github:/home/node/github:rw"
./docker-setup.sh
```

Notes :

- Les chemins doivent être partagés avec Docker Desktop sur macOS/Windows.
- Si vous modifiez `OPENCLAW_EXTRA_MOUNTS`, réexécutez `docker-setup.sh` pour régénérer le
  fichier compose supplémentaire.
- `docker-compose.extra.yml` est généré. Ne l'éditez pas manuellement.

### Persister tout le home du conteneur (optionnel)

Si vous voulez que `/home/node` persiste lors de la recréation du conteneur, définissez un
volume nommé via `OPENCLAW_HOME_VOLUME`. Cela crée un volume Docker et le monte à
`/home/node`, tout en gardant les montages bind standard config/workspace. Utilisez un
volume nommé ici (pas un chemin bind) ; pour les montages bind, utilisez
`OPENCLAW_EXTRA_MOUNTS`.

Exemple :

```bash
export OPENCLAW_HOME_VOLUME="openclaw_home"
./docker-setup.sh
```

Vous pouvez combiner cela avec des montages supplémentaires :

```bash
export OPENCLAW_HOME_VOLUME="openclaw_home"
export OPENCLAW_EXTRA_MOUNTS="$HOME/.codex:/home/node/.codex:ro,$HOME/github:/home/node/github:rw"
./docker-setup.sh
```

Notes :

- Si vous changez `OPENCLAW_HOME_VOLUME`, réexécutez `docker-setup.sh` pour régénérer le
  fichier compose supplémentaire.
- Le volume nommé persiste jusqu'à sa suppression avec `docker volume rm <nom>`.

### Installer des packages apt supplémentaires (optionnel)

Si vous avez besoin de packages système dans l'image (par exemple, des outils de build ou des
bibliothèques média), définissez `OPENCLAW_DOCKER_APT_PACKAGES` avant d'exécuter `docker-setup.sh`.
Cela installe les packages pendant la construction de l'image, donc ils persistent même si le
conteneur est supprimé.

Exemple :

```bash
export OPENCLAW_DOCKER_APT_PACKAGES="ffmpeg build-essential"
./docker-setup.sh
```

Notes :

- Cela accepte une liste séparée par des espaces de noms de packages apt.
- Si vous changez `OPENCLAW_DOCKER_APT_PACKAGES`, réexécutez `docker-setup.sh` pour reconstruire
  l'image.

## Dépannage

Voir le fichier original [install/docker.md](/install/docker) pour plus de détails sur :
- Conteneur complet avec fonctionnalités avancées (opt-in)
- Bac à sable d'agent par session
- Configuration de proxy personnalisée
- Commandes de mise à jour et de maintenance
