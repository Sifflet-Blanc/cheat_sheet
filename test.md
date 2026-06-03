[Accueil](README.md)
# Test

## Type de test 
### tests unitaires ou tests de composants 

Le développeur vérifie son code au niveau du composant qu’il doit réaliser, il vérifie que chaque “brique” est correcte et répond aux spécifications.
- Test sur une chose par test 
- mock toutes les sous fonction pour vraiment tester uniquement la fonction même

### Tests d’intégrations : 

Ils permettent de s’assurer que plusieurs composants du système interagissent entre eux conformément aux spécifications (Est-ce que les composants communiquent bien entre eux ?).
- Appel d'une fonction sans mock les fonctions au quelle elle fait appel

### Tests système (End to End, E2E, Front to Back, n2n)

Ces test sont sur l'api / le front directement, exemple :
- un appel API via l'url 
- une serie d'appel pour voir si tout ce passe bien (ex creer un clien et voir s'il est enregistré)

Appli connue :
- playwright 
- Cypress

### Test manuel

### Test de charge
- Gatling

## Bonne pratique
- Test indépendants
- test = titre explicite pour savoir directement qu'est ce qui a sauté 

### Test diriven developement 
Tester avant de coder la fonction

ce deroule en trois étapes qui boucle :
- imaginer une nouvelle feature et creer le test qu'elle doit passer
- ecrire le code minimum pour qu'il passe le test 
- Refactor le code si possible et recommencer 
