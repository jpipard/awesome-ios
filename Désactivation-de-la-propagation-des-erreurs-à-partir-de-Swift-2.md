### Gérer les erreurs

Swift 2 introduit une nouvelle gestion des erreurs avec les instructions ***try***, ***do-catch*** et ***throw***.
A ne pas confondre avec les instructions ***try-catch*** qui semblent similaires en objectif-C et d'autres langages, et qui sont dans ces contextes, utilisées pour gérer des erreurs qui sont fatales au bon fonctionnement de l'application.

Swift 2 généralise au contraire l'utilisation de ces instructions à la gestion d'erreurs fonctionnelles : ce nouveau mécanisme remplace celui des ***NSError*** utilisé par l'Objective-C.

Cette nouvelle gestion des erreurs permet de les propager tout au long de la chaîne d'appel des fonctions :

####Exemple :

```swift
func function1() throw {
    try fonction2()
}

func function2() throw {
    try fonction3()
}

func function3() throw {
    // Do something and throw error if needed
    // ...
}

do {
    try function1()
} catch {
    print("An error was catched!")
}
```

Ci-dessus, on attrape toute error survenue dans la fonction 'function3' : l'erreur s'est propagée dans la fonction 'function2', puis dans la fonction 'function1' pour finalement se propager au contexte courrant.

### Désactiver la propagation des erreurs :

Il est possible de désactiver la propagation des erreurs en préfixant l'appel d'une fonction avec ***try!***.

Dans ce cas, toute erreur lève une assertion qui provoque l'arrêt de l'application.

Il est alors **fortement** recommandé de vérifier que la génération d'une assertion en cas d'erreur est compatible avec l'utilisation de l'application.

####Exemple :

L'accès à l'instance d'une base de donnée Realm peut s'effectuer en désactivant la propagation des erreurs, sans générer de bug fonctionnel.
L'accès à l'instance par défaut de la base est réalisé par l'appel d'une méthode d'initialisation qui propage des erreurs. Dans le cas d'un accès à la base, d'un point de vue fonctionnel, soit la base est accessible, soit elle ne l'est pas et dans ce cas l'application est inutilisable. La désactivation ne va donc pas à l'encontre du bon fonctionnement de l'application.

Cette désactivation est notamment très utile (et **nécessaire**) dans le cas où l'on souhaite passer une instance de la base en paramètre d'une fonction, avec comme valeur par défaut l'instance de base par défaut. En effet, l'assignation d'une valeur par défaut ne peut traiter la remontée des erreur, ce qui génère une erreur de compilation.

Projets de référence :  [ASV](https://github.com/OMTS/ASV.git)

Sources : [The Swift Programming Language (Swift 2)](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html#//apple_ref/doc/uid/TP40014097-CH42-ID508)

Playground : https://github.com/OMTS/Citadel/tree/master/ios-dev/playgrounds/DisablingErrorPropagation.playground