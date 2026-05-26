# Mémo — Commandes CLI OpenClaw

> Ces commandes s'exécutent sur le VPS via SSH.
> Préfixe Docker : `docker compose run --rm openclaw-cli <commande>`
> Préfixe bare-metal : `openclaw <commande>`
>
> Dans la plupart des cas, tu peux demander à l'agent principal de les exécuter
> à ta place — le CLI reste un outil de secours ou d'automatisation.

---

## Diagnostic

```bash
# État général du gateway
docker compose ps

# Logs du gateway (50 dernières lignes)
docker compose logs --tail=50 openclaw-gateway

# Diagnostic complet (connectivité, config, agents)
docker compose run --rm openclaw-cli doctor

# Statut des canaux Discord
docker compose run --rm openclaw-cli channels status --probe

# Audit sécurité
docker compose run --rm openclaw-cli security audit
```

---

## Gateway

```bash
# Relancer le gateway
docker compose restart openclaw-gateway

# Mettre à jour OpenClaw
docker compose pull
docker compose up -d
```

---

## Agents

```bash
# Lister les agents
openclaw agents list
openclaw agents list --bindings   # avec les règles de routage
openclaw agents list --json

# Créer un agent (interactif)
openclaw agents add <id>

# Créer un agent (non-interactif)
openclaw agents add <id> \
  --workspace ~/.openclaw/workspace-<id> \
  --non-interactive

# Supprimer un agent
openclaw agents delete <id>           # avec confirmation
openclaw agents delete <id> --force   # sans confirmation
```

---

## Bindings (routage channel → agent)

```bash
# Voir tous les bindings
openclaw agents bindings
openclaw agents bindings --agent <id>

# Assigner un channel à un agent
openclaw agents bind --agent <id> --bind discord:<channel-id>

# Retirer un binding
openclaw agents unbind --agent <id> --bind discord:<channel-id>
openclaw agents unbind --agent <id> --all   # retirer tous les bindings
```

### Formats de binding

| Format | Signification |
|---|---|
| `discord:<channel-id>` | Channel Discord spécifique |
| `discord:*` | Tous les channels du compte Discord par défaut |
| `discord:moncompte` | Compte Discord spécifique |

---

## Identité d'un agent

```bash
# Lire depuis IDENTITY.md dans le workspace
openclaw agents set-identity --agent <id> --from-identity

# Définir manuellement
openclaw agents set-identity --agent <id> --name "JARVIS" --emoji "🧠"
```

---

## Cron (rappels et tâches planifiées)

```bash
# Lister les cron jobs actifs
openclaw cron list

# Supprimer un cron job
openclaw cron remove <job-id>
```

---

## Configuration

```bash
# Afficher toute la configuration
openclaw config show

# Lire une valeur
openclaw config get <clé>
openclaw config get bindings

# Modifier une valeur
openclaw config set <clé>=<valeur>
```

---

## Sauvegarde

```bash
tar czf ~/backup-openclaw-$(date +%Y%m%d).tar.gz \
  ~/openclaw/config \
  ~/openclaw/data \
  ~/openclaw/workspace \
  ~/openclaw/workspace-assistant \
  ~/openclaw/.env
```

---

## Référence rapide

| Action | Commande |
|---|---|
| Diagnostic complet | `openclaw doctor` |
| Relancer le gateway | `docker compose restart openclaw-gateway` |
| Lister les agents | `openclaw agents list --bindings` |
| Créer un agent | `openclaw agents add <id>` |
| Assigner un channel | `openclaw agents bind --agent <id> --bind discord:<id>` |
| Lister les rappels | `openclaw cron list` |
| Voir la config | `openclaw config show` |
| Mettre à jour | `docker compose pull && docker compose up -d` |

---

> Voir aussi : [Commandes Discord (slash commands)](./etape-00-commandes-discord.md)
