# Étape 2 — Installer OpenClaw avec Docker Compose

> [!NOTE]
> On utilise l'image pré-compilée officielle d'OpenClaw depuis le GitHub Container Registry.
> Pas besoin de builder localement — c'est plus rapide et adapté à un VPS.

---

## 1. Récupérer les fichiers Docker officiels

OpenClaw fournit un `docker-compose.yml` et un script de setup dans son dépôt GitHub.
On les récupère sans cloner tout le repo :

```bash
cd ~/openclaw

# Récupérer le docker-compose.yml officiel
curl -fsSL https://raw.githubusercontent.com/openclaw/openclaw/main/docker-compose.yml \
  -o docker-compose.yml

# Récupérer le script de setup
curl -fsSL https://raw.githubusercontent.com/openclaw/openclaw/main/scripts/docker/setup.sh \
  -o setup.sh
chmod +x setup.sh
```

---

## 2. Préparer le fichier `.env`

Le fichier `.env` contient tous les secrets. Il ne doit jamais être versionné.

```bash
nano ~/openclaw/.env
```

Contenu minimal à compléter :

```env
# Image à utiliser (image pré-compilée officielle)
OPENCLAW_IMAGE=ghcr.io/openclaw/openclaw:latest

# Désactiver Bonjour/mDNS (inutile et problématique en Docker bridge)
OPENCLAW_DISABLE_BONJOUR=1

# Répertoires persistants (bind mounts)
OPENCLAW_CONFIG_DIR=/home/openclaw/openclaw/config
OPENCLAW_WORKSPACE_DIR=/home/openclaw/openclaw/workspace
OPENCLAW_AUTH_PROFILE_SECRET_DIR=/home/openclaw/openclaw/data

# Clé API du modèle IA (ex: Anthropic)
ANTHROPIC_API_KEY=sk-ant-xxxxxxxxxxxxxxxxxxxx
# ou OpenAI :
# OPENAI_API_KEY=sk-xxxxxxxxxxxxxxxxxxxx

# Token Discord (à remplir à l'étape 4)
# DISCORD_BOT_TOKEN=MTxxxxxxxxxxxxxxxxx
```

> [!WARNING]
> Le fichier `.env` doit rester en permissions `600` (créé ainsi à l'étape 1).
> Vérifie : `ls -la ~/openclaw/.env` → doit afficher `-rw-------`

> [!IMPORTANT]
> `OPENCLAW_GATEWAY_TOKEN` sera **généré automatiquement** par le script de setup.
> Ne pas le définir à la main pour l'instant.

---

## 3. Lancer le setup

Le script d'onboarding installe OpenClaw, génère le token gateway, et démarre le conteneur.

```bash
cd ~/openclaw
OPENCLAW_IMAGE=ghcr.io/openclaw/openclaw:latest ./setup.sh
```

Le script va :
- Puller l'image depuis `ghcr.io/openclaw/openclaw:latest`
- Lancer l'onboarding interactif (choix du provider IA, saisie de la clé API)
- Générer un `OPENCLAW_GATEWAY_TOKEN` et l'écrire dans `.env`
- Démarrer le gateway via `docker compose up -d`

> [!NOTE]
> L'onboarding est interactif : il va poser des questions sur ton provider IA.
> Si tu as déjà mis la clé API dans `.env`, tu peux en général appuyer sur Entrée pour confirmer.

---

## 4. Vérifier que tout tourne

```bash
# Vérifier que le conteneur est up
docker compose ps

# Vérifier les logs du gateway
docker compose logs -f openclaw-gateway
```

Résultat attendu dans les logs :

```
openclaw-gateway  | Gateway running on http://127.0.0.1:18789
```

Vérification santé (depuis le VPS) :

```bash
curl -fsS http://127.0.0.1:18789/healthz
curl -fsS http://127.0.0.1:18789/readyz
```

Les deux doivent répondre `OK`.

> [!NOTE]
> Le port 18789 est accessible depuis le VPS lui-même (`127.0.0.1`),
> mais **bloqué par UFW** pour le reste d'Internet. C'est voulu.
> L'accès depuis ton PC se fera via SSH tunnel (étape 5).

---

## 5. Permissions sur les dossiers montés

L'image Docker tourne avec l'utilisateur `node` (uid 1000). Si tu vois des erreurs de permission :

```bash
sudo chown -R 1000:1000 ~/openclaw/config ~/openclaw/workspace ~/openclaw/data
```

---

## 6. Activer le démarrage automatique

Pour que OpenClaw redémarre automatiquement au boot du VPS :

```bash
cd ~/openclaw
docker compose up -d
```

Docker gère déjà le restart avec la policy `unless-stopped` définie dans le `docker-compose.yml` officiel.

Vérifie que la policy est bien en place :

```bash
docker inspect openclaw-gateway --format '{{.HostConfig.RestartPolicy.Name}}'
# attendu : unless-stopped
```

---

## Récapitulatif

À ce stade :

- [x] Image officielle `ghcr.io/openclaw/openclaw:latest` pullée
- [x] `docker-compose.yml` en place dans `~/openclaw/`
- [x] `.env` configuré avec la clé API IA + permissions `600`
- [x] Onboarding complété, `OPENCLAW_GATEWAY_TOKEN` généré
- [x] Gateway up et accessible sur `http://127.0.0.1:18789` (loopback uniquement)
- [x] Health checks `/healthz` et `/readyz` répondent OK
- [x] Restart policy `unless-stopped` active

➡️ **Étape suivante : [Étape 3 — Créer le bot Discord](./etape-03-discord-bot.md)**
