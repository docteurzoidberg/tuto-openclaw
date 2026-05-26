# Étape 5 — Prompt de bootstrap : laisser l'agent principal interviewer l'utilisateur

> [!NOTE]
> Plutôt que de remplir manuellement les fichiers de l'agent assistant,
> on va demander à l'agent principal de **poser les bonnes questions** et
> de **générer les fichiers** à notre place.

---

## Le principe

L'agent principal connaît OpenClaw. On lui donne un prompt structuré qui lui dit :
1. Pose des questions à l'utilisateur sur son usage souhaité
2. Génère les fichiers `SOUL.md`, `IDENTITY.md`, `USER.md`, `MEMORY.md` pour l'agent assistant
3. Crée les fichiers dans le bon workspace

C'est une façon élégante d'utiliser l'IA pour configurer l'IA.

---

## Le prompt de bootstrap

Envoie ce prompt à l'agent principal **en DM Discord** (ou via la session principale) :

```
Je veux configurer mon agent assistant personnel.
Il sera utilisé via Discord (channels #assistant, #notes, #rappels, #todo).

Pour bien le configurer, j'ai besoin que tu me poses des questions dans cet ordre :

1. **Identité** : Quel nom veux-je donner à mon assistant ? Quel emoji/avatar ?
2. **Langue et ton** : Quelle langue principale ? Quel style (formel/informel, sobre/expansif, avec humour/sans) ?
3. **Fuseau horaire** : Pour bien gérer les rappels et les dates.
4. **Cas d'usage principaux** : Notes, rappels, todos — lequel est le plus important ? D'autres usages ?
5. **Personnalité** : Des références ou modèles d'assistants appréciés ? Des comportements à éviter absolument ?
6. **Règles de confidentialité** : Y a-t-il des sujets que l'assistant ne doit jamais aborder ?

Pose les questions une par une (ou par groupe logique), attends mes réponses,
puis génère les fichiers suivants dans ~/openclaw/workspace-assistant/ :
- SOUL.md
- IDENTITY.md
- USER.md
- MEMORY.md (avec juste les infos de base du setup)

À la fin, affiche un résumé de ce que tu as écrit.
```

---

## Exemple d'échange attendu

**Agent :** Commençons. Comment veux-tu appeler ton assistant ? Et quel emoji lui associer ?

**Toi :** Je l'appellerai "JARVIS". Emoji 🧠.

**Agent :** Super. Quelle langue principale pour les réponses, et quel style préfères-tu — formel ou informel ? Sobre ou avec un peu d'humour ?

**Toi :** Français, informel, un peu d'humour mais pas excessif.

**Agent :** Quel est ton fuseau horaire ?

**Toi :** Europe/Paris.

*[...les questions continuent...]*

**Agent :** Parfait, je génère les fichiers maintenant.
*(il écrit SOUL.md, IDENTITY.md, USER.md, MEMORY.md dans workspace-assistant/)*

Voici un résumé de ce qui a été créé : [...]

---

## Après l'interview

Une fois les fichiers générés, vérifie leur contenu :

```bash
cat ~/openclaw/workspace-assistant/SOUL.md
cat ~/openclaw/workspace-assistant/IDENTITY.md
cat ~/openclaw/workspace-assistant/USER.md
```

Relis-les et corrige ce qui ne te convient pas directement dans les fichiers — ce sont de simples fichiers Markdown éditables.

Redémarre le gateway pour que l'agent assistant charge ses nouveaux fichiers :

```bash
cd ~/openclaw && docker compose restart openclaw-gateway
```

Teste en envoyant un message dans `#assistant` — la personnalité devrait déjà être perceptible.

---

## Affiner plus tard

Ces fichiers ne sont pas figés. Tu peux les modifier à tout moment :
- Directement avec `nano` sur le VPS
- En demandant à l'agent lui-même de les mettre à jour via la conversation
- En redemandant à l'agent principal de régénérer un fichier spécifique

> [!NOTE]
> L'agent assistant peut aussi **modifier ses propres fichiers** si tu lui demandes.
> Par exemple : "Mets à jour ta SOUL.md pour noter que je préfère des réponses courtes."

---

## Récapitulatif

- [x] Prompt d'interview envoyé à l'agent principal
- [x] Questions posées et réponses fournies
- [x] Fichiers `SOUL.md`, `IDENTITY.md`, `USER.md`, `MEMORY.md` générés dans `workspace-assistant/`
- [x] Contenu relu et ajusté si nécessaire
- [x] Gateway redémarré
- [x] Premier test dans `#assistant` concluant

➡️ **Étape suivante : [Étape 6 — Personnaliser la SOUL en détail](./etape-06-soul.md)**
