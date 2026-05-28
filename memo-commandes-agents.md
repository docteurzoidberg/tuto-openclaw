# 📋 Mémo — Commandes agents OpenClaw

> Toutes les commandes s'exécutent sur le VPS via SSH.
> En Docker : `docker compose run --rm openclaw-cli <commande>`
> En bare-metal : `openclaw <commande>`

---

## Lister les agents

```bash
docker compose run --rm openclaw-cli agents list
docker compose run --rm openclaw-cli agents list --bindings   # inclut les règles de routage
docker compose run --rm openclaw-cli agents list --json
```

---

## Créer un agent

```bash
# Interactif (wizard)
docker compose run --rm -it openclaw-cli agents add assistant

# Non-interactif (scripted)
docker compose run --rm openclaw-cli agents add assistant \
  --workspace ~/.openclaw/workspace-assistant \
  --non-interactive
```

---

## Créer un agent et lui assigner des channels Discord directement

```bash
docker compose run --rm openclaw-cli agents add assistant \
  --workspace ~/.openclaw/workspace-assistant \
  --bind discord:* \
  --non-interactive
```

---

## Gérer les bindings (routage channel → agent)

```bash
# Voir tous les bindings
docker compose run --rm openclaw-cli agents bindings
docker compose run --rm openclaw-cli agents bindings --agent assistant

# Assigner un channel à un agent
docker compose run --rm openclaw-cli agents bind --agent assistant --bind discord:*

# Retirer un binding
docker compose run --rm openclaw-cli agents unbind --agent assistant --bind discord:*
docker compose run --rm openclaw-cli agents unbind --agent assistant --all   # retirer tous les bindings
```

### Formats de binding

| Format | Signification |
|---|---|
| `discord:*` | Tous les comptes Discord |
| `discord:moncompte` | Un compte spécifique |
| `discord` | Compte Discord par défaut |
| `telegram:*` | Tous les comptes Telegram |

---

## Définir l'identité d'un agent

```bash
# Lire depuis IDENTITY.md dans le workspace
docker compose run --rm openclaw-cli agents set-identity --agent assistant --from-identity

# Définir manuellement
docker compose run --rm openclaw-cli agents set-identity --agent assistant \
  --name "JARVIS" \
  --emoji "🧠"
```

---

## Supprimer un agent

```bash
docker compose run --rm openclaw-cli agents delete assistant           # demande confirmation
docker compose run --rm openclaw-cli agents delete assistant --force   # sans confirmation
```

> [!NOTE]
> `main` ne peut pas être supprimé.
> Le workspace est déplacé dans la corbeille, pas effacé définitivement.

---

## Référence rapide

| Action | Commande |
|---|---|
| Lister les agents | `docker compose run --rm openclaw-cli agents list` |
| Voir les bindings | `docker compose run --rm openclaw-cli agents bindings` |
| Créer un agent | `docker compose run --rm -it openclaw-cli agents add <id>` |
| Assigner un channel | `docker compose run --rm openclaw-cli agents bind --agent <id> --bind <channel>` |
| Retirer un channel | `docker compose run --rm openclaw-cli agents unbind --agent <id> --bind <channel>` |
| Définir l'identité | `docker compose run --rm openclaw-cli agents set-identity --agent <id> --from-identity` |
| Supprimer un agent | `docker compose run --rm openclaw-cli agents delete <id>` |
