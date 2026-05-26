# 📋 Mémo — Commandes agents OpenClaw

> Toutes les commandes s'exécutent sur le VPS via SSH.
> En Docker : `docker compose run --rm openclaw-cli <commande>`
> En bare-metal : `openclaw <commande>`

---

## Lister les agents

```bash
openclaw agents list
openclaw agents list --bindings   # inclut les règles de routage
openclaw agents list --json
```

---

## Créer un agent

```bash
# Interactif (wizard)
openclaw agents add assistant

# Non-interactif (scripted)
openclaw agents add assistant \
  --workspace ~/.openclaw/workspace-assistant \
  --non-interactive
```

---

## Créer un agent et lui assigner des channels Discord directement

```bash
openclaw agents add assistant \
  --workspace ~/.openclaw/workspace-assistant \
  --bind discord:* \
  --non-interactive
```

---

## Gérer les bindings (routage channel → agent)

```bash
# Voir tous les bindings
openclaw agents bindings
openclaw agents bindings --agent assistant

# Assigner un channel à un agent
openclaw agents bind --agent assistant --bind discord:*

# Retirer un binding
openclaw agents unbind --agent assistant --bind discord:*
openclaw agents unbind --agent assistant --all   # retirer tous les bindings
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
openclaw agents set-identity --agent assistant --from-identity

# Définir manuellement
openclaw agents set-identity --agent assistant \
  --name "JARVIS" \
  --emoji "🧠"
```

---

## Supprimer un agent

```bash
openclaw agents delete assistant           # demande confirmation
openclaw agents delete assistant --force   # sans confirmation
```

> [!NOTE]
> `main` ne peut pas être supprimé.
> Le workspace est déplacé dans la corbeille, pas effacé définitivement.

---

## Référence rapide

| Action | Commande |
|---|---|
| Lister les agents | `agents list` |
| Voir les bindings | `agents bindings` |
| Créer un agent | `agents add <id> --workspace <path>` |
| Assigner un channel | `agents bind --agent <id> --bind <channel>` |
| Retirer un channel | `agents unbind --agent <id> --bind <channel>` |
| Définir l'identité | `agents set-identity --agent <id> --from-identity` |
| Supprimer un agent | `agents delete <id>` |
