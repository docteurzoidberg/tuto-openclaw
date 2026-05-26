# Étape 3 — Créer l'agent assistant

> [!NOTE]
> On crée ici un second agent dédié à l'assistance personnelle.
> Il aura son propre workspace, sa propre personnalité, et ses propres channels Discord.

---

## 1. Créer le workspace de l'agent assistant

```bash
mkdir -p ~/openclaw/workspace-assistant
```

Ce dossier est le "cerveau" de l'agent assistant — ses fichiers de personnalité
et sa mémoire y vivront séparément de l'agent principal.

---

## 2. Déclarer l'agent dans la configuration OpenClaw

```bash
cat > /tmp/assistant-agent.patch.json5 <<'JSON5'
{
  agents: {
    list: [
      {
        id: "main",
        default: true,
        name: "Agent Principal",
        workspace: "/home/openclaw/openclaw/workspace",
      },
      {
        id: "assistant",
        name: "Assistant",
        workspace: "/home/openclaw/openclaw/workspace-assistant",
      },
    ],
  },
}
JSON5
```

> [!IMPORTANT]
> Les chemins doivent correspondre aux chemins **à l'intérieur du conteneur Docker**.
> Le workspace monté dans le conteneur est `/home/openclaw/openclaw/workspace` —
> vérifie dans ton `.env` la valeur de `OPENCLAW_WORKSPACE_DIR`.

Dry-run puis application :

```bash
docker compose run --rm openclaw-cli config patch --file /tmp/assistant-agent.patch.json5 --dry-run
docker compose run --rm openclaw-cli config patch --file /tmp/assistant-agent.patch.json5
```

---

## 3. Initialiser les fichiers bootstrap de l'agent assistant

OpenClaw peut générer les fichiers de base automatiquement :

```bash
docker compose run --rm openclaw-cli setup --agent assistant
```

Vérifie que les fichiers sont bien créés :

```bash
ls ~/openclaw/workspace-assistant/
# AGENTS.md  SOUL.md  IDENTITY.md  USER.md  TOOLS.md  HEARTBEAT.md  MEMORY.md
```

---

## 4. Monter le workspace assistant dans Docker

Le nouveau workspace doit être accessible dans le conteneur. Édite ton `.env` :

```bash
nano ~/openclaw/.env
```

Ajoute le mount supplémentaire :

```env
OPENCLAW_EXTRA_MOUNTS=/home/openclaw/openclaw/workspace-assistant:/home/openclaw/openclaw/workspace-assistant:rw
```

> [!NOTE]
> `OPENCLAW_EXTRA_MOUNTS` accepte plusieurs mounts séparés par des virgules.

Recrée le fichier Compose avec le mount supplémentaire et redémarre :

```bash
cd ~/openclaw
./setup.sh  # régénère docker-compose.extra.yml avec les nouveaux mounts
docker compose -f docker-compose.yml -f docker-compose.extra.yml up -d
```

> [!NOTE]
> Une fois `OPENCLAW_EXTRA_MOUNTS` défini, utilise toujours les deux fichiers Compose :
> ```bash
> docker compose -f docker-compose.yml -f docker-compose.extra.yml <commande>
> ```
> Tu peux créer un alias dans `~/.bashrc` pour simplifier :
> ```bash
> alias oc="docker compose -f ~/openclaw/docker-compose.yml -f ~/openclaw/docker-compose.extra.yml"
> ```
> Puis utiliser `oc up -d`, `oc restart`, `oc logs -f openclaw-gateway`, etc.

---

## 5. Vérifier que les deux agents sont actifs

```bash
docker compose run --rm openclaw-cli agents list
```

Tu dois voir `main` et `assistant` dans la liste.

---

## Récapitulatif

- [x] Workspace `workspace-assistant/` créé sur le VPS
- [x] Agent `assistant` déclaré dans la config avec son workspace
- [x] Fichiers bootstrap initialisés dans `workspace-assistant/`
- [x] Mount Docker ajouté via `OPENCLAW_EXTRA_MOUNTS`
- [x] Gateway redémarré avec les deux fichiers Compose
- [x] Les deux agents visibles dans `agents list`

➡️ **Étape suivante : [Étape 4 — Attacher l'agent aux channels Discord](./etape-04-discord-binding.md)**
