# Étape 4 — Fail2Ban : protection contre les attaques par force brute

---

## Principe

Fail2Ban surveille les logs du système et bloque automatiquement les IPs
qui tentent trop de connexions échouées en peu de temps.

```
Attaquant        VPS
   │              │
   ├─ SSH fail ──►│ Fail2Ban détecte 5 échecs
   ├─ SSH fail ──►│ en moins de 10 minutes
   ├─ SSH fail ──►│
   ├─ SSH fail ──►│
   └─ SSH fail ──►│──► ban IP pour 1h via iptables/UFW
```

---

## Installation

```bash
sudo apt install fail2ban -y
sudo systemctl enable --now fail2ban
```

---

## Configuration de base

Fail2Ban utilise un fichier de config principal et des fichiers "jail" pour chaque service.

> [!IMPORTANT]
> Ne jamais modifier `/etc/fail2ban/jail.conf` directement — ce fichier est écrasé
> lors des mises à jour. Utiliser un fichier override à la place.

```bash
sudo nano /etc/fail2ban/jail.local
```

Config minimale :

```ini
[DEFAULT]
# Bannir pendant 1 heure
bantime  = 3600

# Fenêtre d'observation : 10 minutes
findtime = 600

# Nombre d'échecs avant bannissement
maxretry = 5

# Ne jamais bannir ces IPs (ta propre IP fixe si tu en as une)
# ignoreip = 127.0.0.1/8 ::1 TON_IP_FIXE

[sshd]
enabled = true
port    = ssh
logpath = %(sshd_log)s
backend = %(sshd_backend)s
```

Redémarrer Fail2Ban :

```bash
sudo systemctl restart fail2ban
```

---

## Vérifications

```bash
# Statut général
sudo fail2ban-client status

# Statut de la jail SSH
sudo fail2ban-client status sshd

# IPs actuellement bannies
sudo fail2ban-client status sshd | grep "Banned IP"

# Débannir une IP manuellement (si tu t'es banni toi-même)
sudo fail2ban-client set sshd unbanip <TON_IP>
```

---

## Étendre Fail2Ban à d'autres services

Si tu as un reverse proxy exposé (Option A), tu peux aussi protéger l'accès à l'UI :

```ini
# À adapter selon le reverse proxy choisi
[nginx-http-auth]
enabled = true
...

[traefik-auth]
enabled = true
...
```

> 🚧 *Section à compléter en fonction du reverse proxy choisi*

---

## Récapitulatif

- [x] Fail2Ban installé et actif
- [x] Jail SSH configurée (5 échecs → ban 1h)
- [x] `jail.local` utilisé (pas `jail.conf`)
- [x] Statut vérifié

➡️ **Étape suivante : [Étape 5 — Audit final et checklist hardening](./etape-05-audit-final.md)**
