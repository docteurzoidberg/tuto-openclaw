# Étape 4 — Connecter Discord à OpenClaw

> [!NOTE]
> On va configurer OpenClaw pour qu'il utilise ton bot Discord,
> puis effectuer le **pairing** pour autoriser ton compte à lui parler.

Tu auras besoin des trois valeurs récupérées à l'étape précédente :
- Bot Token
- Server ID
- User ID

---

## Comment fonctionne la communication Discord avec OpenClaw

OpenClaw supporte **deux modes** de communication Discord, indépendants :

| Mode | Description | Config requise |
|---|---|---|
| **DM (message privé)** | Conversation privée entre toi et le bot | Pairing uniquement |
| **Channel de serveur** | Le bot répond dans un channel de ton serveur | Config guild + pairing |

Les deux modes sont actifs en parallèle une fois configurés.

---

## 1. Ajouter le token Discord dans `.env`

Sur le VPS, édite le fichier `.env` :

```bash
nano ~/openclaw/.env
```

Décommente et remplis la ligne Discord :

```env
DISCORD_BOT_TOKEN=MTxxxxxxxxxx.xxxxxx.xxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

Sauvegarde (`Ctrl+O` puis `Ctrl+X`).

> [!WARNING]
> Le token ne doit jamais apparaître en clair dans un fichier versionné.
> Le `.env` est dans `~/openclaw/` — hors du repo Git du tuto, c'est bien.

---

## 2. Appliquer la configuration Discord

Crée un fichier de patch de configuration :

```bash
cat > /tmp/discord.patch.json5 <<'JSON5'
{
  channels: {
    discord: {
      enabled: true,
      token: { source: "env", provider: "default", id: "DISCORD_BOT_TOKEN" },
      groupPolicy: "allowlist",
      guilds: {
        "TON_SERVER_ID": {
          requireMention: false,
          users: ["TON_USER_ID"],
        },
      },
    },
  },
}
JSON5
```

**Remplace** `TON_SERVER_ID` et `TON_USER_ID` par tes vrais IDs.

> [!NOTE]
> **`groupPolicy: "allowlist"`** — le bot ne répondra que sur les serveurs explicitement listés dans `guilds`.
> Les autres serveurs où tu l'aurais invité par erreur sont ignorés. C'est la config la plus sûre.
>
> **`requireMention: false`** — dans un serveur privé pour toi seul, le bot répond à tous tes messages
> sans avoir besoin d'être @mentionné. Si tu partages le serveur avec d'autres personnes,
> passe ce paramètre à `true`.
>
> **`users`** — seuls les User IDs listés ici peuvent interagir avec le bot sur ce serveur.

Test à sec d'abord :

```bash
docker compose run --rm openclaw-cli config patch --file /tmp/discord.patch.json5 --dry-run
```

Si le dry-run est OK, applique :

```bash
docker compose run --rm openclaw-cli config patch --file /tmp/discord.patch.json5
```

---

## 3. Redémarrer le gateway

```bash
cd ~/openclaw
docker compose restart openclaw-gateway
```

Vérifie que le bot se connecte dans les logs :

```bash
docker compose logs -f openclaw-gateway
```

Tu dois voir quelque chose comme :

```
Discord gateway connected
Logged in as OpenClaw#1234
```

---

## 4. Pairing — première connexion

Le pairing est une autorisation unique qui lie ton compte Discord à OpenClaw.
**Un seul pairing suffit** pour débloquer à la fois les DMs et les channels du serveur.

1. Dans Discord, **envoie un message privé (DM) à ton bot**
   → Il te répond avec un code de pairing (format `XXXX-XXXX`)

2. Sur le VPS, liste les pairings en attente et approuve :

```bash
docker compose run --rm openclaw-cli pairing list discord
docker compose run --rm openclaw-cli pairing approve discord XXXX-XXXX
```

> [!NOTE]
> Les codes de pairing expirent après **1 heure**.
> Si le tien a expiré, renvoie simplement un DM au bot pour en générer un nouveau.

---

## 5. Tester les DMs

Dans Discord, envoie un message privé à ton bot :

```
Bonjour, tu m'entends ?
```

Il doit répondre. Les DMs ont leur propre session dédiée — la conversation est privée et persistante.

---

## 6. Tester sur un channel du serveur

Crée un channel texte sur ton serveur (ou utilise `#général`), et envoie un message.

Le bot répond directement, **sans @mention** nécessaire.

> [!NOTE]
> Chaque channel Discord a sa propre **session isolée** dans OpenClaw.
> Le contexte de `#coding` est séparé de `#général`, etc.
> C'est utile pour organiser les conversations par thème.

---

## 7. Ajouter un nouveau channel plus tard

Quand tu crées un nouveau channel sur ton serveur Discord, **rien à reconfigurer** dans OpenClaw.

Tant que :
- Le channel est sur le serveur dont le Server ID est dans `guilds`
- Le bot a accès au channel (permissions Discord standard)

→ Le bot y répond automatiquement.

### Restreindre le bot à certains channels uniquement

Si tu veux que le bot réponde **uniquement dans des channels spécifiques** et ignore les autres :

```bash
cat > /tmp/discord-channels.patch.json5 <<'JSON5'
{
  channels: {
    discord: {
      guilds: {
        "TON_SERVER_ID": {
          channels: ["ID_CHANNEL_1", "ID_CHANNEL_2"],
        },
      },
    },
  },
}
JSON5
docker compose run --rm openclaw-cli config patch --file /tmp/discord-channels.patch.json5
docker compose restart openclaw-gateway
```

> [!NOTE]
> Pour récupérer l'ID d'un channel : clic droit sur le channel dans Discord
> → **Copier l'identifiant du channel** (mode développeur requis, activé à l'étape 3).

---

## Récapitulatif

- [x] Token Discord ajouté dans `.env`
- [x] Configuration Discord appliquée via `config patch`
- [x] `groupPolicy: allowlist` — serveur privé uniquement
- [x] `requireMention: false` — pas besoin de @mentionner le bot
- [x] `users` allowlist — toi seul
- [x] Gateway redémarré, bot connecté
- [x] Pairing approuvé
- [x] Bot répond en DM ✓
- [x] Bot répond dans les channels du serveur ✓
- [x] Nouveaux channels : automatiquement pris en charge

➡️ **Étape suivante : [Étape 5 — Accéder à l'UI OpenClaw via SSH tunnel](./etape-05-ssh-tunnel.md)**
