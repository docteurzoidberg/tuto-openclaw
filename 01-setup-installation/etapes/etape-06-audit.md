# Étape 6 — Vérifications finales et audit sécurité

> [!NOTE]
> On vérifie que tout est en ordre avant de déclarer le setup terminé.
> Ces commandes sont à relancer après chaque modification importante de la config.

---

## 1. Santé du gateway

```bash
cd ~/openclaw

# Conteneur en cours d'exécution ?
docker compose ps

# Health checks HTTP
curl -fsS http://127.0.0.1:18789/healthz && echo "liveness OK"
curl -fsS http://127.0.0.1:18789/readyz  && echo "readiness OK"

# Diagnostic complet
docker compose run --rm openclaw-cli doctor
```

---

## 2. Audit sécurité OpenClaw

```bash
# Audit de base
docker compose run --rm openclaw-cli security audit

# Audit approfondi
docker compose run --rm openclaw-cli security audit --deep
```

OpenClaw vérifie automatiquement :
- Exposition réseau du gateway (doit être loopback)
- Permissions des fichiers de config et secrets
- Politique des canaux (pas de `groupPolicy: open`)
- Token gateway présent et actif

> [!IMPORTANT]
> Si l'audit remonte des warnings, corrige-les avant de continuer.
> La commande `--fix` peut corriger automatiquement les problèmes courants :
> ```bash
> docker compose run --rm openclaw-cli security audit --fix
> ```

---

## 3. Vérifier le firewall UFW

```bash
sudo ufw status verbose
```

Résultat attendu — **seul SSH doit être ouvert** :

```
Status: active

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    Anywhere
```

> [!WARNING]
> Si tu vois le port `18789` ouvert dans UFW, ferme-le immédiatement :
> ```bash
> sudo ufw deny 18789
> sudo ufw reload
> ```

---

## 4. Vérifier les permissions des fichiers sensibles

```bash
# .env : doit être 600 (lecture/écriture propriétaire uniquement)
ls -la ~/openclaw/.env

# Dossier config : doit être 700
ls -la ~/openclaw/config/
```

Si les permissions sont trop ouvertes :

```bash
chmod 600 ~/openclaw/.env
chmod 700 ~/openclaw/config ~/openclaw/data
```

---

## 5. Vérifier le statut des canaux

```bash
docker compose run --rm openclaw-cli channels status --probe
```

Tu dois voir Discord avec le statut `connected`.

---

## 6. Vérifier la politique Discord

```bash
docker compose run --rm openclaw-cli config get channels.discord.groupPolicy
```

Doit retourner `allowlist`. **Jamais `open`** sur un serveur en production.

---

## Checklist finale

```
Infrastructure
  [x] UFW actif — seul SSH ouvert, port 18789 fermé
  [x] User openclaw dédié, pas de root
  [x] SSH : auth par clé, login root désactivé

Fichiers & secrets
  [x] .env en permissions 600
  [x] Token Discord stocké en variable d'env (jamais en clair)
  [x] OPENCLAW_GATEWAY_TOKEN généré et présent dans .env

OpenClaw
  [x] Gateway up, health checks OK
  [x] Audit sécurité sans erreur
  [x] Discord connecté (channels status = connected)
  [x] groupPolicy = allowlist
  [x] Bot répond en DM ✓
  [x] Bot répond dans les channels du serveur ✓
  [x] UI accessible via SSH tunnel ✓
  [x] Restart policy unless-stopped active ✓
```

---

## Commandes de maintenance utiles

```bash
# Redémarrer le gateway
cd ~/openclaw && docker compose restart

# Voir les logs en direct
docker compose logs -f openclaw-gateway

# Mettre à jour l'image OpenClaw
docker compose pull && docker compose up -d

# Sauvegarder la config
tar czf openclaw-backup-$(date +%Y%m%d).tar.gz ~/openclaw/config ~/openclaw/data ~/openclaw/.env
```

---

**Le setup de base est terminé.** Ton bot Discord est opérationnel, sécurisé, et redémarre automatiquement au boot.

➡️ **Suite : [Partie 2 — Configuration & Utilisation](../../02-configuration-utilisation/README.md)**
