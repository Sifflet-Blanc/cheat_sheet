[Home](README.md)
# Java

## Programation fonctionnelle, Async et Thread
Quand java fait un appel (a une bdd par exemple) il attend activement donc est bloqué ce qui rend le multi threading obligatoire pour des appli web.

La class `Thread` existe mais elle est très bas niveau on utilise des classes superieur.

Executor permet avec `execute` d'executer un `Runnable` en parallele.

`ExecutorService` extend `Executor` et rajoute un cycle de vie `shtdown()` et `awaitTermination(long timeout, TimeUnit timeUnit)`

`Executors` est une Factory (quand il y a un `s` a la fin du nom de la tache c'est probablement une `Factory`)

Ex de code avec n action a faire en concurence : 
```Java
ExecutorService executorService = Executors.newCachedThreadPool();

urls.forEach(u -> executorService.execute(new DownloadTask(u, tmpDir)));

executorService.shutdown();

executorService.awaitTermination(Long.MAX_VALUE, TimeUnit.SECONDS);
```

Pour faire de la programation asynchrone en Java on peut utiliser `Future<T>`
```Java
ExecutorService executorService = Executors.newCachedThreadPool();

urls.forEach(u -> executorService.execute(new DownloadTask(u, tmpDir)));

executorService.shutdown();

executorService.awaitTermination(Long.MAX_VALUE, TimeUnit.SECONDS);
```

Le mot cle `synchronized` (meme emplacement que `static` uniquement sur les fonctions) permet de mettre un verou sur une methode.
`volatile` permet de verouiller la lecture seulement (uniquement pour les attributs) (par contre ces deux mot cle rallentissent potenciellement le programe, ex volatile force la variable a rester dans la ram plutot que dans le cache) .

Les classe `Atomic` (ex `AtomicLong`, `AtomicReference<T>`) est une classe qui a une implementation qui resiste au problème de concurence.


# Stream
Interet de la `Stream` c'est pour des traitements sur des collections. Pour l'acces au données et les ajouts, suppressions ciblée les collections restent meilleures.

3 type d'operation
## Stateless
`filter` ou `map`

## Stateful
`distinc`, `sorted`, `limit`

## Operation finale
`forEach`, `toArray`, `collect`

Possibilitée de traitement parallele avec l'option `.parallel()` transforme le reste du traitement en traitement parallele.
On peut revenir en sequentiel avec `.sequential()`


# Optional 
`Optional<T>` permet de traiter explicitement les cas de nullité.
*Doit être utilisé sur les retours de methode (sauf s'il s'agit de collection (bonne pratique))*
Ne doit pas etre utilisé en attributs de classe ni en paramètre de méthode (bonne pratique)


# JVM
Xms - Xmx (respectivement la mémoire minimale, maximale, pour la jvm (pour la HEAP))
Xss (Changer le nombre de stack)

## Garbage Collector
Caractéristiques : 
- **serial** ou **parallel**
- **Stop the world** ou **Concurrent**
- **Compating**, **non compacting** ou **copying**

### CMS (Concurrent Mark and Sweep)
**Concurrent**

Categorise les objects à chaque passage 
- Nouvel objet toujours utilisé -> Eden
- Eden -> S1
- S1 -> S2
- S2 -> Object a duree de vie longue
    Les passages du garbage collecteur sont de moins en moins frequent plus la categorie est old

### G1 Heap Allocation
**Compacting**
Même principe que CMS mais avec du compacting pour optimiser la lecture mémoire.


## Thread dump / Heap dump

### Thread dump 
Liste les threads java actifs dans la JVM, permet de determiner les cuase d'un pb via les traces et logs de l'appli.

Recuperer le PID du process Java
```bash
sudo -u user jsp -l
```

Obtenir un Tread Dump 
```bash
jstack -l <PID>
```

### Heap dump 
Fait une copie de la mémoire, permet d'identifier un certain nombre de pb lié a celle-ci. Souvent réalisé suite à une erreur de type *OutOfMemoryError*.
```bash
jmap -dump:format=b,file=<FILENAME.hprof> <PID>
```


# Virtual thread
Inclut un system d'alerte qui fait que lors d'une attente IO le processus libère le Thread. Quand la réponse IO arrive une interruption est levé qui vas remettre le processus sur le thread pool.
