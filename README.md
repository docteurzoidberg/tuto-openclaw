# 📘 Tuto OpenClaw

Tutoriel complet pour installer et utiliser OpenClaw sur un VPS avec un bot Discord.

> **Stack :** OVH VPS · Linux · Docker Compose · Discord · GitHub Copilot ou Claude Code

---

## 📂 Organisation

| Dossier | Contenu |
|---|---|
| [`01-setup-installation/`](01-setup-installation/README.md) | Du VPS vierge au bot Discord opérationnel |
| [`02-configuration-utilisation/`](02-configuration-utilisation/README.md) | Agent assistant personnel, personnalisation, cas d'usage |
| [`03-hardening/`](03-hardening/README.md) | *(Optionnel)* Exposition sécurisée de l'UI, Fail2Ban, audit final |

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

> Agent assistant personnel : création via conversation, personnalisation, cas d'usage — 7 étapes
> **Philosophie : tu parles à l'agent principal, il fait le reste.**

| # | Étape | Description |
|---|---|---|
| 1 | [Concepts : agent principal vs spécialisés](02-configuration-utilisation/etapes/etape-01-concepts.md) | Architecture multi-agents, workspaces, bindings |
| 2 | [Premier contact avec l'agent principal](02-configuration-utilisation/etapes/etape-02-premier-contact.md) | DM Discord, personnalisation via conversation |
| 3 | [Créer l'agent assistant](02-configuration-utilisation/etapes/etape-03-creer-agent.md) | Un prompt → l'agent crée workspace, config, bindings Discord |
| 4 | [Personnaliser l'agent assistant](02-configuration-utilisation/etapes/etape-04-personnalisation.md) | Interview par l'agent principal → SOUL, IDENTITY, USER, MEMORY |
| 5 | [Notes, rappels et todos](02-configuration-utilisation/etapes/etape-05-usages.md) | Langage naturel, ce que l'agent fait en coulisses |
| 6 | [Créer un skill custom](02-configuration-utilisation/etapes/etape-06-skill-custom.md) | L'agent crée son propre skill sur demande |
| 7 | [Maintenance et debug](02-configuration-utilisation/etapes/etape-07-maintenance.md) | Agent principal d'abord, CLI en dernier recours |

---

### [Partie 3 — Hardening](03-hardening/README.md) *(optionnel)*

> Exposition sécurisée de l'UI, protection du VPS — technologie à choisir

| # | Étape | Description |
|---|---|---|
| 1 | [Principes : exposition sécurisée de l'UI](03-hardening/etapes/etape-01-principes.md) | Reverse proxy vs VPN mesh, comparatif |
| 2 | [Option A : Reverse proxy](03-hardening/etapes/etape-02-reverse-proxy.md) | 🚧 Traefik / Caddy / Nginx — à choisir |
| 3 | [Option B : VPN mesh](03-hardening/etapes/etape-03-vpn-mesh.md) | 🚧 Tailscale / WireGuard — à choisir |
| 4 | [Fail2Ban](03-hardening/etapes/etape-04-fail2ban.md) | Protection SSH et services exposés |
| 5 | [Audit final et checklist hardening](03-hardening/etapes/etape-05-audit-final.md) | Checklist complète post-hardening |

---

## 🗺️ Roadmap

- [x] Partie 1 — Setup VPS & Installation
- [x] Partie 2 — Configuration & Utilisation
- [x] Partie 3 — Hardening *(structure + placeholders — technologie à choisir)*
