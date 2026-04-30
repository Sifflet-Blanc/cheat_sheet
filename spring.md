# Spring
- Framework
- Java (JVM)
- Ioc Container
	Injection de dépendances
- AOP (programmation axée sur l'aspect)
- Eco-système (ensemble d'application)
  - Spring Boot
  - Spring security
  - Spring Boot (config de Spring pas default)
  - ... 

## IoC Container

## AOP

## Transaction
Actions atomiques, un ensemble d'action qui doivent se produire ensemble, si une echoue toutes doivent echoué.
ACID

L'indépendance des services doit etre maximum (Ex: SQLException, une Exception n'est un un pb ici c'est juste que si on change de moteur de bdd il faudra changer la couche service ce qu'on ne veux pas)

Bean c'est un object Java géré par Spring, jamais de new avec les beans.


On peut faire nous même des wrap de controler quand on a toujours les même @

## Scope
Dans spring core:
- **singleton (défaut)**: 1 instance de bean dans l'application context
- **prototype**: 1 instance nouvelle a chaque demande de conteneur

Dans spring web:
- **Session**
- **Request**


Injection des beans dans les objects **@Autowired**(defini par default)
- **Constructeur** (recommandé **toujours** sauf exception)
- Champ (seulement dans des cas spécifiques, des tests, ou sur du legacy)
- Setter (idem)

**3 Types d'injection :**
- Par Type (défaut)
- Par Nom : utilise l'id du bean précisé par **@Qualifier("bean-id")**
  [exemple: dans les tests, si plusieurs datasources]
- Factory-Method



fichier de config Spring pour definir des Beans (généralement pour des objects externes)


# AOP
Concepts 
- **JoinPoint** : l’endroit où appliquer le traitement
  - une méthode
  - une initialisation de variable ...
- **PointCut** : la règle de sélection du JoinPoint
  - une annotation
  - une classe
  - un nom de méthode ...
- **Advice** : décrit quand on doit mettre en place l’aspect
  - @Before
  - @After
  - @Around
  - @AtAfterThrowing ...


## @Transactional 
- readOnly 
	boolean (optimisation de perf)
- transactionManager
	choisir parmi de multiples txManagers
- Propagation
	Gerer les créations et imbrications
