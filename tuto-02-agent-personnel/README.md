# 📘 Tuto 2 — Configurer un agent assistant personnel

Créer, personnaliser et utiliser un agent OpenClaw dédié à l'assistance personnelle.

## Objectif

À la fin de ce tuto :
- Comprendre la différence entre **agent principal** et **agents spécialisés**
- Avoir un **agent principal** personnalisé pour les tâches OpenClaw
- Avoir un **agent assistant personnel** sur un channel Discord dédié
- L'assistant prend des notes, envoie des rappels, gère des todolists
- Personnalité custom via `SOUL.md`, mémoire via `MEMORY.md`
- Un skill custom créé de zéro
- Accès CLI sur le VPS via SSH pour debug et administration

## Décisions de conception

- **Agent principal** (`agent:main`) → tâches OpenClaw (créer agents, projets, bots…) + personnalisé avec SOUL/MEMORY
- **Agent assistant** → agent secondaire nommé par l'utilisateur, attaché à des channels Discord dédiés (`#assistant`, `#notes`, `#rappels`, `#todo`)
- **Prompt de bootstrap** → l'agent principal pose les bonnes questions pour configurer l'assistant (personnalité, préférences, cas d'usage)
- **Accès admin** → CLI OpenClaw sur le VPS via SSH (pas UI web ni app desktop)

## Étapes prévues

- [ ] Étape 1 — Concepts : agent principal vs agents spécialisés
- [ ] Étape 2 — Personnaliser l'agent principal (onboarding, SOUL, MEMORY)
- [ ] Étape 3 — Créer l'agent assistant personnel
- [ ] Étape 4 — Attacher l'agent à des channels Discord (`#assistant`, `#notes`, `#rappels`, `#todo`)
- [ ] Étape 5 — Prompt de bootstrap : laisser l'agent principal interviewer l'utilisateur
- [ ] Étape 6 — Personnaliser la SOUL de l'assistant (personnalité, ton, langue)
- [ ] Étape 7 — Configurer la mémoire longue durée (MEMORY.md)
- [ ] Étape 8 — Cas d'usage : notes, rappels (cron), todolists
- [ ] Étape 9 — Créer un skill custom (exemple : todolist en Markdown)
- [ ] Étape 10 — Accès et debug via le CLI OpenClaw sur le VPS (SSH)

## Statut

🟡 En cours de planification
