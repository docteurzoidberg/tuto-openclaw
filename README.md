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

## Étapes prévues

- [ ] Étape 1 — Setup VPS (vérifs OS, user dédié, SSH hardening, UFW, Docker Compose)
- [ ] Étape 2 — Installation OpenClaw via Docker Compose
- [ ] Étape 3 — Création du bot Discord (Developer Portal)
- [ ] Étape 4 — Configuration OpenClaw + Discord
- [ ] Étape 5 — Accès à l'UI via SSH tunnel
- [ ] Étape 6 — Audit sécurité & vérifications finales
- [ ] (Futur) Étape 7 — Hardening : Fail2Ban, reverse proxy ou Tailscale

## Décisions actées

- Docker Compose (pas bare-metal)
- SSH tunnel uniquement pour l'accès UI dans un premier temps
- Fail2Ban et reverse proxy/Tailscale reportés au tuto hardening
- Token Discord stocké en variable d'env (jamais en clair)

## Statut

🟡 En cours de planification — Étape 1
