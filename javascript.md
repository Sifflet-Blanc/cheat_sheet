[Home](README.md)
# JavaScript

Event loop & Mono thread 

## package manager
file `package.json`

mettre `^` ou `~` devant les dependances pour avoir de la souplesse (^4.2.3 = 4.X.X (forcement superieur), ~4.2.3 = 4.2.X)

The `package-lock.json` is created by `npm install`/`npm i` it fixe the version and the dependance tree.
`npm ci` to only install from `package-lock.json`


### Hoisting
Fait remonter les déclarations de variables au plus haut de son scope.

### Other 

ne pas utiliser `var` prefere
- `let` 
- `const` 

avec un `#` devant le nom de la variable (sans espace) la variable sera privée a la classe.
