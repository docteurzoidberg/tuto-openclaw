# Étape 1 — Préparer le VPS

> [!NOTE]
> Cette étape couvre la mise en place d'une base saine sur ton VPS avant d'installer OpenClaw.
> On suppose que tu as déjà un accès SSH fonctionnel par clé et que Docker + Docker Compose sont installés.

---

## 1. Vérifier l'OS et la version

Connecte-toi en SSH puis lance :

```bash
lsb_release -a
uname -r
docker --version
docker compose version
```

Résultat attendu :
- Ubuntu 22.04/24.04 LTS **ou** Debian 11/12
- Docker 24+ et Docker Compose v2 (`docker compose` sans tiret)

> [!WARNING]
> Si tu as encore `docker-compose` (avec tiret, v1), il faut migrer vers Compose v2.
> Vérifie avec `docker compose version` — si la commande échoue, voir la [doc officielle Docker](https://docs.docker.com/compose/install/).

---

## 2. Créer un utilisateur dédié

Ne fais pas tourner OpenClaw sous root. On crée un user `openclaw` :

```bash
sudo adduser openclaw
sudo usermod -aG sudo openclaw
```

Ajoute-le aussi au groupe `docker` pour qu'il puisse lancer des conteneurs sans `sudo` :

```bash
sudo usermod -aG docker openclaw
```

> [!NOTE]
> L'appartenance au groupe `docker` ne prend effet qu'à la prochaine connexion.
> Déconnecte-toi et reconnecte-toi en tant que `openclaw` pour la suite.

Bascule sur ce nouvel utilisateur :

```bash
su - openclaw
# ou reconnecte-toi directement en SSH avec : ssh openclaw@<IP_DU_VPS>
```

Vérifie que docker est accessible :

```bash
docker ps
```

---

## 3. Hardening SSH

> [!WARNING]
> Avant de modifier la config SSH, assure-toi d'avoir **une session SSH ouverte** en parallèle.
> Si tu te trompes et que tu te lock out, tu auras besoin de la console OVH pour récupérer l'accès.

### Désactiver l'authentification par mot de passe

```bash
sudo nano /etc/ssh/sshd_config
```

Trouve et modifie ces lignes (ou ajoute-les si absentes) :

```
PasswordAuthentication no
PermitRootLogin no
```

Sauvegarde puis redémarre SSH :

```bash
sudo systemctl restart sshd
```

### Vérifier que ta clé SSH fonctionne toujours

**Depuis un autre terminal**, tente une connexion :

```bash
ssh openclaw@<IP_DU_VPS>
```

Si ça passe → tu peux fermer l'ancienne session. Si ça bloque → tu as un problème avec ta clé, **ne ferme pas la session en cours** et corrige avant.

---

## 4. Firewall UFW

### Vérifier si UFW est installé

```bash
sudo ufw status
```

Si la commande n'existe pas :

```bash
sudo apt install ufw -y
```

### Configurer les règles

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow ssh
```

> [!NOTE]
> OVH propose aussi un firewall managé dans son interface (Network Security). Les deux peuvent coexister.
> UFW gère le firewall local sur le VPS, le firewall OVH agit en amont au niveau réseau.

Active UFW :

```bash
sudo ufw enable
sudo ufw status verbose
```

Résultat attendu :

```
Status: active

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    Anywhere
```

> [!IMPORTANT]
> Le port **18789** (gateway OpenClaw) doit rester **fermé** dans UFW.
> L'accès à l'interface OpenClaw se fera uniquement via SSH tunnel — jamais directement exposé sur Internet.

---

## 5. Préparer la structure du projet

Toujours en tant qu'utilisateur `openclaw`, crée la structure de dossiers qui accueillera OpenClaw :

```bash
mkdir -p ~/openclaw/{workspace,config,data}
cd ~/openclaw
```

Structure finale :

```
~/openclaw/
├── docker-compose.yml   ← à créer à l'étape suivante
├── .env                 ← variables d'environnement (secrets, tokens)
├── workspace/           ← workspace de l'agent (fichiers, mémoire)
├── config/              ← config OpenClaw persistante
└── data/                ← état interne du gateway
```

> [!WARNING]
> Le fichier `.env` contiendra des secrets (token Discord, clé API IA).
> Restreins ses permissions dès maintenant :
> ```bash
> touch ~/openclaw/.env
> chmod 600 ~/openclaw/.env
> ```

---

## Récapitulatif

À ce stade ton VPS doit être dans cet état :

- [x] OS vérifié (Ubuntu 22.04/24.04 ou Debian 11/12)
- [x] Docker + Docker Compose v2 fonctionnels
- [x] User `openclaw` créé, dans les groupes `sudo` et `docker`
- [x] SSH : login root et auth par mot de passe désactivés
- [x] UFW actif : SSH autorisé, tout le reste bloqué
- [x] Structure de dossiers `~/openclaw/` en place, `.env` avec permissions 600

➡️ **Étape suivante : [Étape 2 — Installation OpenClaw via Docker Compose](./etape-02-installation.md)**
