# Étape 5 — Accéder à l'UI OpenClaw via SSH tunnel

> [!NOTE]
> Le gateway OpenClaw tourne en loopback sur le VPS (`127.0.0.1:18789`).
> Il n'est pas accessible directement depuis Internet — c'est voulu.
> On crée un tunnel SSH pour y accéder depuis ton PC de façon sécurisée.

---

## Principe du tunnel SSH

```
Ton PC                        VPS
localhost:18789  ←──SSH──►  127.0.0.1:18789
     │                            │
 Navigateur                  OpenClaw UI
```

Le tunnel fait croire à ton navigateur que l'UI tourne localement,
alors qu'elle tourne sur le VPS. Tout le trafic passe par SSH, chiffré.

---

## 1. Ouvrir le tunnel

Depuis un terminal sur **ton PC** :

```bash
ssh -N -L 18789:127.0.0.1:18789 openclaw@<IP_DU_VPS>
```

| Option | Rôle |
|---|---|
| `-N` | Pas de commande à exécuter — tunnel uniquement |
| `-L 18789:127.0.0.1:18789` | Redirige ton port local 18789 vers le port 18789 du VPS |

> [!NOTE]
> La commande ne rend pas la main — c'est normal. Le tunnel reste ouvert tant
> que le terminal est ouvert. Laisse-le tourner en arrière-plan.

---

## 2. Ouvrir l'UI dans le navigateur

Avec le tunnel actif, ouvre :

```
http://localhost:18789
```

Tu dois voir l'interface de contrôle OpenClaw.

---

## 3. Se connecter à l'UI

L'UI demande un token d'authentification au premier accès.

Récupère le token généré pendant l'onboarding :

```bash
grep OPENCLAW_GATEWAY_TOKEN ~/openclaw/.env
```

Copie la valeur et colle-la dans le champ de l'UI.

---

## 4. Optionnel — Tunnel en arrière-plan

Si tu ne veux pas garder un terminal dédié au tunnel, lance-le en background :

```bash
ssh -f -N -L 18789:127.0.0.1:18789 openclaw@<IP_DU_VPS>
```

`-f` envoie le processus en arrière-plan avant d'exécuter.

Pour le fermer :

```bash
pkill -f "ssh -f -N -L 18789"
```

---

## 5. Optionnel — Alias dans `~/.ssh/config`

Pour ne pas retaper la commande à chaque fois, ajoute un alias dans ta config SSH locale :

```bash
nano ~/.ssh/config
```

```
Host openclaw-vps
    HostName <IP_DU_VPS>
    User openclaw
    LocalForward 18789 127.0.0.1:18789
```

Ensuite le tunnel se lance simplement avec :

```bash
ssh -N openclaw-vps
```

---

## Récapitulatif

- [x] Tunnel SSH actif depuis ton PC
- [x] UI accessible sur `http://localhost:18789`
- [x] Connecté avec le gateway token
- [x] Port 18789 toujours fermé sur Internet ✓

➡️ **Étape suivante : [Étape 6 — Vérifications finales et audit sécurité](./etape-06-audit.md)**
