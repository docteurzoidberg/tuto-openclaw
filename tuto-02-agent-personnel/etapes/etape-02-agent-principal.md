# Étape 2 — Personnaliser l'agent principal

> [!NOTE]
> L'agent principal est déjà créé par l'onboarding.
> On va explorer son workspace, comprendre ses fichiers, et le personnaliser.

---

## 1. Accéder au workspace de l'agent principal

Sur le VPS via SSH :

```bash
ls ~/openclaw/workspace/
```

Tu dois voir les fichiers bootstrap créés par l'onboarding :

```
AGENTS.md
SOUL.md
IDENTITY.md
USER.md
TOOLS.md
HEARTBEAT.md
MEMORY.md
```

> [!NOTE]
> Si certains fichiers sont manquants, tu peux les régénérer :
> ```bash
> docker compose run --rm openclaw-cli setup
> ```

---

## 2. Renseigner le profil utilisateur (`USER.md`)

Ce fichier dit à l'agent principal qui tu es : ta langue, ton fuseau horaire, comment il doit t'appeler.

```bash
nano ~/openclaw/workspace/USER.md
```

Exemple minimal :

```markdown
# USER.md - About Your Human

- **Name:** [Ton prénom ou pseudo]
- **What to call them:** [Comment il doit t'appeler]
- **Timezone:** Europe/Paris
- **Notes:** Préfère les réponses en français.
```

---

## 3. Donner une identité à l'agent principal (`IDENTITY.md`)

```bash
nano ~/openclaw/workspace/IDENTITY.md
```

Exemple :

```markdown
# IDENTITY.md

- **Name:** HAL
- **Creature:** Assistant IA personnel
- **Vibe:** Direct, efficace, sobre
- **Emoji:** 🤖
```

---

## 4. Définir la personnalité (`SOUL.md`)

Le `SOUL.md` définit le **comportement** de l'agent : son ton, ses principes, ce qu'il fait et ne fait pas.

> [!IMPORTANT]
> Ce fichier a un impact direct sur la qualité des réponses.
> Prends le temps de le remplir avec soin — tu pourras l'affiner au fil du temps.

```bash
nano ~/openclaw/workspace/SOUL.md
```

Template de base pour un agent principal "admin OpenClaw" :

```markdown
# SOUL.md - Agent Principal

## Rôle

Je suis l'agent principal d'OpenClaw. Mon rôle est la gestion et l'administration
du gateway : créer des agents, configurer des channels, gérer des projets.

## Principes

- **Direct.** Pas de remplissage, pas de formules creuses.
- **Précis.** Avant d'agir, je lis le contexte. Je confirme avant les actions destructives.
- **Transparent.** Je dis ce que je fais et pourquoi.

## Limites

- Je ne prends pas de décision importante sans confirmation.
- Ce qui est privé reste privé.

## Langue

Réponses en français par défaut.
```

---

## 5. Initialiser la mémoire longue durée (`MEMORY.md`)

Le `MEMORY.md` de l'agent principal mémorisera ses décisions et son contexte au fil du temps.
Pour l'instant, on l'initialise avec les infos de base :

```bash
nano ~/openclaw/workspace/MEMORY.md
```

```markdown
# MEMORY.md - Mémoire Long Terme

## Setup

- Installation OpenClaw complétée le : [DATE]
- Provider IA : [GitHub Copilot / Claude Code]
- VPS : OVH

## Agents configurés

- `agent:main` — agent principal, administration OpenClaw
- (agent:assistant à venir)

## Notes
```

---

## 6. Redémarrer le gateway pour appliquer

```bash
cd ~/openclaw && docker compose restart openclaw-gateway
```

Envoie un DM au bot Discord pour vérifier que l'agent répond avec sa nouvelle identité.

---

## Récapitulatif

- [x] `USER.md` renseigné (langue, timezone, préférences)
- [x] `IDENTITY.md` avec nom et emoji
- [x] `SOUL.md` avec rôle, principes, limites
- [x] `MEMORY.md` initialisé
- [x] Gateway redémarré

➡️ **Étape suivante : [Étape 3 — Créer l'agent assistant](./etape-03-creer-agent.md)**
