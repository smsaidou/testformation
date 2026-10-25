---
type: howto
title: Utiliser les Promises en JavaScript et TypeScript
---

# Utiliser les Promises en JavaScript et TypeScript

## Créer une Promise (JS)

```js
const maPromise = new Promise((resolve, reject) => {
  const ok = true
  if (ok) {
    resolve('succès')   // fulfilled
  } else {
    reject('erreur')    // rejected
  }
})
```

## Consommer une Promise (JS)

```js
// Avec .then / .catch
maPromise
  .then(valeur => console.log(valeur))  // 'succès'
  .catch(err => console.error(err))

// Avec async / await
async function run() {
  try {
    const valeur = await maPromise
    console.log(valeur)
  } catch (err) {
    console.error(err)
  }
}
```

## Typer une Promise en TypeScript

```ts
// Promise<T> — T est le type de la valeur résolue
function fetchUser(id: number): Promise<User> {
  return fetch(`/users/${id}`).then(res => res.json())
}

// async function retourne toujours Promise<T>
async function getUser(id: number): Promise<User> {
  const res = await fetch(`/users/${id}`)
  return res.json()
}
```

## Patterns courants

### Plusieurs Promises en parallèle

```ts
// Attend que TOUTES soient résolues
const [a, b, c] = await Promise.all([
  fetchUser(1),
  fetchUser(2),
  fetchUser(3),
])

// Prend la PREMIÈRE résolue
const premier = await Promise.race([fetchA(), fetchB()])

// Attend toutes, même celles qui échouent
const resultats = await Promise.allSettled([fetchA(), fetchB()])
resultats.forEach(r => {
  if (r.status === 'fulfilled') console.log(r.value)
  if (r.status === 'rejected')  console.error(r.reason)
})
```

### Promise déjà résolue / rejetée

```ts
const ok  = Promise.resolve(42)       // fulfilled immédiatement
const ko  = Promise.reject(new Error('raté'))  // rejected immédiatement
```

## Erreurs fréquentes

| Erreur | Pourquoi | Fix |
|---|---|---|
| Oublier `await` | La valeur est une Promise, pas la donnée | Ajouter `await` |
| `await` hors d'un `async` | Syntaxe invalide | Envelopper dans `async function` |
| Pas de `.catch` / `try/catch` | L'erreur est silencieuse | Toujours gérer le cas rejeté |
| `Promise.all` s'arrête au premier échec | Comportement par défaut | Utiliser `Promise.allSettled` si besoin |
