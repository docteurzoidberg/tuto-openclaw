# PROMPT.md — Contexte de session pour reprise

Ce fichier permet à un agent de reprendre le travail sur ce projet sans historique de session.

---

## Projet

**Tuto OpenClaw** — Tutoriel complet pour installer et utiliser OpenClaw sur un VPS avec un bot Discord.
Destiné à un utilisateur technique mais débutant sur OpenClaw.

Repo GitHub : https://github.com/docteurzoidberg/tuto-openclaw

---

## Stack technique

- **VPS :** OVH, Linux (Ubuntu 22.04/24.04 ou Debian 11/12), déjà installé avec Docker + Compose v2
- **Déploiement OpenClaw :** Docker Compose, image officielle `ghcr.io/openclaw/openclaw:latest`
- **Messagerie :** Discord
- **Provider IA :** GitHub Copilot ou Claude Code (au choix de l'utilisateur)
- **Accès UI :** SSH tunnel (phase 1) — reverse proxy ou VPN mesh prévu en Partie 3

---

## Structure du repo

```
tuto-openclaw/
├── README.md                          ← index général + table des matières complète
├── PROMPT.md                          ← ce fichier
├── 01-setup-installation/             ← Partie 1, 6 étapes, COMPLÈTE
│   ├── README.md
│   └── etapes/
│       ├── etape-01-setup-vps.md
│       ├── etape-02-installation.md
│       ├── etape-03-discord-bot.md
│       ├── etape-04-config-discord.md
│       ├── etape-05-ssh-tunnel.md
│       └── etape-06-audit.md
├── 02-configuration-utilisation/      ← Partie 2, 10 étapes, COMPLÈTE
│   ├── README.md
│   └── etapes/
│       ├── etape-01-concepts.md
│       ├── etape-02-agent-principal.md
│       ├── etape-03-creer-agent.md
│       ├── etape-04-discord-binding.md
│       ├── etape-05-bootstrap-interview.md
│       ├── etape-06-soul.md
│       ├── etape-07-memory.md
│       ├── etape-08-usages.md
│       ├── etape-09-skill-custom.md
│       └── etape-10-cli-ssh.md
└── 03-hardening/                      ← Partie 3, OPTIONNELLE, structure en place
    ├── README.md
    └── etapes/
        ├── etape-01-principes.md      ← complet (comparatif reverse proxy vs VPN)
        ├── etape-02-reverse-proxy.md  ← 🚧 placeholder, technologie non choisie
        ├── etape-03-vpn-mesh.md       ← 🚧 placeholder, technologie non choisie
        ├── etape-04-fail2ban.md       ← complet
        └── etape-05-audit-final.md    ← complet
```

---

## Décisions actées

- **Partie 3 optionnelle** : les étapes 2 et 3 sont des placeholders — la technologie (Traefik/Caddy/Nginx pour le reverse proxy, Tailscale/WireGuard pour le VPN) n'a pas encore été choisie. Fail2Ban est documenté, le reste attend.
- **Format Markdown GFM** (GitHub Flavored Markdown) avec admonitions `> [!NOTE]` / `> [!WARNING]` / `> [!IMPORTANT]` rendues nativement par GitHub.
- **Pas de tables dans les fichiers Discord** (hors scope ici — le tuto est GitHub uniquement pour l'instant).
- **GitHub Pages** envisagé plus tard — la structure des fichiers est compatible Jekyll/MkDocs.

---

## Git

- **Remote :** `https://github.com/docteurzoidberg/tuto-openclaw.git`
- **Branche :** `main`
- **Identité git :** `DrZoid <docteurzoidberg@users.noreply.github.com>`
- **Token GitHub :** stocké sur le serveur OpenClaw dans `~/.openclaw/secrets/github-tokens.env`
  - Variable : `GITHUB_TOKEN_TUTO_OPENCLAW`
  - Usage : `source ~/.openclaw/secrets/github-tokens.env` puis push avec le token dans l'URL remote

---

## Roadmap

- [x] Partie 1 — Setup VPS & Installation (6 étapes)
- [x] Partie 2 — Configuration & Utilisation (10 étapes)
- [x] Partie 3 — Hardening (structure + principes + Fail2Ban — placeholders reverse proxy/VPN)
- [ ] Partie 3 — Compléter étape 2 (reverse proxy) une fois la techno choisie
- [ ] Partie 3 — Compléter étape 3 (VPN mesh) une fois la techno choisie
- [ ] Envisager GitHub Pages pour rendre le tuto navigable en ligne

---

## Comment reprendre

1. Lire ce fichier
2. Lire le `README.md` racine pour la vue d'ensemble
3. Lire les `README.md` de chaque partie pour le détail des étapes
4. Pour pusher : `source ~/.openclaw/secrets/github-tokens.env` puis configurer la remote avec le token
