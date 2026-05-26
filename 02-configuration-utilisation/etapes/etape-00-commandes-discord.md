# Mémo — Commandes Discord (slash commands)

> Ces commandes s'envoient directement dans Discord, comme un message normal.
> Elles commencent toutes par `/` et sont traitées directement par OpenClaw —
> pas besoin de passer par le CLI.

---

## Essentielles au quotidien

| Commande | Effet |
|---|---|
| `/status` | État du gateway, modèle actif, usage |
| `/help` | Résumé des commandes disponibles |
| `/commands` | Catalogue complet des commandes |
| `/whoami` | Affiche ton identifiant Discord |
| `/new` | Démarre une nouvelle session (archive l'ancienne) |
| `/reset` | Réinitialise la session en cours |
| `/stop` | Arrête le traitement en cours |
| `/restart` | Redémarre OpenClaw |

---

## Modèle IA

| Commande | Effet |
|---|---|
| `/model` | Affiche le modèle actif |
| `/model list` | Liste les modèles disponibles |
| `/model <nom>` | Change de modèle pour cette session |
| `/models` | Ouvre le sélecteur interactif (Discord) |

---

## Comportement de la session

| Commande | Effet |
|---|---|
| `/think <level>` | Niveau de réflexion : `off` `low` `medium` `high` |
| `/verbose on\|off` | Active/désactive les détails d'exécution |
| `/reasoning on\|off` | Active/désactive l'affichage du raisonnement |
| `/compact` | Compacte le contexte de session (utile si session longue) |

---

## Skills

| Commande | Effet |
|---|---|
| `/skill <nom>` | Lance un skill par son nom |
| `/tools` | Liste les outils disponibles dans cette session |

---

## Administration (propriétaire uniquement)

| Commande | Effet |
|---|---|
| `/config show` | Affiche la configuration OpenClaw |
| `/config get <clé>` | Lit une valeur de config |
| `/config set <clé>=<valeur>` | Modifie une valeur de config |
| `/plugins list` | Liste les plugins installés |
| `/plugins enable\|disable <nom>` | Active/désactive un plugin |

> [!NOTE]
> `/config` et `/plugins` sont désactivés par défaut.
> Pour les activer, demande à l'agent principal :
> ```
> Active les commandes /config et /plugins dans Discord.
> ```

---

## Threads Discord (avancé)

| Commande | Effet |
|---|---|
| `/focus <cible>` | Lie ce thread à une session spécifique |
| `/unfocus` | Retire le binding du thread |
| `/agents` | Liste les agents liés au thread actif |

---

## Conseils

- `/new` vs `/reset` : `/new` archive et repart de zéro, `/reset` efface la session en place
- `/compact` est utile quand une conversation est très longue — ça réduit le contexte sans tout effacer
- `/btw <question>` pose une question de côté sans affecter le contexte de la session principale
