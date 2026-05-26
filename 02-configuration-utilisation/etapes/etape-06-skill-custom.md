# Étape 6 — Créer un skill custom

> [!NOTE]
> Les skills donnent à l'agent des instructions précises pour certaines tâches.
> On va demander à l'agent assistant de créer son propre skill de gestion de todos.

---

## Qu'est-ce qu'un skill ?

Un skill est un fichier d'instructions spécialisées que l'agent lit avant d'effectuer
certaines tâches. Sans skill, l'agent improvise. Avec un skill, il suit toujours
les mêmes règles — format cohérent, comportement prévisible.

Les skills se trouvent dans `skills/` à l'intérieur du workspace de l'agent.
L'agent peut les créer, les modifier, et les consulter lui-même.

---

## Demander la création du skill

Dans `#assistant`, envoie ce message :

```
Je veux que tu gères ma todo liste de façon cohérente et prévisible.
Crée un skill "todolist" dans ton workspace avec les règles suivantes :

- La todo est stockée dans un fichier todos.md à la racine de ton workspace
- Structure en 4 sections : Urgent, Cette semaine, Quand possible, Terminées
- Format des tâches : cases à cocher Markdown (- [ ] et - [x])
- Quand une tâche est cochée, la déplacer en section Terminées avec la date
- Si aucune priorité précisée → "Cette semaine" par défaut
- Toujours confirmer avant de supprimer une tâche non cochée
- Afficher d'abord les urgentes quand on demande la liste

Confirme-moi quand le skill est créé.
```

L'agent va créer le fichier `skills/todolist/SKILL.md` dans son workspace.

---

## Ce que l'agent va générer

Après cette demande, ce fichier existe dans `~/openclaw/workspace-assistant/skills/todolist/SKILL.md`.

C'est un fichier Markdown que l'agent lit automatiquement dès qu'il détecte
une tâche liée à la gestion de todos. Tu peux demander à voir son contenu :

```
Montre-moi le skill todolist que tu viens de créer.
```

---

## Tester le skill

```
Ajoute "préparer la démo" en urgence.
Montre-moi ma todo liste.
J'ai fini "préparer la démo".
```

Le comportement doit maintenant être strictement conforme aux règles du skill.

---

## Aller plus loin

Le même principe s'applique à d'autres comportements :

```
Crée un skill pour la prise de notes : chaque note doit avoir une date,
un titre court, et être classée par thème dans un fichier notes.md.
```

```
Crée un skill pour les rappels : quand je mentionne une date ou une heure,
propose-moi systématiquement de créer un rappel.
```

---

## Modifier ou supprimer un skill

```
Mets à jour le skill todolist : ajoute une section "En cours" entre Urgent et Cette semaine.
Supprime le skill todolist.
```

L'agent modifie ou supprime le fichier directement.

➡️ **Étape suivante : [Étape 7 — Maintenance et debug](./etape-07-maintenance.md)**
