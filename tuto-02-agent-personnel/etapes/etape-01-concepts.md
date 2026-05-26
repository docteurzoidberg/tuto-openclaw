# Étape 1 — Concepts : agent principal vs agents spécialisés

> [!NOTE]
> Avant de créer quoi que ce soit, comprendre comment OpenClaw organise les agents.
> C'est la base pour tout ce qui suit.

---

## L'agent principal (`agent:main`)

Quand tu as installé OpenClaw, un agent a été créé automatiquement : l'**agent principal**.

C'est l'agent qui :
- répond par défaut à tous les messages entrants
- a accès à l'ensemble de ton workspace
- peut créer d'autres agents, modifier la configuration, gérer des projets
- est le point d'entrée naturel pour les tâches d'administration OpenClaw

**Think of it as the control room.** Tu lui parles pour tout ce qui concerne OpenClaw lui-même.

---

## Les agents spécialisés

OpenClaw permet de créer plusieurs agents, chacun avec :
- son propre **workspace** (fichiers, mémoire, notes)
- sa propre **personnalité** (`SOUL.md`)
- ses propres **outils** autorisés
- son propre **modèle IA** si besoin
- ses propres **channels Discord** attachés

Un agent spécialisé ne répond que dans les contextes qui lui sont assignés — il n'interfère pas avec les autres.

### Exemple de setup à 2 agents

```
┌─────────────────────────────────────────────────┐
│               OpenClaw Gateway                  │
│                                                  │
│  agent:main              agent:assistant         │
│  ├─ workspace/           ├─ workspace-assistant/ │
│  ├─ SOUL.md (admin)      ├─ SOUL.md (perso)      │
│  ├─ MEMORY.md            ├─ MEMORY.md            │
│  └─ Channels:            └─ Channels:            │
│     └─ Discord DM           ├─ #assistant        │
│                             ├─ #notes            │
│                             ├─ #rappels          │
│                             └─ #todo             │
└─────────────────────────────────────────────────┘
```

---

## Les workspaces

Chaque agent a son propre dossier workspace sur le VPS.
C'est là que vivent ses fichiers de personnalité, sa mémoire, ses notes.

| Fichier | Rôle |
|---|---|
| `AGENTS.md` | Instructions de comportement, règles de fonctionnement |
| `SOUL.md` | Personnalité, ton, style de réponse |
| `IDENTITY.md` | Nom, emoji, avatar |
| `USER.md` | Profil de l'utilisateur (préférences, langue, etc.) |
| `MEMORY.md` | Mémoire longue durée — persiste entre les sessions |
| `TOOLS.md` | Notes sur les outils disponibles |
| `HEARTBEAT.md` | Checklist pour les tâches périodiques proactives |

Ces fichiers sont **injectés automatiquement** dans le contexte de l'agent à chaque session.
L'agent les lit, les comprend, et s'en souvient.

---

## Les bindings Discord

Par défaut, tous les messages Discord vont vers l'**agent principal**.

Pour qu'un channel aille vers un agent spécialisé, on configure un **binding** :

```
Channel #notes  →  agent:assistant
Channel #rappels →  agent:assistant
DM Bot          →  agent:main  (par défaut)
```

Le binding est précis : seul le channel configuré est redirigé. Les autres restent sur l'agent principal.

---

## Ce qu'on va construire

```
Toi (Discord)
 │
 ├─ DM au bot           → agent:main    (admin OpenClaw)
 ├─ #assistant          → agent:assistant  (conversation générale)
 ├─ #notes              → agent:assistant  (prise de notes)
 ├─ #rappels            → agent:assistant  (rappels & cron)
 └─ #todo               → agent:assistant  (gestion de tâches)
```

➡️ **Étape suivante : [Étape 2 — Personnaliser l'agent principal](./etape-02-agent-principal.md)**
