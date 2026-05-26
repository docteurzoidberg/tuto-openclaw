# Étape 3 — Option B : VPN mesh

> [!NOTE]
> 🚧 **Placeholder** — La technologie VPN mesh n'est pas encore choisie.
> Cette étape sera complétée une fois le choix effectué.
> Options envisagées : **Tailscale**, **WireGuard**

---

## Principe général

Un VPN mesh crée un réseau privé virtuel entre tes appareils.
Chaque appareil membre du réseau peut joindre les autres directement,
comme s'ils étaient sur le même réseau local — même depuis Internet.

```
[PC / Mobile]  ←──VPN mesh──►  [VPS]
      │                           │
  Client VPN                 Serveur VPN
  installé                   installé
                                  │
                             OpenClaw :18789
                             (accessible uniquement
                              depuis le mesh)
```

---

## Tailscale vs WireGuard

| | **Tailscale** | **WireGuard** |
|---|---|---|
| **Complexité** | Simple (install + login) | Plus complexe (config manuelle) |
| **Gestion des clés** | Automatique | Manuelle |
| **Dépendance** | Service Tailscale (gratuit jusqu'à 3 appareils) | Aucune dépendance externe |
| **Performances** | Excellentes | Excellentes |
| **Config OpenClaw** | Support natif (`gateway.bind: "tailnet"`) | Via loopback + routing |

---

## Ce que devra couvrir cette étape

1. Installation du client VPN sur le VPS
2. Installation du client VPN sur les appareils utilisateurs
3. Vérification de la connectivité mesh
4. Configuration d'OpenClaw pour écouter sur l'interface VPN
5. Test d'accès depuis l'extérieur via l'IP mesh
6. Suppression du SSH tunnel (devenu inutile)

---

## Config OpenClaw avec Tailscale (aperçu)

OpenClaw a un support natif de Tailscale via `gateway.bind: "tailnet"` :

```json5
{
  gateway: {
    bind: "tailnet",
  },
}
```

Avec cette config, l'UI est accessible via l'IP Tailscale du VPS ou via MagicDNS,
et uniquement depuis les appareils membres du tailnet.

> Pour WireGuard, la config sera différente — à documenter lors du choix.

---

> 🚧 *Étape à compléter — technologie à choisir*
