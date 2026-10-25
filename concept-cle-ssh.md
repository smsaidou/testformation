---
type: concept
title: Comprendre la clé SSH
---

# Comprendre la clé SSH

## C'est quoi une clé SSH ?

Une clé SSH, c'est une paire de fichiers créés sur ton ordi qui servent à prouver ton identité sans mot de passe.

Il y en a toujours deux :

| Fichier | Rôle | À partager ? |
|---|---|---|
| `~/.ssh/id_ed25519` | Clé **privée** — reste sur ton ordi | Jamais |
| `~/.ssh/id_ed25519.pub` | Clé **publique** — enregistrée sur GitHub | Oui |

La clé privée = la clé physique. La clé publique = la serrure posée sur GitHub.

## Comment elle arrive sur ton ordi ?

Elle est générée une seule fois avec la commande :

```bash
ssh-keygen
```

Ensuite la clé publique (`.pub`) est copiée manuellement sur GitHub dans **Settings → SSH keys**.

## Preuve sur le compte GitHub smsaidou

La clé enregistrée sur GitHub pour ce projet :

```
Nom        : totot
Empreinte  : SHA256:eY5f9I6vSFK6mpbUij51IclTC49LFFk+1zatAN2lP2Q
Ajoutée le : 31 juillet 2026
Dernière utilisation : il y a moins d'une semaine
Accès      : Read/write
```

C'est cette clé qui a permis le push du dépôt `testformation`.

## Pourquoi ça a marché directement ?

Parce que la clé existait déjà avant la session de travail. L'ordi et GitHub se connaissaient déjà. Aucune configuration supplémentaire n'était nécessaire.

## Ce qu'il faut retenir

Tout environnement (CI/CD, autre machine, nouveau développeur) qui doit pusher sur ce dépôt doit avoir :

- soit sa propre clé SSH enregistrée sur le compte GitHub
- soit un Personal Access Token (PAT) pour l'authentification HTTPS

Sans l'un ou l'autre, le push échoue avec `Device not configured`.
