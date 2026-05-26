# Étape 4 — Connecter Discord à OpenClaw

> [!NOTE]
> On va configurer OpenClaw pour qu'il utilise ton bot Discord,
> puis effectuer le **pairing** pour autoriser ton compte à lui parler.

Tu auras besoin des trois valeurs récupérées à l'étape précédente :
- Bot Token
- Server ID
- User ID

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

Le pairing autorise ton compte Discord à interagir avec l'agent.

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

## 5. Tester

Dans Discord, envoie un message à ton bot en DM :

```
Bonjour, tu m'entends ?
```

Il doit répondre. Si c'est le cas, le setup Discord est opérationnel.

Tu peux aussi tester dans un channel de ton serveur — le bot répondra sans avoir besoin d'être @mentionné (grâce au `requireMention: false` configuré plus haut).

---

## Récapitulatif

- [x] Token Discord ajouté dans `.env`
- [x] Configuration Discord appliquée via `config patch`
- [x] `groupPolicy: allowlist` avec ton User ID
- [x] Gateway redémarré, bot connecté
- [x] Pairing approuvé
- [x] Bot répond en DM et sur le serveur

➡️ **Étape suivante : [Étape 5 — Accéder à l'UI OpenClaw via SSH tunnel](./etape-05-ssh-tunnel.md)**
