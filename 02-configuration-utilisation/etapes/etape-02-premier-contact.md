# Étape 2 — Premier contact avec l'agent principal

> [!NOTE]
> L'agent principal est déjà en place depuis la partie 1.
> On va faire connaissance avec lui et lui donner ses premières instructions.

---

## Envoyer le premier message

Dans Discord, envoie un **DM au bot** :

```
Bonjour. Qui es-tu et que peux-tu faire pour moi ?
```

L'agent va se présenter avec sa configuration par défaut.
Il a déjà un nom, une personnalité générique, et accès à tous ses outils.

---

## Le personnaliser

L'agent principal a des fichiers qui définissent son comportement.
Tu peux lui demander de les mettre à jour directement en conversation :

```
Je voudrais te personnaliser. Pose-moi les questions nécessaires pour :
- Savoir comment m'appeler et dans quelle langue me répondre
- Définir ton rôle principal (administration OpenClaw, gestion de mes agents)
- Établir ton ton et ton style de réponse
```

Il va te poser des questions et mettre à jour ses propres fichiers en fonction
de tes réponses. Pas besoin de toucher à quoi que ce soit toi-même.

---

## Ce que l'agent va créer / mettre à jour

Après cet échange, l'agent aura mis à jour ces fichiers dans son workspace :

| Fichier | Contenu |
|---|---|
| `SOUL.md` | Sa personnalité, son ton, ses principes |
| `IDENTITY.md` | Son nom, son emoji |
| `USER.md` | Ton profil, ta langue, tes préférences |
| `MEMORY.md` | Les infos importantes à retenir sur toi |

> [!NOTE]
> Ces fichiers se trouvent dans `~/openclaw/workspace/` sur le VPS.
> Tu n'as pas à les modifier — mais tu peux les consulter ou demander à l'agent
> de les mettre à jour à tout moment, en conversation.

---

## Vérifier que tout est bon

Quelques phrases pour tester :

```
Comment tu t'appelles ?
Qu'est-ce que tu sais de moi ?
Quel est ton rôle ?
```

Si les réponses ne te conviennent pas, demande-lui de corriger :

```
En fait, je préfère que tu me répondes de façon plus concise.
Note que mon fuseau horaire est Europe/Paris.
```

---

## Alternative : CLI sur le VPS

Si Discord n'est pas disponible (bot muet, problème de connexion), tu peux
interagir avec l'agent via SSH sur le VPS :

```bash
docker compose run --rm openclaw-cli chat --agent main
```

Tape tes messages directement dans le terminal. Même agent, mêmes capacités.

---

➡️ **Étape suivante : [Étape 3 — Créer l'agent assistant](./etape-03-creer-agent.md)**
