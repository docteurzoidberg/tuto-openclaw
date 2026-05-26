# Étape 8 — Cas d'usage : notes, rappels et todolists

> [!NOTE]
> On met ici l'agent en pratique avec les trois cas d'usage principaux.
> Chaque section montre comment interagir via Discord et ce qui se passe en coulisses.

---

## Cas d'usage 1 — Prise de notes (`#notes`)

### Usage basique

Dans `#notes`, envoie simplement :

```
Note : réunion avec Marie vendredi 10h, sujet budget Q3
```

L'agent va :
1. Confirmer qu'il a noté
2. Écrire la note dans `memory/YYYY-MM-DD.md`
3. Proposer éventuellement de créer un rappel

### Retrouver une note

```
Retrouve la note sur la réunion avec Marie.
Qu'est-ce que j'ai noté cette semaine ?
Résume mes notes du mois dernier.
```

### Organiser les notes

Tu peux demander à l'agent de créer des fichiers thématiques :

```
Crée un fichier réunions.md et mets-y toutes mes notes de réunion.
```

---

## Cas d'usage 2 — Rappels (`#rappels`)

Les rappels utilisent le système de **cron** d'OpenClaw.
L'agent crée un job qui envoie un message à l'heure souhaitée.

### Créer un rappel

Dans `#rappels` :

```
Rappelle-moi de préparer la présentation demain à 9h.
Rappelle-moi d'appeler le médecin lundi matin.
Dans 2 heures, rappelle-moi de sortir le pain du four.
```

L'agent va créer un cron job qui t'enverra un message Discord au bon moment.

### Rappels récurrents

```
Tous les lundis à 8h30, rappelle-moi de vérifier mes emails importants.
Chaque soir à 19h, demande-moi ce que j'ai accompli aujourd'hui.
```

### Gérer les rappels existants

```
Quels rappels est-ce que j'ai en cours ?
Annule le rappel de lundi matin.
```

---

## Cas d'usage 3 — Todolists (`#todo`)

L'agent maintient une liste de tâches dans un fichier `todos.md` de son workspace.

### Ajouter des tâches

Dans `#todo` :

```
Ajoute à ma todo : acheter des billets de train pour le 15
TODO : finir le rapport avant vendredi
```

### Consulter et cocher

```
Montre-moi ma todo liste.
J'ai fini le rapport, coche-le.
Quelles tâches sont en retard ?
```

### Organisation par priorité

```
Ajoute "appeler le comptable" en priorité haute.
Montre-moi seulement les tâches urgentes.
```

### Format du fichier `todos.md`

L'agent gère un fichier Markdown simple :

```markdown
# Todo List

## 🔴 Urgent
- [ ] Appeler le comptable

## 🟡 Cette semaine
- [ ] Acheter billets de train pour le 15
- [x] ~~Finir le rapport~~ ✓

## 🟢 Quand possible
- [ ] Trier les vieux emails
```

---

## Combiner les cas d'usage

L'agent peut gérer plusieurs choses en une seule phrase :

```
Note que j'ai eu une idée de projet ce matin, mets-la dans ma todo en priorité basse,
et rappelle-moi d'en reparler la semaine prochaine.
```

---

## Récapitulatif

| Channel | Usage | Stockage |
|---|---|---|
| `#notes` | Notes libres, résumés, idées | `memory/YYYY-MM-DD.md` |
| `#rappels` | Rappels one-shot et récurrents | Cron jobs OpenClaw |
| `#todo` | Liste de tâches avec priorités | `todos.md` |
| `#assistant` | Conversation générale | Session isolée |

➡️ **Étape suivante : [Étape 9 — Créer un skill custom](./etape-09-skill-custom.md)**
