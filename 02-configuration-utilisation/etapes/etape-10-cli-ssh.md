# Étape 10 — Debug et administration via CLI SSH

> [!NOTE]
> Le client OpenClaw CLI permet de tout administrer directement depuis le VPS en SSH.
> C'est ton outil de debug et de maintenance quand Discord ne répond plus ou qu'il faut
> inspecter l'état interne du gateway.

---

## Connexion

```bash
ssh openclaw@<IP_DU_VPS>
cd ~/openclaw
```

Toutes les commandes CLI s'exécutent via le conteneur `openclaw-cli` :

```bash
docker compose run --rm openclaw-cli <commande>
```

> [!NOTE]
> Si tu as configuré l'alias à l'étape 3, utilise `oc run --rm openclaw-cli <commande>`.
> Pour raccourcir encore, tu peux ajouter dans `~/.bashrc` :
> ```bash
> alias openclaw="docker compose -f ~/openclaw/docker-compose.yml -f ~/openclaw/docker-compose.extra.yml run --rm openclaw-cli"
> ```
> Puis simplement : `openclaw status`, `openclaw agents list`, etc.

---

## Commandes essentielles

### État général

```bash
# État du gateway
docker compose ps
docker compose logs --tail=50 openclaw-gateway

# Diagnostic complet
docker compose run --rm openclaw-cli doctor

# Statut des canaux (Discord connecté ?)
docker compose run --rm openclaw-cli channels status --probe

# Statut des modèles IA
docker compose run --rm openclaw-cli models status
```

### Agents

```bash
# Lister les agents configurés
docker compose run --rm openclaw-cli agents list

# Voir la config d'un agent spécifique
docker compose run --rm openclaw-cli config get agents.list
```

### Sessions

```bash
# Lister les sessions actives
docker compose run --rm openclaw-cli sessions list

# Voir l'historique d'une session
docker compose run --rm openclaw-cli sessions history --session <session-id>

# Réinitialiser une session (efface le contexte)
docker compose run --rm openclaw-cli sessions reset --session <session-id>
```

### Configuration

```bash
# Voir la config complète
docker compose run --rm openclaw-cli config show

# Voir une clé spécifique
docker compose run --rm openclaw-cli config get channels.discord
docker compose run --rm openclaw-cli config get bindings
docker compose run --rm openclaw-cli config get agents.list

# Modifier une valeur
docker compose run --rm openclaw-cli config set <clé> <valeur>
```

### Cron jobs (rappels)

```bash
# Voir tous les cron jobs en cours
docker compose run --rm openclaw-cli cron list

# Voir les détails d'un job
docker compose run --rm openclaw-cli cron get <job-id>

# Supprimer un job
docker compose run --rm openclaw-cli cron remove <job-id>

# Voir l'historique d'exécution d'un job
docker compose run --rm openclaw-cli cron runs <job-id>
```

### Sécurité

```bash
# Audit sécurité
docker compose run --rm openclaw-cli security audit
docker compose run --rm openclaw-cli security audit --deep

# Corriger les problèmes courants automatiquement
docker compose run --rm openclaw-cli security audit --fix
```

---

## Scénarios de debug courants

### Le bot Discord ne répond plus

```bash
# 1. Vérifier que le conteneur tourne
docker compose ps

# 2. Regarder les erreurs dans les logs
docker compose logs --tail=100 openclaw-gateway | grep -i error

# 3. Vérifier la connexion Discord
docker compose run --rm openclaw-cli channels status --probe

# 4. Redémarrer si nécessaire
docker compose restart openclaw-gateway
```

### L'agent répond avec la mauvaise personnalité

```bash
# Vérifier quel agent traite le channel
docker compose run --rm openclaw-cli config get bindings

# Vérifier le workspace de l'agent
docker compose run --rm openclaw-cli config get agents.list

# Regarder le fichier SOUL.md de l'agent concerné
cat ~/openclaw/workspace-assistant/SOUL.md
```

### Un rappel ne se déclenche pas

```bash
# Lister les cron jobs
docker compose run --rm openclaw-cli cron list

# Vérifier l'heure système du VPS (doit correspondre à ton timezone)
date
timedatectl

# Voir l'historique d'exécution
docker compose run --rm openclaw-cli cron runs <job-id>
```

### Voir ce que "voit" l'agent en temps réel

```bash
# Logs en direct avec filtrage
docker compose logs -f openclaw-gateway | grep -i "assistant\|discord\|agent"
```

---

## Mise à jour d'OpenClaw

```bash
cd ~/openclaw

# Puller la nouvelle image
docker compose pull

# Redémarrer avec la nouvelle image
docker compose up -d

# Vérifier la version
docker compose run --rm openclaw-cli --version
```

---

## Sauvegarde

Avant toute modification importante :

```bash
# Sauvegarder config + workspaces + données
tar czf ~/backup-openclaw-$(date +%Y%m%d-%H%M).tar.gz \
  ~/openclaw/config \
  ~/openclaw/data \
  ~/openclaw/workspace \
  ~/openclaw/workspace-assistant \
  ~/openclaw/.env

# Vérifier la sauvegarde
ls -lh ~/backup-openclaw-*.tar.gz
```

---

## Récapitulatif des commandes à retenir

| Besoin | Commande |
|---|---|
| Bot muet | `docker compose logs --tail=100 openclaw-gateway` |
| Canal déconnecté | `openclaw-cli channels status --probe` |
| Voir les rappels | `openclaw-cli cron list` |
| Voir les agents | `openclaw-cli agents list` |
| Voir la config | `openclaw-cli config show` |
| Audit sécurité | `openclaw-cli security audit` |
| Redémarrer | `docker compose restart openclaw-gateway` |
| Mettre à jour | `docker compose pull && docker compose up -d` |

---

**Le tuto 2 est terminé.** Ton agent assistant personnel est opérationnel, personnalisé,
et tu sais comment l'administrer depuis le VPS.

---

## Pour aller plus loin

- Ajouter d'autres skills (résumé journalier, gestion d'agenda, etc.)
- Configurer le heartbeat pour que l'agent soit proactif sans que tu lui parles
- Ajouter un reverse proxy (Traefik) pour accéder à l'UI OpenClaw sans SSH tunnel
- Mettre en place Fail2Ban et un hardening avancé du VPS
