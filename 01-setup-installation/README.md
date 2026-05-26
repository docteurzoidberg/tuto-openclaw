# 📘 Partie 1 — Setup VPS & Installation OpenClaw

Du VPS vierge au bot Discord opérationnel.

## Prérequis

- Un VPS Linux (Ubuntu 22.04/24.04 ou Debian 11/12) chez OVH
- Accès SSH fonctionnel par clé
- Docker + Docker Compose v2 installés
- Un compte Discord et les droits pour créer une application

## Étapes

- [x] [Étape 1 — Préparer le VPS](etapes/etape-01-setup-vps.md) — user dédié, SSH hardening, UFW
- [x] [Étape 2 — Installer OpenClaw](etapes/etape-02-installation.md) — Docker Compose, onboarding, GitHub Copilot / Claude Code
- [x] [Étape 3 — Créer le bot Discord](etapes/etape-03-discord-bot.md) — Developer Portal, token, IDs
- [x] [Étape 4 — Connecter Discord à OpenClaw](etapes/etape-04-config-discord.md) — config, pairing, DMs, channels
- [x] [Étape 5 — Accéder à l'UI via SSH tunnel](etapes/etape-05-ssh-tunnel.md)
- [x] [Étape 6 — Audit sécurité & vérifications finales](etapes/etape-06-audit.md)

## Résultat

À la fin de cette partie, tu as :
- OpenClaw qui tourne en Docker sur ton VPS, redémarrage automatique
- Un bot Discord connecté qui répond en DM et dans les channels
- L'UI accessible en local via SSH tunnel
- Un audit sécurité sans erreur

➡️ **Suite : [Partie 2 — Configuration & Utilisation](../02-configuration-utilisation/README.md)**
