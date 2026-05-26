# Étape 3 — Créer le bot Discord

> [!NOTE]
> Cette étape se passe entièrement dans le **Discord Developer Portal** et l'app Discord.
> Rien à faire sur le VPS pour l'instant.

À la fin de cette étape tu auras :
- Un bot Discord créé et configuré
- Son **token** (à garder secret)
- Ton **Server ID** et ton **User ID**

---

## 1. Créer un serveur Discord dédié

> [!NOTE]
> Si tu as déjà un serveur privé pour toi seul, tu peux l'utiliser. Sinon, crées-en un.

Dans Discord :
1. Clique sur le **+** dans la barre latérale gauche → **Créer un serveur**
2. Choisis **Pour moi et mes amis**
3. Nomme-le comme tu veux (ex. `Mon OpenClaw`)

---

## 2. Créer une application Discord

1. Va sur [discord.com/developers/applications](https://discord.com/developers/applications)
2. Clique **New Application**
3. Donne-lui un nom (ex. `OpenClaw`) → **Create**

---

## 3. Configurer le bot

Dans la sidebar, clique sur **Bot**.

### Changer le nom d'utilisateur du bot

Modifie le **Username** si tu veux un nom différent du nom de l'application.

### Activer les intents privilégiés

Scroll jusqu'à **Privileged Gateway Intents** et active :

- ✅ **Server Members Intent** — nécessaire pour les allowlists et la résolution des noms
- ✅ **Message Content Intent** — **obligatoire** pour qu'OpenClaw puisse lire les messages

Clique **Save Changes**.

### Récupérer le token

Scroll en haut de la page **Bot**, clique **Reset Token**.

> [!WARNING]
> Malgré son nom, ce bouton **génère** ton premier token — il ne réinitialise rien.
> Copie ce token et garde-le en lieu sûr. Tu ne pourras plus le voir après avoir quitté la page.
> Si tu le perds, tu devras en générer un nouveau.

---

## 4. Inviter le bot sur ton serveur

Dans la sidebar, clique sur **OAuth2**.

### Générer l'URL d'invitation

Scroll jusqu'à **OAuth2 URL Generator** et coche :

**Scopes :**
- ✅ `bot`
- ✅ `applications.commands`

**Bot Permissions** (section qui apparaît en dessous) :

| Catégorie | Permission | Obligatoire |
|---|---|---|
| General | View Channels | ✅ |
| Text | Send Messages | ✅ |
| Text | Read Message History | ✅ |
| Text | Embed Links | ✅ |
| Text | Attach Files | ✅ |
| Text | Add Reactions | optionnel |
| Text | Send Messages in Threads | optionnel |

Copie l'**URL générée** en bas de page, ouvre-la dans ton navigateur, sélectionne ton serveur → **Continuer** → **Autoriser**.

Ton bot apparaît maintenant dans la liste des membres de ton serveur.

---

## 5. Activer le mode développeur et récupérer les IDs

Le mode développeur permet de copier les IDs internes Discord.

Dans Discord :
1. **Paramètres utilisateur** (icône engrenage) → **Avancé** → active **Mode développeur**

### Copier le Server ID

Clic droit sur l'icône de ton serveur dans la barre latérale → **Copier l'identifiant du serveur**

### Copier ton User ID

Clic droit sur **ton propre avatar** (dans n'importe quel message ou dans la liste des membres) → **Copier l'identifiant de l'utilisateur**

---

## 6. Autoriser les DMs depuis le serveur

Pour que le bot puisse t'envoyer des messages privés (nécessaire pour le pairing) :

Clic droit sur l'icône de ton serveur → **Paramètres de confidentialité** → active **Messages directs**

---

## Récapitulatif

Note ces trois valeurs, tu en auras besoin à l'étape suivante :

```
Bot Token   : MTxxxxxxxxxx.xxxxxx.xxxxxxxxxxxxxxxxxxxxxxxxxxxx
Server ID   : 000000000000000000
User ID     : 000000000000000000
```

> [!WARNING]
> Le token bot est un **secret**. Ne le partage jamais, ne le commite jamais dans un repo.
> On le stockera dans le fichier `.env` du VPS à l'étape suivante.

À ce stade :

- [x] Application Discord créée
- [x] Bot configuré avec les intents `Server Members` et `Message Content`
- [x] Token bot copié et mis en lieu sûr
- [x] Bot invité sur ton serveur
- [x] Mode développeur activé
- [x] Server ID et User ID récupérés
- [x] DMs autorisés depuis le serveur

➡️ **Étape suivante : [Étape 4 — Connecter Discord à OpenClaw](./etape-04-config-discord.md)**
