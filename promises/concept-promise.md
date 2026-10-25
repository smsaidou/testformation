---
type: concept
title: C'est quoi une Promise ?
---

# C'est quoi une Promise ?

## Définition simple

Une Promise, c'est un objet qui représente une valeur **pas encore disponible** mais qui le sera dans le futur (ou qui échouera).

Comme commander un café :
- Le serveur te donne un ticket (la Promise)
- Tu n'attends pas debout au comptoir (le code continue)
- Quand le café est prêt, il t'appelle (le résultat arrive)

## Les 3 états d'une Promise

```
pending   → en cours (le café se prépare)
fulfilled → succès   (le café est prêt)
rejected  → échec    (la machine est en panne)
```

Une Promise ne peut passer qu'une seule fois de `pending` à `fulfilled` ou `rejected`. Elle ne revient jamais en arrière.

## Pourquoi ça existe ?

Avant les Promises, le code asynchrone s'écrivait avec des callbacks imbriqués :

```js
// Avant — "callback hell"
lireFile('a.txt', function(err, a) {
  lireFile('b.txt', function(err, b) {
    lireFile('c.txt', function(err, c) {
      // impossible à lire
    })
  })
})
```

Avec les Promises :

```js
// Après — lisible
lireFile('a.txt')
  .then(a => lireFile('b.txt'))
  .then(b => lireFile('c.txt'))
  .catch(err => console.error(err))
```

## async / await — le sucre syntaxique

`async/await` est une façon d'écrire les Promises comme si le code était synchrone :

```js
async function main() {
  const a = await lireFile('a.txt')
  const b = await lireFile('b.txt')
  const c = await lireFile('c.txt')
}
```

Sous le capot, c'est toujours des Promises. `async/await` ne fait que les rendre plus lisibles.
