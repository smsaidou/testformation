---
type: reference
title: Analyse systémique — dépôt Git et système ad_images
---

# Analyse systémique — dépôt Git et système ad_images

## Acteurs

| Acteur | Rôle |
|---|---|
| Développeur (`smsaidou`) | Produit et pousse le code |
| Machine locale | Détient la clé privée SSH |
| GitHub | Héberge le dépôt, vérifie l'identité |
| Système `ad_images` | Consomme ce dépôt (dépendance) |

## Flux d'authentification

```
Machine locale
  └─ clé privée id_ed25519
        │
        ▼
     GitHub SSH
        │  vérifie contre clé publique "totot"
        ▼
  accès Read/Write
        │
        ▼
  dépôt testformation
        │
        ▼
  système ad_images
```

## Points de rupture identifiés

**1. Couplage fort machine ↔ accès**
La clé SSH `totot` est liée à un seul ordi. Si cet ordi change ou est perdu, le push est bloqué. Il n'y a pas de clé de secours documentée.

**2. Dépendance implicite de ad_images**
On sait que `ad_images` dépend de ce dépôt, mais la nature exacte de cette dépendance n'est pas documentée — clonage ? import de package ? webhook ?

**3. Pas de CI/CD**
Actuellement tout push est manuel. Si `ad_images` attend une livraison automatique, il n'y a pas de pipeline configuré.

**4. Pas de gestion des accès équipe**
Une seule clé SSH. Si un autre développeur rejoint le projet, il n'a pas accès.

## Risques

| Risque | Probabilité | Impact |
|---|---|---|
| Perte de la machine → plus d'accès | Moyenne | Haut |
| Nouveau dev sans clé → bloqué | Haute | Moyen |
| `ad_images` casse si le dépôt change de structure | Inconnue | Haut |

## Ce qui manque pour que le système soit robuste

1. Documenter le lien exact entre `ad_images` et ce dépôt
2. Ajouter une clé de déploiement (deploy key) pour les environnements CI/CD
3. Définir un process pour onboarder un nouveau développeur
4. Versionner les changements qui impactent `ad_images`
