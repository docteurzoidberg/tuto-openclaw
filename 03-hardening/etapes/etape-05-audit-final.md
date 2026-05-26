# Étape 5 — Audit final et checklist hardening

---

## Audit OpenClaw

```bash
docker compose run --rm openclaw-cli security audit --deep
```

---

## Checklist complète post-hardening

### Infrastructure VPS

```
[x] User dédié openclaw, pas de root
[x] SSH : auth par clé uniquement, PasswordAuthentication no, PermitRootLogin no
[x] UFW actif :
    - SSH ouvert
    - Port 18789 fermé
    - 443/80 ouverts uniquement si Option A (reverse proxy)
[x] Fail2Ban actif, jail SSH configurée
[ ] Mises à jour automatiques de sécurité activées (optionnel)
```

### Exposition de l'UI

```
Option A — Reverse proxy
  [ ] Reverse proxy configuré et actif
  [ ] Certificat SSL valide (Let's Encrypt)
  [ ] Auth middleware en place
  [ ] Port 18789 toujours inaccessible directement depuis Internet

Option B — VPN mesh
  [ ] Client VPN installé sur VPS et appareils utilisateurs
  [ ] Connectivité mesh vérifiée
  [ ] UI accessible via IP mesh uniquement
  [ ] SSH tunnel devenu inutile (ou conservé en fallback)
```

### OpenClaw

```
[x] gateway.bind = "lan" ou "tailnet" — jamais 0.0.0.0
[x] OPENCLAW_GATEWAY_TOKEN présent et fort
[x] Token Discord en variable d'env (jamais en clair)
[x] groupPolicy = "allowlist" sur Discord
[x] Security audit sans erreur
[x] Health checks /healthz et /readyz OK
[x] Restart policy unless-stopped active
```

### Fichiers & secrets

```
[x] .env en permissions 600
[x] config/ en permissions 700
[x] data/ en permissions 700
[x] Aucun secret versionné dans Git
```

---

## Mises à jour de sécurité automatiques (optionnel)

Pour que le VPS applique automatiquement les correctifs de sécurité :

```bash
sudo apt install unattended-upgrades -y
sudo dpkg-reconfigure --priority=low unattended-upgrades
```

---

## Commandes de vérification rapide

```bash
# UFW
sudo ufw status verbose

# Fail2Ban
sudo fail2ban-client status sshd

# OpenClaw
docker compose run --rm openclaw-cli security audit
docker compose run --rm openclaw-cli channels status --probe

# Permissions
ls -la ~/openclaw/.env
ls -la ~/openclaw/config/
```

---

**La partie hardening est terminée.** Ton VPS et OpenClaw sont dans un état sécurisé adapté à un usage personnel sur Internet.
