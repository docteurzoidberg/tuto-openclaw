# 📘 Tuto OpenClaw

Tutoriel complet pour installer et utiliser OpenClaw sur un VPS avec un bot Discord.

> **Stack :** OVH VPS · Linux · Docker Compose · Discord · GitHub Copilot ou Claude Code

---

## 📂 Organisation

| Dossier | Contenu |
|---|---|
| [`01-setup-installation/`](01-setup-installation/README.md) | Du VPS vierge au bot Discord opérationnel |
| [`02-configuration-utilisation/`](02-configuration-utilisation/README.md) | Agent assistant personnel, personnalisation, cas d'usage |

---

## 📋 Table des matières

### [Partie 1 — Setup VPS & Installation](01-setup-installation/README.md)

> Du VPS vierge au bot Discord opérationnel — 6 étapes

| # | Étape | Description |
|---|---|---|
| 1 | [Préparer le VPS](01-setup-installation/etapes/etape-01-setup-vps.md) | User dédié, SSH hardening, UFW, Docker |
| 2 | [Installer OpenClaw](01-setup-installation/etapes/etape-02-installation.md) | Docker Compose, onboarding, GitHub Copilot / Claude Code |
| 3 | [Créer le bot Discord](01-setup-installation/etapes/etape-03-discord-bot.md) | Developer Portal, token, Server ID, User ID |
| 4 | [Connecter Discord à OpenClaw](01-setup-installation/etapes/etape-04-config-discord.md) | Config, pairing, DMs, channels |
| 5 | [Accéder à l'UI via SSH tunnel](01-setup-installation/etapes/etape-05-ssh-tunnel.md) | Tunnel SSH, gateway token, alias |
| 6 | [Audit sécurité & vérifications finales](01-setup-installation/etapes/etape-06-audit.md) | Checklist complète, commandes de maintenance |

---

### [Partie 2 — Configuration & Utilisation](02-configuration-utilisation/README.md)

> Agent assistant personnel : personnalisation, mémoire, skills, cas d'usage — 10 étapes

| # | Étape | Description |
|---|---|---|
| 1 | [Concepts : agent principal vs spécialisés](02-configuration-utilisation/etapes/etape-01-concepts.md) | Architecture multi-agents, workspaces, bindings |
| 2 | [Personnaliser l'agent principal](02-configuration-utilisation/etapes/etape-02-agent-principal.md) | USER.md, IDENTITY.md, SOUL.md, MEMORY.md |
| 3 | [Créer l'agent assistant](02-configuration-utilisation/etapes/etape-03-creer-agent.md) | Workspace dédié, déclaration config, Docker mounts |
| 4 | [Attacher l'agent aux channels Discord](02-configuration-utilisation/etapes/etape-04-discord-binding.md) | Bindings `#assistant` `#notes` `#rappels` `#todo` |
| 5 | [Prompt de bootstrap : interview utilisateur](02-configuration-utilisation/etapes/etape-05-bootstrap-interview.md) | L'agent principal génère les fichiers de config |
| 6 | [Personnaliser la SOUL](02-configuration-utilisation/etapes/etape-06-soul.md) | Personnalité, ton, langue, principes, limites |
| 7 | [Configurer la mémoire longue durée](02-configuration-utilisation/etapes/etape-07-memory.md) | MEMORY.md, notes quotidiennes, cycle de mémorisation |
| 8 | [Cas d'usage : notes, rappels, todolists](02-configuration-utilisation/etapes/etape-08-usages.md) | Exemples concrets par channel |
| 9 | [Créer un skill custom](02-configuration-utilisation/etapes/etape-09-skill-custom.md) | Skill todolist Markdown pas à pas |
| 10 | [Debug et administration via CLI SSH](02-configuration-utilisation/etapes/etape-10-cli-ssh.md) | Commandes essentielles, scénarios de debug |

---

## 🗺️ Roadmap

- [x] Partie 1 — Setup VPS & Installation
- [x] Partie 2 — Configuration & Utilisation
- [ ] Partie 3 — Hardening (Fail2Ban, Traefik ou Tailscale)
