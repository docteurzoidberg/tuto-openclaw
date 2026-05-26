# Étape 1 — Concepts : agent principal vs agents spécialisés

> [!NOTE]
> Avant de commencer, comprendre comment OpenClaw organise les agents.
> C'est la base de tout ce qui suit.

---

## L'agent principal

Quand tu as installé OpenClaw, un agent a été créé automatiquement : l'**agent principal**.

C'est lui qui a répondu quand tu as envoyé ton premier DM au bot Discord.
C'est lui qui connaît OpenClaw, qui peut créer d'autres agents, modifier la configuration,
gérer des projets. **C'est ton interlocuteur pour tout ce qui concerne OpenClaw lui-même.**

Il a accès à des outils puissants : modifier sa propre configuration, créer des fichiers,
planifier des tâches, lancer des commandes sur le serveur. Tu n'as pas à faire tout ça
toi-même — tu lui demandes, il s'en occupe.

---

## Les agents spécialisés

OpenClaw permet de créer plusieurs agents, chacun avec :
- Sa propre **personnalité** et son propre **ton**
- Sa propre **mémoire** (fichiers séparés)
- Ses propres **channels Discord** — il ne répond que là où tu l'as assigné

Un agent spécialisé ne voit pas les conversations des autres agents.
Chaque channel Discord peut avoir son propre agent dédié.

---

## Ce qu'on va construire

```
Toi (Discord)
 │
 ├─ DM au bot              → agent principal  (admin, gestion OpenClaw)
 ├─ #assistant             → agent assistant  (conversation générale)
 ├─ #notes                 → agent assistant  (prise de notes)
 ├─ #rappels               → agent assistant  (rappels & tâches planifiées)
 └─ #todo                  → agent assistant  (liste de tâches)
```

L'agent assistant aura sa propre personnalité, sa propre mémoire,
et gérera tes notes, rappels et todos au quotidien.

---

## Comment ça va se passer

**Tu n'auras pas à éditer de fichiers de configuration.**

Tu vas envoyer des messages à l'agent principal — en DM Discord.
Il va poser des questions, générer les fichiers nécessaires, configurer les channels,
et créer l'agent assistant à ta place.

C'est exactement comme ça qu'OpenClaw est fait pour être utilisé.

➡️ **Étape suivante : [Étape 2 — Premier contact avec l'agent principal](./etape-02-premier-contact.md)**
