---
type: howto
title: Promises en Node.js
---

# Promises en Node.js

## Le problème : les APIs Node.js sont en callbacks

Les fonctions natives de Node.js (filesystem, réseau…) utilisent des callbacks à l'ancienne :

```js
const fs = require('fs')

fs.readFile('fichier.txt', 'utf8', (err, data) => {
  if (err) throw err
  console.log(data)
})
```

## Solution 1 : util.promisify

Convertit n'importe quelle fonction callback en Promise :

```js
const { promisify } = require('util')
const fs = require('fs')

const readFile = promisify(fs.readFile)

const data = await readFile('fichier.txt', 'utf8')
```

## Solution 2 : fs/promises (Node.js 14+)

Node.js fournit directement des versions Promise de ses APIs :

```js
import { readFile, writeFile } from 'fs/promises'

const data = await readFile('fichier.txt', 'utf8')
await writeFile('sortie.txt', data)
```

## Fetch natif (Node.js 18+)

```js
// Plus besoin de node-fetch
const res = await fetch('https://api.example.com/data')
const json = await res.json()
```

## Gestion d'erreurs en production

```js
// Capturer les rejets non gérés (évite le crash silencieux)
process.on('unhandledRejection', (reason, promise) => {
  console.error('Promise non gérée :', reason)
  process.exit(1)
})
```

## Pattern : wrapper d'API Node.js

```ts
import { readFile } from 'fs/promises'
import { join } from 'path'

async function lireConfig(nom: string): Promise<Record<string, unknown>> {
  const chemin = join(__dirname, 'config', `${nom}.json`)
  const contenu = await readFile(chemin, 'utf8')
  return JSON.parse(contenu)
}
```

## Résumé des modules Promise-natifs

| Besoin | Module |
|---|---|
| Fichiers | `fs/promises` |
| DNS | `dns/promises` |
| Stream | `stream/promises` |
| Timers | `timers/promises` |
| Callback quelconque | `util.promisify` |
