# Étape 4 — Attacher l'agent assistant aux channels Discord

> [!NOTE]
> On configure les **bindings** : chaque channel Discord sera redirigé vers le bon agent.
> L'agent principal garde les DMs, l'agent assistant reçoit les channels dédiés.

---

## 1. Créer les channels Discord

Dans ton serveur Discord, crée ces 4 channels texte :

| Channel | Usage |
|---|---|
| `#assistant` | Conversation générale avec l'assistant |
| `#notes` | Prise de notes rapides |
| `#rappels` | Rappels et tâches planifiées |
| `#todo` | Gestion de liste de tâches |

Pour chaque channel, récupère son **Channel ID** :
- Clic droit sur le channel → **Copier l'identifiant du channel** (mode développeur requis)

---

## 2. Configurer les bindings dans OpenClaw

Les bindings indiquent à OpenClaw : "ce channel → cet agent".

```bash
cat > /tmp/bindings.patch.json5 <<'JSON5'
{
  bindings: [
    {
      agentId: "assistant",
      match: {
        channel: "discord",
        peer: { kind: "channel", id: "ID_CHANNEL_ASSISTANT" },
      },
    },
    {
      agentId: "assistant",
      match: {
        channel: "discord",
        peer: { kind: "channel", id: "ID_CHANNEL_NOTES" },
      },
    },
    {
      agentId: "assistant",
      match: {
        channel: "discord",
        peer: { kind: "channel", id: "ID_CHANNEL_RAPPELS" },
      },
    },
    {
      agentId: "assistant",
      match: {
        channel: "discord",
        peer: { kind: "channel", id: "ID_CHANNEL_TODO" },
      },
    },
  ],
}
JSON5
```

**Remplace** les 4 `ID_CHANNEL_*` par les vrais IDs copiés depuis Discord.

Dry-run puis application :

```bash
docker compose run --rm openclaw-cli config patch --file /tmp/bindings.patch.json5 --dry-run
docker compose run --rm openclaw-cli config patch --file /tmp/bindings.patch.json5
```

---

## 3. Redémarrer le gateway

```bash
cd ~/openclaw && docker compose restart openclaw-gateway
```

---

## 4. Vérifier les bindings

```bash
docker compose run --rm openclaw-cli config get bindings
```

---

## 5. Tester

- Envoie un message dans `#assistant` → doit répondre l'agent **assistant**
- Envoie un DM au bot → doit répondre l'agent **principal**

> [!NOTE]
> Pour l'instant l'agent assistant a une personnalité générique (fichiers bootstrap par défaut).
> On va la personnaliser aux étapes suivantes.

---

## Ordre de priorité des bindings

Si plusieurs bindings pourraient correspondre, OpenClaw applique cet ordre de priorité :

1. Match exact par `peer.id` (channel ID) ← **c'est ce qu'on utilise ici**
2. Match par `guildId` (serveur entier)
3. Match par `accountId`
4. Agent par défaut (`default: true` dans `agents.list`)

Nos bindings par channel ID sont donc les plus précis — seuls ces channels vont vers l'assistant.

---

## Récapitulatif

- [x] Channels `#assistant`, `#notes`, `#rappels`, `#todo` créés sur Discord
- [x] Channel IDs récupérés
- [x] Bindings configurés — 4 channels → `agent:assistant`
- [x] DMs → `agent:main` (par défaut, rien à configurer)
- [x] Tests OK : bonne répartition des messages

➡️ **Étape suivante : [Étape 5 — Prompt de bootstrap : interview utilisateur](./etape-05-bootstrap-interview.md)**
