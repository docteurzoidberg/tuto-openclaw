# Étape 2 — Option A : Reverse proxy avec authentification

> [!NOTE]
> 🚧 **Placeholder** — La technologie de reverse proxy n'est pas encore choisie.
> Cette étape sera complétée une fois le choix effectué.
> Options envisagées : **Traefik**, **Caddy**, **Nginx Proxy Manager**

---

## Principe général

Quelle que soit la technologie choisie, le schéma est identique :

```
Internet
   │
   ▼
[Reverse proxy :443]
   ├─ Certificat SSL/TLS (Let's Encrypt)
   ├─ Auth middleware (BasicAuth, ForwardAuth, OAuth…)
   └─ Proxy vers 127.0.0.1:18789
```

---

## Prérequis communs

- Un **nom de domaine** pointant vers l'IP du VPS (ex. `openclaw.mondomaine.fr`)
- Le port **443 (HTTPS)** ouvert dans UFW :
  ```bash
  sudo ufw allow 443/tcp
  ```
- Le port **80 (HTTP)** ouvert si le reverse proxy gère les certificats Let's Encrypt :
  ```bash
  sudo ufw allow 80/tcp
  ```

> [!WARNING]
> Ne pas ouvrir le port 18789 dans UFW — le reverse proxy communique en loopback.

---

## Ce que devra couvrir cette étape

1. Installation et configuration du reverse proxy choisi
2. Configuration du vhost / route vers `http://127.0.0.1:18789`
3. Certificat SSL automatique (Let's Encrypt)
4. Middleware d'authentification devant l'UI
5. Test d'accès depuis l'extérieur
6. Vérification que le port 18789 reste inaccessible directement

---

## Token OpenClaw + reverse proxy

OpenClaw a son propre système d'authentification par token (`OPENCLAW_GATEWAY_TOKEN`).
Ce token reste requis même avec un reverse proxy — c'est une **double couche d'auth**.

```
Requête
  │
  ▼
[Auth reverse proxy] ← première couche
  │
  ▼
[Token OpenClaw]     ← deuxième couche
  │
  ▼
UI OpenClaw
```

---

> 🚧 *Étape à compléter — technologie à choisir*
