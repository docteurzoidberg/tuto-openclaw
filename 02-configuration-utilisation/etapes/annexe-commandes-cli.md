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
docker compose run --rm openclaw-cli agents list
docker compose run --rm openclaw-cli agents list --bindings   # avec les règles de routage
docker compose run --rm openclaw-cli agents list --json

# Créer un agent (interactif)
docker compose run --rm -it openclaw-cli agents add <id>

# Créer un agent (non-interactif)
docker compose run --rm openclaw-cli agents add <id> \
  --workspace ~/.openclaw/workspace-<id> \
  --non-interactive

# Supprimer un agent
docker compose run --rm openclaw-cli agents delete <id>           # avec confirmation
docker compose run --rm openclaw-cli agents delete <id> --force   # sans confirmation
```

---

## Bindings (routage channel → agent)

```bash
# Voir tous les bindings
docker compose run --rm openclaw-cli agents bindings
docker compose run --rm openclaw-cli agents bindings --agent <id>

# Assigner un channel à un agent
docker compose run --rm openclaw-cli agents bind --agent <id> --bind discord:<channel-id>

# Retirer un binding
docker compose run --rm openclaw-cli agents unbind --agent <id> --bind discord:<channel-id>
docker compose run --rm openclaw-cli agents unbind --agent <id> --all   # retirer tous les bindings
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
docker compose run --rm openclaw-cli agents set-identity --agent <id> --from-identity

# Définir manuellement
docker compose run --rm openclaw-cli agents set-identity --agent <id> --name "JARVIS" --emoji "🧠"
```

---

## Cron (rappels et tâches planifiées)

```bash
# Lister les cron jobs actifs
docker compose run --rm openclaw-cli cron list

# Supprimer un cron job
docker compose run --rm openclaw-cli cron remove <job-id>
```

---

## Configuration

```bash
# Afficher toute la configuration
docker compose run --rm openclaw-cli config show

# Lire une valeur
docker compose run --rm openclaw-cli config get <clé>
docker compose run --rm openclaw-cli config get bindings

# Modifier une valeur
docker compose run --rm openclaw-cli config set <clé>=<valeur>
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
| Diagnostic complet | `docker compose run --rm openclaw-cli doctor` |
| Relancer le gateway | `docker compose restart openclaw-gateway` |
| Lister les agents | `docker compose run --rm openclaw-cli agents list --bindings` |
| Créer un agent | `docker compose run --rm -it openclaw-cli agents add <id>` |
| Assigner un channel | `docker compose run --rm openclaw-cli agents bind --agent <id> --bind discord:<id>` |
| Lister les rappels | `docker compose run --rm openclaw-cli cron list` |
| Voir la config | `docker compose run --rm openclaw-cli config show` |
| Mettre à jour | `docker compose pull && docker compose up -d` |

---

> Voir aussi : [Commandes Discord (slash commands)](./etape-00-commandes-discord.md)
