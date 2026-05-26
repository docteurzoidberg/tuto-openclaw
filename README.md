# 📘 Projet : Tuto OpenClaw

Tutoriel complet pour installer OpenClaw sur un VPS et configurer un bot Discord.
Destiné à un utilisateur technique mais débutant sur OpenClaw.

## Structure

```
tuto-openclaw/
├── README.md           ← ce fichier (index du projet)
├── etapes/             ← une étape = un fichier Markdown finalisé
├── drafts/             ← brouillons en cours
└── assets/             ← captures, schémas, etc.
```

## Stack technique

- **Déploiement :** Docker Compose
- **Provider VPS :** OVH (Linux, Ubuntu ou Debian)
- **Accès UI :** SSH tunnel (phase 1) — reverse proxy / Tailscale à planifier plus tard
- **Niveau lecteur :** technique, à l'aise SSH/Linux, découverte d'OpenClaw

## Étapes

- [x] [Étape 1 — Setup VPS](etapes/etape-01-setup-vps.md) — user dédié, SSH hardening, UFW, Docker
- [x] [Étape 2 — Installation OpenClaw](etapes/etape-02-installation.md) — Docker Compose, onboarding, GitHub Copilot / Claude Code
- [x] [Étape 3 — Créer le bot Discord](etapes/etape-03-discord-bot.md) — Developer Portal, token, IDs
- [x] [Étape 4 — Connecter Discord à OpenClaw](etapes/etape-04-config-discord.md) — config, pairing, DMs, channels
- [x] [Étape 5 — Accès à l'UI via SSH tunnel](etapes/etape-05-ssh-tunnel.md)
- [x] [Étape 6 — Audit sécurité & vérifications finales](etapes/etape-06-audit.md)
- [ ] (Futur) Hardening : Fail2Ban, reverse proxy ou Tailscale

## Décisions actées

- Docker Compose (pas bare-metal)
- SSH tunnel uniquement pour l'accès UI dans un premier temps
- Fail2Ban et reverse proxy/Tailscale reportés au tuto hardening
- Token Discord stocké en variable d'env (jamais en clair)

## Statut

✅ Tuto 1 — Setup complet (6 étapes)
🟡 Tuto 2 — Agent assistant personnel (en cours de planification)
