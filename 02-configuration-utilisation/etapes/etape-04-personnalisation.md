# Étape 4 — Personnaliser l'agent assistant

> [!NOTE]
> L'agent assistant existe mais a une personnalité générique.
> On va le personnaliser via une conversation avec l'agent principal —
> qui va jouer le rôle d'intermédiaire et générer les fichiers.

---

## L'interview de personnalisation

En DM avec l'agent principal, envoie ce prompt :

```
Je veux personnaliser mon agent assistant.
Pose-moi les questions nécessaires pour définir :

1. Son nom et son identité (emoji, style visuel)
2. Sa langue et son ton (formel/informel, avec/sans humour, verbeux/concis)
3. Mon fuseau horaire (pour les rappels et les dates)
4. Ses cas d'usage principaux (notes, rappels, todos — lequel est prioritaire ?)
5. Sa personnalité (des références ? des comportements à éviter absolument ?)
6. Ses règles de confidentialité (sujets qu'il ne doit pas aborder)

Pose les questions une par une. Quand tu as tout, génère les fichiers
SOUL.md, IDENTITY.md, USER.md et MEMORY.md dans le workspace de l'agent assistant,
et affiche un résumé de ce que tu as écrit.
```

---

## Comment ça se passe

L'agent principal va te poser ses questions une par une.
Réponds naturellement — pas besoin de format particulier.

Exemple d'échange :

> **Agent :** Comment veux-tu appeler ton assistant ? Un emoji pour l'identifier ?
>
> **Toi :** JARVIS. Emoji 🧠.
>
> **Agent :** Quelle langue et quel style ? Formel ou informel, avec ou sans humour ?
>
> **Toi :** Français, informel, une touche d'humour mais pas excessif. Réponses courtes.
>
> *[...]*
>
> **Agent :** Parfait. Je génère les fichiers.
> *(Il écrit SOUL.md, IDENTITY.md, USER.md, MEMORY.md dans workspace-assistant/)*
> Voici ce que j'ai créé : [résumé]

---

## Affiner après coup

Tu peux ajuster à tout moment, directement dans `#assistant` :

```
Je préfère que tu sois encore plus concis dans tes réponses.
Note que j'aime qu'on aille droit au but.
Mets à jour ta SOUL.md pour refléter ça.
```

L'agent assistant peut **modifier ses propres fichiers** sur ta demande.

---

## Ce que l'agent principal aura généré

Après l'interview, ces fichiers existent dans `~/openclaw/workspace-assistant/` :

**`SOUL.md`** — personnalité, ton, principes, règles de confidentialité
**`IDENTITY.md`** — nom (ex: JARVIS), emoji, description courte
**`USER.md`** — ton prénom, ta langue, ton fuseau horaire, tes préférences
**`MEMORY.md`** — initialisé avec les infos de base du setup

---

## Tester la personnalisation

Envoie un message dans `#assistant` :

```
Bonjour, qui es-tu ?
```

La réponse doit refléter la personnalité que tu as définie.
Si ce n'est pas le cas, le gateway n'a peut-être pas rechargé les fichiers —
demande à l'agent principal :

```
Peux-tu redémarrer le gateway pour que l'agent assistant charge sa nouvelle personnalité ?
```

➡️ **Étape suivante : [Étape 5 — Notes, rappels et todos](./etape-05-usages.md)**
