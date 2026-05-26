# Étape 5 — Notes, rappels et todos

> [!NOTE]
> L'agent assistant est maintenant opérationnel et personnalisé.
> Voici comment l'utiliser au quotidien — parle-lui naturellement.

---

## `#notes` — Prise de notes

Envoie tes notes comme tu les penserais à voix haute :

```
Note : réunion avec Marie vendredi 10h, sujet budget Q3
```
```
Idée de projet : application de suivi de lecture, noter ça quelque part
```
```
Résumé de ma journée : finalement réglé le bug de prod, déployé en 16h
```

L'agent confirme, enregistre, et peut retrouver tes notes plus tard :

```
Qu'est-ce que j'ai noté cette semaine ?
Retrouve la note sur la réunion avec Marie.
Résume mes notes du mois dernier.
```

**En coulisses :** l'agent écrit dans `memory/YYYY-MM-DD.md` dans son workspace.

---

## `#rappels` — Rappels et tâches planifiées

L'agent crée des rappels qui t'enverront un message Discord au bon moment.
Parle-lui naturellement :

```
Rappelle-moi de préparer la présentation demain à 9h.
Dans 2 heures, rappelle-moi de sortir le pain du four.
Rappelle-moi d'appeler le médecin lundi matin.
Tous les lundis à 8h30, rappelle-moi de vérifier mes emails importants.
```

Pour gérer tes rappels existants :

```
Quels rappels est-ce que j'ai en cours ?
Annule le rappel de lundi matin.
```

**En coulisses :** l'agent crée des **cron jobs** dans OpenClaw qui envoient
un message dans ce channel à l'heure programmée. Tu n'as rien à configurer.

---

## `#todo` — Liste de tâches

```
Ajoute à ma todo : acheter des billets de train pour le 15
TODO urgent : appeler le comptable avant vendredi
```

Consulter et gérer la liste :

```
Montre-moi ma todo liste.
J'ai fini le rapport, coche-le.
Quelles tâches sont en retard ?
Ajoute "trier les emails" en priorité basse.
```

**En coulisses :** l'agent maintient un fichier `todos.md` dans son workspace,
avec des sections par priorité (urgent / cette semaine / quand possible / terminées).

---

## `#assistant` — Conversation générale

Pour tout le reste — questions, aide, réflexion, résumés :

```
Qu'est-ce qu'on a fait ensemble cette semaine ?
Aide-moi à rédiger un email pour reporter cette réunion.
Quels sont mes rappels et todos en cours ?
```

---

## Combiner les usages

L'agent comprend les phrases naturelles qui mélangent plusieurs intentions :

```
Note que j'ai eu une idée ce matin — appli de suivi de lecture.
Ajoute-la à ma todo en priorité basse et rappelle-moi d'en reparler la semaine prochaine.
```

Il va noter l'idée, l'ajouter à la todo, et créer le rappel — en une seule réponse.

---

## Ce que l'agent retient entre les sessions

Entre chaque conversation, l'agent repart de zéro — mais ses fichiers persistent.
Il relit `MEMORY.md` et les notes récentes à chaque session.

Pour lui apprendre quelque chose de durable :

```
Souviens-toi que ma réunion hebdo est le lundi à 14h.
Note dans ta mémoire que j'utilise Python pour mes projets perso.
```

➡️ **Étape suivante : [Étape 6 — Créer un skill custom](./etape-06-skill-custom.md)**
