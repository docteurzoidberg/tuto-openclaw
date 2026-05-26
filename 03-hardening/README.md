# 📘 Partie 3 — Hardening *(optionnel)*

> [!NOTE]
> Cette partie est **optionnelle**. Le setup des parties 1 et 2 est déjà sécurisé
> pour un usage personnel. Ces étapes s'adressent à ceux qui veulent aller plus loin.

---

## Objectif

À l'issue des parties 1 et 2, OpenClaw est accessible via SSH tunnel uniquement.
Cette partie couvre deux axes d'amélioration :

1. **Exposition de l'UI** — accéder à l'interface OpenClaw sans SSH tunnel
   (reverse proxy ou VPN mesh)
2. **Hardening du VPS** — protection contre les attaques courantes

La technologie de reverse proxy ou VPN **n'est pas encore choisie** —
les étapes ci-dessous restent volontairement génériques avec des placeholders.

---

## Étapes

- [ ] [Étape 1 — Principes : exposition sécurisée de l'UI](etapes/etape-01-principes.md)
- [ ] [Étape 2 — Option A : Reverse proxy avec authentification](etapes/etape-02-reverse-proxy.md) *(Traefik, Caddy, Nginx…)*
- [ ] [Étape 3 — Option B : VPN mesh](etapes/etape-03-vpn-mesh.md) *(Tailscale, WireGuard…)*
- [ ] [Étape 4 — Fail2Ban : protection SSH et services exposés](etapes/etape-04-fail2ban.md)
- [ ] [Étape 5 — Audit final et checklist hardening](etapes/etape-05-audit-final.md)

---

## Résultat attendu

- UI OpenClaw accessible depuis n'importe où, sans SSH tunnel
- Accès protégé par authentification forte
- VPS durci contre les attaques par force brute

➡️ Choisir entre [Option A — Reverse proxy](etapes/etape-02-reverse-proxy.md) et [Option B — VPN mesh](etapes/etape-03-vpn-mesh.md) selon tes besoins.
