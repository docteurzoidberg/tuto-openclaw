# 📘 Tuto OpenClaw

Tutoriel complet pour installer et utiliser OpenClaw sur un VPS avec un bot Discord.

## Structure

```
tuto-openclaw/
├── 01-setup-installation/     ← Du VPS vierge au bot Discord opérationnel
│   ├── README.md
│   └── etapes/                ← 6 étapes
└── 02-configuration-utilisation/  ← Agent assistant personnel
    ├── README.md
    └── etapes/                ← 10 étapes
```

## Parties

### [Partie 1 — Setup VPS & Installation](01-setup-installation/README.md)

Du VPS vierge au bot Discord opérationnel.

| # | Étape |
|---|---|
| 1 | Préparer le VPS (user dédié, SSH, UFW) |
| 2 | Installer OpenClaw via Docker Compose |
| 3 | Créer le bot Discord (Developer Portal) |
| 4 | Connecter Discord à OpenClaw |
| 5 | Accéder à l'UI via SSH tunnel |
| 6 | Audit sécurité & vérifications finales |

### [Partie 2 — Configuration & Utilisation](02-configuration-utilisation/README.md)

Créer un agent assistant personnel, le personnaliser, l'utiliser au quotidien.

| # | Étape |
|---|---|
| 1 | Concepts : agent principal vs agents spécialisés |
| 2 | Personnaliser l'agent principal |
| 3 | Créer l'agent assistant |
| 4 | Attacher l'agent aux channels Discord |
| 5 | Prompt de bootstrap : interview utilisateur |
| 6 | Personnaliser la SOUL (personnalité, ton, langue) |
| 7 | Configurer la mémoire longue durée |
| 8 | Cas d'usage : notes, rappels, todolists |
| 9 | Créer un skill custom |
| 10 | Debug et administration via CLI SSH |

## Stack technique

- **VPS :** OVH, Linux (Ubuntu 22.04/24.04 ou Debian 11/12)
- **Déploiement :** Docker Compose
- **Messagerie :** Discord
- **Provider IA :** GitHub Copilot ou Claude Code (au choix)
- **Accès UI :** SSH tunnel (phase 1)

## Roadmap

- [ ] Partie 3 — Hardening (Fail2Ban, Traefik ou Tailscale)
