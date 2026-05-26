# Étape 9 — Créer un skill custom

> [!NOTE]
> Les skills sont des fichiers d'instructions spécialisées que l'agent lit
> avant d'effectuer certaines tâches. On va créer un skill dédié à la gestion
> de la todolist — pour que l'agent sache exactement comment s'en occuper.

---

## Qu'est-ce qu'un skill ?

Un skill est un fichier `SKILL.md` placé dans un dossier du workspace.
Quand l'agent détecte qu'une tâche correspond à un skill, il le lit et suit ses instructions.

**Sans skill :** l'agent improvise comment gérer la todo.
**Avec skill :** l'agent suit des règles précises et cohérentes à chaque fois.

---

## Structure d'un skill

```
workspace-assistant/
└── skills/
    └── todolist/
        └── SKILL.md
```

Chaque skill a :
- Un nom
- Une description (utilisée pour détecter quand l'appliquer)
- Des instructions précises

---

## Créer le skill `todolist`

```bash
mkdir -p ~/openclaw/workspace-assistant/skills/todolist
nano ~/openclaw/workspace-assistant/skills/todolist/SKILL.md
```

```markdown
# Skill : Todolist

## Quand utiliser ce skill

Utilise ce skill pour toute opération sur la liste de tâches :
- Ajouter une tâche
- Cocher / décocher une tâche
- Afficher la liste
- Changer la priorité d'une tâche
- Supprimer une tâche

## Fichier cible

La todolist est dans : `todos.md` (à la racine du workspace).
Si le fichier n'existe pas, le créer avec la structure définie ci-dessous.

## Structure du fichier `todos.md`

```markdown
# Todo List

## 🔴 Urgent
<!-- tâches urgentes ici -->

## 🟡 Cette semaine
<!-- tâches pour la semaine ici -->

## 🟢 Quand possible
<!-- tâches sans deadline ici -->

## ✅ Terminées (derniers 7 jours)
<!-- tâches cochées récemment, nettoyer après 7 jours -->
```

## Règles

1. **Ajouter une tâche** : placer dans la bonne section selon la priorité indiquée.
   Si pas de priorité précisée → section "Cette semaine" par défaut.

2. **Format d'une tâche** :
   - Non faite : `- [ ] Description de la tâche`
   - Faite : `- [x] ~~Description~~ ✓ (YYYY-MM-DD)`

3. **Cocher une tâche** : déplacer vers la section "Terminées" avec la date.

4. **Afficher la liste** : lire `todos.md` et présenter de façon lisible.
   Toujours afficher d'abord les urgentes.

5. **Nettoyer** : les tâches terminées de plus de 7 jours peuvent être supprimées
   si l'utilisateur demande un nettoyage.

6. **Confirmer avant suppression** : toujours demander confirmation avant de
   supprimer une tâche non cochée.

## Réponses types

- Ajout : "✅ Ajouté : [tâche] (priorité [niveau])"
- Cochée : "☑️ [tâche] marquée comme terminée."
- Liste vide : "Ta todo est vide. 🎉"
```

---

## Tester le skill

Redémarre le gateway :

```bash
cd ~/openclaw && docker compose restart openclaw-gateway
```

Dans `#todo` sur Discord, teste :

```
Ajoute "préparer la démo" en priorité urgente.
Montre-moi ma todo liste.
J'ai fini "préparer la démo".
```

L'agent doit maintenant suivre exactement les règles du skill.

---

## Aller plus loin : d'autres skills

Le même principe s'applique à n'importe quel comportement répétitif :

| Skill | Utilité |
|---|---|
| `rappels/SKILL.md` | Règles pour créer/gérer les cron jobs de rappel |
| `notes/SKILL.md` | Format standard pour les notes, nommage des fichiers |
| `resume-journalier/SKILL.md` | Structure du résumé quotidien |

---

## Récapitulatif

- [x] Concept de skill compris
- [x] Dossier `skills/todolist/` créé dans `workspace-assistant/`
- [x] `SKILL.md` rédigé avec règles précises
- [x] Gateway redémarré
- [x] Skill testé en conversation Discord

➡️ **Étape suivante : [Étape 10 — Debug et administration via CLI SSH](./etape-10-cli-ssh.md)**
