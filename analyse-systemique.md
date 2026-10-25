---
type: reference
title: Analyse systémique — dépôt Git testformation
---

# Analyse systémique — dépôt Git testformation

## Acteurs

| Acteur | Rôle |
|---|---|
| Développeur (`smsaidou`) | Produit et pousse le code |
| Machine locale | Détient la clé privée SSH |
| GitHub | Héberge le dépôt, vérifie l'identité |

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
```

## Points de rupture identifiés

**1. Couplage fort machine ↔ accès**
La clé SSH `totot` est liée à un seul ordi. Si cet ordi change ou est perdu, le push est bloqué. Il n'y a pas de clé de secours documentée.

**2. Pas de CI/CD**
Actuellement tout push est manuel. Il n'y a pas de pipeline configuré.

**3. Pas de gestion des accès équipe**
Une seule clé SSH. Si un autre développeur rejoint le projet, il n'a pas accès.

## Risques

| Risque | Probabilité | Impact |
|---|---|---|
| Perte de la machine → plus d'accès | Moyenne | Haut |
| Nouveau dev sans clé → bloqué | Haute | Moyen |

## Ce qui manque pour que le système soit robuste

1. Ajouter une clé de déploiement (deploy key) pour les environnements CI/CD
2. Définir un process pour onboarder un nouveau développeur
3. Versionner les changements structurels du dépôt
