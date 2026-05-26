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

## 2. Choisir ton provider IA

OpenClaw supporte plusieurs sources de modèles. Pour ce tuto, on couvre les deux options **sans coût d'API supplémentaire** si tu as déjà un abonnement :

| Provider | Prérequis | Auth |
|---|---|---|
| **GitHub Copilot** | Abonnement GitHub Copilot actif | Device flow OAuth (navigateur) |
| **Claude Code** | Abonnement Claude Pro/Max actif | Réutilise le login Claude CLI existant |

> [!NOTE]
> Le choix se fait **pendant l'onboarding interactif**. Pas besoin de décider maintenant,
> mais assure-toi d'avoir accès à l'un des deux avant de continuer.

### Pré-requis pour Claude Code uniquement

Si tu choisis **Claude Code**, installe et connecte le CLI **avant** de lancer le setup :

```bash
npm install -g @anthropic-ai/claude-code
claude login
```

Vérifie que c'est fonctionnel :

```bash
claude --version
```

Pour GitHub Copilot, rien à faire à l'avance — le device flow se lance pendant le setup.

---

## 3. Préparer le fichier `.env`

Le fichier `.env` contient les secrets de configuration. Il ne doit jamais être versionné.

```bash
nano ~/openclaw/.env
```

Contenu minimal :

```env
# Image pré-compilée officielle
OPENCLAW_IMAGE=ghcr.io/openclaw/openclaw:latest

# Désactiver Bonjour/mDNS (problématique en réseau Docker bridge)
OPENCLAW_DISABLE_BONJOUR=1

# Répertoires persistants (bind mounts vers le VPS)
OPENCLAW_CONFIG_DIR=/home/openclaw/openclaw/config
OPENCLAW_WORKSPACE_DIR=/home/openclaw/openclaw/workspace
OPENCLAW_AUTH_PROFILE_SECRET_DIR=/home/openclaw/openclaw/data

# Token Discord (à remplir à l'étape 4)
# DISCORD_BOT_TOKEN=MTxxxx...xxxx
```

> [!WARNING]
> Vérifie les permissions : `ls -la ~/openclaw/.env` → doit afficher `-rw-------`
> Si ce n'est pas le cas : `chmod 600 ~/openclaw/.env`

> [!IMPORTANT]
> `OPENCLAW_GATEWAY_TOKEN` sera **généré automatiquement** par le script de setup.
> Ne pas le définir à la main.

---

## 4. Lancer le setup (onboarding interactif)

> [!IMPORTANT]
> Le script est **interactif** : il requiert un terminal TTY.
> Lance-le directement dans ta session SSH — pas dans un script ou un cron.

```bash
cd ~/openclaw
./setup.sh
```

Le script va :
1. Puller l'image `ghcr.io/openclaw/openclaw:latest`
2. Lancer l'onboarding interactif — **choix du provider IA** (voir ci-dessous)
3. Générer un `OPENCLAW_GATEWAY_TOKEN` et l'écrire dans `.env`
4. Démarrer le gateway via `docker compose up -d`

### Option A — GitHub Copilot

Quand l'onboarding te demande ton provider, choisis **GitHub Copilot**.

Il lance le **device flow OAuth** :
- Il affiche une URL et un code à usage unique
- Ouvre l'URL dans ton navigateur, connecte-toi à GitHub, entre le code
- Garde le terminal SSH ouvert jusqu'à confirmation

```
→ Visit: https://github.com/login/device
→ Code:  XXXX-XXXX
→ Waiting for authorization...
✓ Logged in as ton-username
```

### Option B — Claude Code

Choisis **Claude CLI** dans l'onboarding — OpenClaw réutilise automatiquement
le token de la session `claude login` effectuée à l'étape précédente.

---

## 5. Vérifier que tout tourne

```bash
# État des conteneurs
docker compose ps

# Logs du gateway (Ctrl+C pour quitter)
docker compose logs -f openclaw-gateway
```

Résultat attendu dans les logs :

```
openclaw-gateway  | Gateway running on http://127.0.0.1:18789
```

Health checks depuis le VPS :

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

## 6. Permissions sur les dossiers montés

L'image Docker tourne avec l'utilisateur `node` (uid 1000). Si tu vois des erreurs de permission au démarrage :

```bash
sudo chown -R 1000:1000 ~/openclaw/config ~/openclaw/workspace ~/openclaw/data
```

---

## 7. Démarrage automatique au boot

Docker gère déjà le restart avec la policy `unless-stopped` définie dans le `docker-compose.yml` officiel.

Vérifie :

```bash
docker inspect openclaw-gateway --format '{{.HostConfig.RestartPolicy.Name}}'
# attendu : unless-stopped
```

Pour redémarrer manuellement :

```bash
cd ~/openclaw && docker compose restart
```

---

## Récapitulatif

À ce stade :

- [x] `docker-compose.yml` et `setup.sh` en place dans `~/openclaw/`
- [x] `.env` configuré avec permissions `600`
- [x] Provider IA configuré (GitHub Copilot ou Claude Code)
- [x] Onboarding complété, `OPENCLAW_GATEWAY_TOKEN` généré
- [x] Gateway up et accessible sur `http://127.0.0.1:18789` (loopback uniquement)
- [x] Health checks `/healthz` et `/readyz` répondent `OK`
- [x] Restart policy `unless-stopped` active

➡️ **Étape suivante : [Étape 3 — Créer le bot Discord](./etape-03-discord-bot.md)**
