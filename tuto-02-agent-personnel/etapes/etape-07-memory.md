# Étape 7 — Configurer la mémoire longue durée (MEMORY.md)

> [!NOTE]
> Par défaut, chaque session repart de zéro — l'agent ne se souvient de rien.
> `MEMORY.md` est le mécanisme qui permet à l'agent de **persister des informations
> entre les sessions**.

---

## Comment fonctionne la mémoire dans OpenClaw

```
Session 1                Session 2                Session 3
────────────             ────────────             ────────────
Tu dis: "J'aime        Agent lit MEMORY.md      Agent lit MEMORY.md
le café sans sucre"    → sait que tu aimes      → se souvient encore
                       le café sans sucre
Agent écrit dans
MEMORY.md
```

**Le cycle :**
1. Tu donnes une information en conversation
2. L'agent l'écrit dans `MEMORY.md` (ou un fichier daily)
3. À la prochaine session, il relit `MEMORY.md` et "se souvient"

---

## Les deux niveaux de mémoire

| Fichier | Type | Contenu |
|---|---|---|
| `MEMORY.md` | Longue durée | Infos stables, préférences, décisions importantes |
| `memory/YYYY-MM-DD.md` | Quotidien | Log des événements du jour, notes brèves |

---

## Structure recommandée de `MEMORY.md`

```bash
nano ~/openclaw/workspace-assistant/MEMORY.md
```

```markdown
# MEMORY.md — Mémoire Long Terme

## À propos de [Prénom]

- **Prénom :** [Prénom]
- **Timezone :** Europe/Paris
- **Langue :** Français
- **Préférences :** [ex: café sans sucre, réunions le matin, etc.]

## Habitudes et routines

- [ex: check des emails le matin vers 9h]
- [ex: réunion d'équipe chaque lundi 14h]

## Projets en cours

- [Projet 1] — [état]
- [Projet 2] — [état]

## Décisions importantes

- [Date] : [Décision prise]

## Notes diverses

- [Informations utiles à retenir]
```

---

## Apprendre à l'agent à utiliser sa mémoire

L'agent écrira dans `MEMORY.md` si sa `SOUL.md` lui dit de le faire (ce qu'on a configuré
à l'étape précédente). Mais tu peux aussi lui demander explicitement :

```
Souviens-toi que ma réunion hebdo est le lundi à 14h.
Note dans ta mémoire que j'utilise Python pour mes projets.
Qu'est-ce que tu sais de moi ?
```

---

## Mémoire quotidienne

Pour les notes du jour, l'agent crée automatiquement un fichier `memory/YYYY-MM-DD.md`
dans son workspace. Ces fichiers s'accumulent et l'agent peut les consulter pour
retrouver des informations récentes.

Tu peux lui demander :
```
Qu'est-ce qu'on a fait ensemble hier ?
Retrouve la note que j'ai faite la semaine dernière sur [sujet].
```

---

## Nettoyer la mémoire

Si la `MEMORY.md` grossit trop ou contient des infos obsolètes :

```bash
nano ~/openclaw/workspace-assistant/MEMORY.md
```

Ou demande directement à l'agent :
```
Retire de ta mémoire l'info sur [sujet], ce n'est plus d'actualité.
Fais le ménage dans ta mémoire, supprime les infos de plus de 3 mois.
```

---

## Récapitulatif

- [x] Rôle de MEMORY.md compris (longue durée vs. quotidien)
- [x] MEMORY.md initialisé avec les infos de base
- [x] SOUL.md configuré pour que l'agent écrive en mémoire
- [x] Agent testé : il mémorise les informations qu'on lui donne

➡️ **Étape suivante : [Étape 8 — Cas d'usage : notes, rappels, todolists](./etape-08-usages.md)**
