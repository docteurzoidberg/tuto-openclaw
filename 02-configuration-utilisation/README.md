# 📘 Partie 2 — Configuration & Utilisation

Créer un agent assistant personnel en parlant à l'agent principal.
Pas de fichiers de config à éditer — tu converses, l'agent fait.

## Prérequis

- Partie 1 complétée (OpenClaw installé, bot Discord connecté)

## Canal principal : Discord

Tout ce tuto se fait en **DM Discord avec ton bot**.
C'est la façon naturelle d'utiliser OpenClaw — tu parles, l'agent agit.

## Fallback : CLI sur le VPS

Si Discord n'est pas disponible (bot muet, problème réseau, gateway à relancer),
tu peux interagir avec l'agent directement depuis le VPS via SSH :

```bash
ssh openclaw@<IP_DU_VPS>
cd ~/openclaw
docker compose run --rm openclaw-cli chat
```

Tu te retrouves dans un terminal de chat avec l'agent principal.
Mêmes capacités qu'en Discord — tu peux envoyer exactement les mêmes prompts.
Tape `exit` ou `Ctrl+C` pour quitter.

> [!NOTE]
> Le CLI est un **fallback**, pas le canal principal.
> Dans la suite du tuto, tous les exemples sont en Discord.
> Si tu es en CLI, les prompts sont identiques — copie-colle.

## Étapes

- [x] [Étape 1 — Concepts : agent principal vs agents spécialisés](etapes/etape-01-concepts.md)
- [x] [Étape 2 — Premier contact avec l'agent principal](etapes/etape-02-premier-contact.md)
- [x] [Étape 3 — Créer l'agent assistant](etapes/etape-03-creer-agent.md)
- [x] [Étape 4 — Personnaliser l'agent assistant](etapes/etape-04-personnalisation.md)
- [x] [Étape 5 — Notes, rappels et todos](etapes/etape-05-usages.md)
- [x] [Étape 6 — Créer un skill custom](etapes/etape-06-skill-custom.md)
- [x] [Étape 7 — Maintenance et debug](etapes/etape-07-maintenance.md)

## Annexes

- [Mémo — Commandes Discord (slash commands)](etapes/etape-00-commandes-discord.md)
- [Mémo — Commandes CLI OpenClaw](etapes/annexe-commandes-cli.md)

## Résultat

À la fin de cette partie, tu as :
- Un agent principal configuré à ton goût
- Un agent assistant personnel avec sa propre personnalité et sa propre mémoire
- 4 channels Discord dédiés (`#assistant`, `#notes`, `#rappels`, `#todo`)
- Notes, rappels et todos opérationnels — gérés en langage naturel
- Un skill custom pour la gestion des tâches
- Les clés pour maintenir et débugger via l'agent ou le CLI en dernier recours
