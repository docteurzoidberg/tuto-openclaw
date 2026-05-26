# 📘 Tuto 2 — Configurer un agent assistant personnel

Créer, personnaliser et utiliser un agent OpenClaw dédié à l'assistance personnelle.

## Objectif

À la fin de ce tuto :
- Comprendre la différence entre **agent principal** et **agents spécialisés**
- Avoir un **agent principal** personnalisé pour les tâches OpenClaw (créer agents, projets, bots…)
- Avoir un **agent assistant personnel** attaché à des channels Discord dédiés
- L'assistant prend des notes, envoie des rappels, gère des todolists
- Personnalité custom via `SOUL.md`, mémoire via `MEMORY.md`
- Un skill custom créé de zéro
- Accès admin via le CLI OpenClaw sur le VPS en SSH

## Stack & décisions

| Élément | Choix |
|---|---|
| **Agent principal** | `agent:main` — tâches OpenClaw, création d'agents, admin |
| **Agent assistant** | `agent:assistant` — notes, rappels, todos |
| **Channels Discord** | `#assistant`, `#notes`, `#rappels`, `#todo` → tous sur l'agent assistant |
| **Accès admin** | CLI OpenClaw sur le VPS via SSH (pas UI web ni app desktop) |
| **Bootstrap** | Prompt dédié pour que l'agent principal interview l'utilisateur |

## Étapes

- [ ] [Étape 1 — Concepts : agent principal vs agents spécialisés](etapes/etape-01-concepts.md)
- [ ] [Étape 2 — Personnaliser l'agent principal](etapes/etape-02-agent-principal.md)
- [ ] [Étape 3 — Créer l'agent assistant](etapes/etape-03-creer-agent.md)
- [ ] [Étape 4 — Attacher l'agent aux channels Discord](etapes/etape-04-discord-binding.md)
- [ ] [Étape 5 — Prompt de bootstrap : interview utilisateur](etapes/etape-05-bootstrap-interview.md)
- [ ] [Étape 6 — Personnaliser la SOUL (personnalité, ton, langue)](etapes/etape-06-soul.md)
- [ ] [Étape 7 — Configurer la mémoire longue durée (MEMORY.md)](etapes/etape-07-memory.md)
- [ ] [Étape 8 — Cas d'usage : notes, rappels (cron), todolists](etapes/etape-08-usages.md)
- [ ] [Étape 9 — Créer un skill custom (todolist Markdown)](etapes/etape-09-skill-custom.md)
- [ ] [Étape 10 — Debug et administration via CLI SSH](etapes/etape-10-cli-ssh.md)

## Statut

🟡 En cours de rédaction
