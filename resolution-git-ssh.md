---
type: decision
title: Push GitHub via SSH pour le système ad_images
---

# Push GitHub via SSH pour le système ad_images

## Contexte

Lors de l'initialisation du dépôt Git pour le projet `testformation` (système dépendant de `ad_images`), le push initial vers GitHub a échoué à cause de l'absence de credentials HTTPS configurés dans l'environnement.

## Problème rencontré

```
fatal: could not read Username for 'https://github.com': Device not configured
```

L'URL distante configurée initialement utilisait le protocole HTTPS :

```
https://github.com/smsaidou/testformation.git
```

L'environnement ne disposait pas de credentials GitHub (token ou gestionnaire de mots de passe) pour authentifier la requête.

## Décision

Basculer l'URL du remote `origin` vers SSH, qui s'appuie sur une clé SSH locale déjà configurée.

## Commandes appliquées

```bash
git remote set-url origin git@github.com:smsaidou/testformation.git
git push -u origin main
```

## Résultat

```
To github.com:smsaidou/testformation.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```

Le push a réussi. La branche `main` est désormais trackée sur `origin`.

## Séquence complète d'initialisation

```bash
echo "# testformation" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:smsaidou/testformation.git  # SSH dès le départ
git push -u origin main
```

## Impact sur ad_images

Tout système dépendant de `ad_images` qui clone ou interagit avec ce dépôt doit disposer d'une clé SSH autorisée sur le compte GitHub `smsaidou`. En environnement CI/CD, utiliser un deploy key ou un token PAT avec les scopes `repo`.

## À retenir

| Protocole | Prérequis | Recommandé |
|-----------|-----------|------------|
| HTTPS | Token PAT ou gestionnaire de credentials | CI/CD avec secrets |
| SSH | Clé SSH enregistrée sur GitHub | Développement local |
