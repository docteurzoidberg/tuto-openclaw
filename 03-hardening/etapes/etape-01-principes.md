# Étape 1 — Principes : exposition sécurisée de l'UI

> Cette étape explique les concepts avant de choisir une technologie.

---

## Situation de départ

Après les parties 1 et 2 :

```
Internet
   │
  VPS (OVH)
   ├─ UFW : seul SSH ouvert
   ├─ OpenClaw Gateway → 127.0.0.1:18789 (loopback)
   └─ Accès UI → SSH tunnel uniquement
```

L'UI n'est pas exposée sur Internet. Pour y accéder depuis ton PC ou ton téléphone
sans SSH tunnel, il faut choisir une solution d'exposition.

---

## Les deux approches

### Option A — Reverse proxy + HTTPS public

```
Internet ──HTTPS──► [Reverse proxy] ──► OpenClaw :18789
                         │
                    Certificat SSL
                    Auth middleware
```

**Principe :** un reverse proxy (Traefik, Caddy, Nginx…) reçoit les requêtes HTTPS,
vérifie l'authentification, et les transmet au gateway OpenClaw en loopback.

**Avantages :**
- Accessible depuis n'importe quel navigateur, n'importe où
- Pas de client à installer sur chaque appareil
- HTTPS automatique (Let's Encrypt)

**Inconvénients :**
- L'UI est exposée sur Internet — surface d'attaque plus grande
- Nécessite un nom de domaine
- Auth middleware obligatoire (sinon l'UI est publique)

**Quand choisir cette option :**
- Tu veux accéder à l'UI depuis un navigateur sans rien installer
- Tu as déjà un domaine et une config Traefik/Caddy

---

### Option B — VPN mesh (accès privé)

```
Internet
   │
[VPN mesh : Tailscale, WireGuard…]
   │
  VPS ──► OpenClaw :18789
   │
  PC / Mobile (client VPN installé)
```

**Principe :** un VPN mesh crée un réseau privé virtuel entre tes appareils.
L'UI OpenClaw n'est jamais exposée sur Internet — accessible uniquement
depuis les appareils membres du réseau.

**Avantages :**
- Zéro exposition sur Internet
- Pas besoin de domaine
- Sécurité maximale — accès impossible sans le client VPN

**Inconvénients :**
- Nécessite d'installer un client sur chaque appareil
- Dépendance à un service tiers (pour Tailscale) ou config plus complexe (WireGuard)

**Quand choisir cette option :**
- Tu veux le maximum de sécurité
- Tu contrôles tes appareils et peux installer un client VPN
- Tu n'as pas de domaine ou ne veux pas exposer de service sur Internet

---

## Règle de base commune aux deux options

> [!IMPORTANT]
> Quelle que soit l'option choisie, OpenClaw doit **toujours** rester configuré
> avec `gateway.bind: "lan"` ou `"loopback"` — jamais `"0.0.0.0"`.
> C'est le reverse proxy ou le VPN qui gère l'exposition, pas OpenClaw directement.

---

## Décision

| Critère | Option A (Reverse proxy) | Option B (VPN mesh) |
|---|---|---|
| Besoin d'un domaine | ✅ Oui | ❌ Non |
| Client à installer | ❌ Non | ✅ Oui (chaque appareil) |
| Exposition Internet | ⚠️ Oui (HTTPS) | ✅ Non |
| Facilité d'accès mobile | ✅ Facile | ⚠️ Client requis |
| Niveau de sécurité | 🟡 Bon (si bien configuré) | 🟢 Maximal |

➡️ [Option A — Reverse proxy](./etape-02-reverse-proxy.md)
➡️ [Option B — VPN mesh](./etape-03-vpn-mesh.md)
