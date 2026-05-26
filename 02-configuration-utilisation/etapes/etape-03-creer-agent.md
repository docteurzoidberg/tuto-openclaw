# Étape 3 — Créer l'agent assistant

> [!NOTE]
> On demande à l'agent principal de créer l'agent assistant à notre place.
> Un seul prompt suffit pour tout mettre en place.

---

## Créer les channels Discord d'abord

Avant de donner les instructions à l'agent, crée ces 4 channels texte sur ton serveur Discord :

- `#assistant` — conversation générale
- `#notes` — prise de notes
- `#rappels` — rappels et tâches planifiées
- `#todo` — liste de tâches

Pour chaque channel, récupère son **Channel ID** :
> Clic droit sur le channel → **Copier l'identifiant du channel** *(mode développeur requis — activé à la partie 1)*

---

## Le prompt de création

En DM avec l'agent principal, envoie ce message en remplaçant les IDs :

```
Je veux créer un second agent dédié à mon assistance personnelle.
Voici sa configuration :

- ID de l'agent : assistant
- Channels Discord à lui assigner :
  - #assistant → ID : XXXXXXXXXXXXXX
  - #notes     → ID : XXXXXXXXXXXXXX
  - #rappels   → ID : XXXXXXXXXXXXXX
  - #todo      → ID : XXXXXXXXXXXXXX

Peux-tu :
1. Créer un workspace dédié pour cet agent
2. Le déclarer dans la configuration OpenClaw
3. Configurer les bindings Discord pour ces 4 channels
4. Initialiser ses fichiers de base (SOUL.md, IDENTITY.md, USER.md, MEMORY.md, AGENTS.md)
5. Redémarrer le gateway pour appliquer les changements
```

L'agent va s'occuper de tout.

---

## Ce que l'agent va faire en coulisses

1. **Créer le workspace** `~/openclaw/workspace-assistant/` sur le VPS
2. **Déclarer l'agent** dans `openclaw.json` avec son workspace dédié
3. **Configurer les bindings** — les 4 channels Discord redirigés vers `agent:assistant`
4. **Générer les fichiers bootstrap** dans `workspace-assistant/`
5. **Redémarrer le gateway** pour appliquer

---

## Vérifier que ça fonctionne

Envoie un message dans `#assistant` sur ton serveur Discord.
L'agent assistant doit répondre — avec sa personnalité générique pour l'instant.

Si le bot ne répond pas dans `#assistant`, demande à l'agent principal :

```
L'agent assistant ne répond pas dans #assistant. Peux-tu vérifier la configuration des bindings ?
```

---

## Les fichiers de l'agent assistant

Après la création, ces fichiers existent dans `~/openclaw/workspace-assistant/` sur le VPS.
Tu n'as pas à les toucher — l'agent les gère lui-même.
Ils sont là si tu veux les consulter ou si tu as besoin de les inspecter un jour.

| Fichier | Rôle |
|---|---|
| `SOUL.md` | Personnalité, ton, principes |
| `IDENTITY.md` | Nom, emoji |
| `USER.md` | Ton profil utilisateur |
| `MEMORY.md` | Mémoire longue durée |
| `AGENTS.md` | Instructions de comportement |

➡️ **Étape suivante : [Étape 4 — Personnaliser l'agent assistant](./etape-04-personnalisation.md)**
