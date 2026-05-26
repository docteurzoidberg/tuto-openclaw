# Étape 7 — Maintenance et debug

> [!NOTE]
> Dans l'usage quotidien, tu n'as pas besoin du CLI.
> Cette étape couvre les cas où quelque chose ne fonctionne plus —
> et comment diagnostiquer via l'agent principal ou, en dernier recours, le CLI.

---

## En cas de problème — demander à l'agent principal d'abord

Avant de toucher quoi que ce soit sur le VPS, essaie toujours de passer
par l'agent principal en DM Discord. Il a accès aux outils de diagnostic
et peut résoudre la plupart des problèmes lui-même.

### Bot muet dans un channel

```
L'agent assistant ne répond plus dans #notes. Peux-tu diagnostiquer ?
```

### Rappel qui ne s'est pas déclenché

```
Mon rappel de ce matin ne s'est pas déclenché. Peux-tu vérifier les cron jobs actifs ?
```

### Vérifier l'état général

```
Peux-tu faire un audit de santé du gateway et me dire si tout est OK ?
```

### Mettre à jour OpenClaw

```
Peux-tu mettre à jour OpenClaw vers la dernière version ?
```

---

## Si l'agent principal ne répond pas non plus

Le gateway est probablement arrêté. Dans ce cas, connecte-toi en SSH sur le VPS :

```bash
ssh openclaw@<IP_DU_VPS>
cd ~/openclaw
```

### Relancer le gateway

```bash
docker compose restart openclaw-gateway
```

### Voir ce qui ne va pas

```bash
docker compose logs --tail=50 openclaw-gateway
```

### Vérifier que les conteneurs tournent

```bash
docker compose ps
```

Dans 90% des cas, un `restart` suffit. Une fois le gateway relancé,
l'agent principal est à nouveau joignable en Discord.

---

## Commandes CLI utiles (référence)

Pour les cas plus complexes, voici les commandes à connaître.
Dans la plupart des cas, tu peux aussi les demander à l'agent principal.

```bash
# Diagnostic complet
docker compose run --rm openclaw-cli doctor

# Statut des canaux Discord
docker compose run --rm openclaw-cli channels status --probe

# Audit sécurité
docker compose run --rm openclaw-cli security audit

# Lister les cron jobs (rappels actifs)
docker compose run --rm openclaw-cli cron list

# Supprimer un cron job par son ID
docker compose run --rm openclaw-cli cron remove <job-id>

# Lister les agents configurés
docker compose run --rm openclaw-cli agents list

# Voir la configuration des bindings Discord
docker compose run --rm openclaw-cli config get bindings
```

---

## Mettre à jour OpenClaw manuellement

```bash
cd ~/openclaw
docker compose pull
docker compose up -d
```

---

## Sauvegarder avant une modification importante

```bash
tar czf ~/backup-openclaw-$(date +%Y%m%d).tar.gz \
  ~/openclaw/config \
  ~/openclaw/data \
  ~/openclaw/workspace \
  ~/openclaw/workspace-assistant \
  ~/openclaw/.env
```

---

## Où sont les fichiers (référence rapide)

| Quoi | Où sur le VPS |
|---|---|
| Config OpenClaw | `~/openclaw/config/openclaw.json` |
| Workspace agent principal | `~/openclaw/workspace/` |
| Workspace agent assistant | `~/openclaw/workspace-assistant/` |
| Secrets (.env) | `~/openclaw/.env` |
| Logs | `docker compose logs openclaw-gateway` |
| Sessions (historique) | `~/openclaw/data/agents/` |

---

**Le tuto est terminé.** L'agent assistant est opérationnel, personnalisé,
et tu sais comment le maintenir — en parlant à l'agent principal pour 99% des cas,
et en SSH uniquement quand tout est silencieux.
