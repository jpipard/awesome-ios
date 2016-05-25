## Cas d'étude :

Une vue est affichée à l'intérieure d'une autre dont la hauteur peut s'accroitre en fonction du device (cf. par exemple la différence d'affichage entre un iPad Air et un iPad Pro).

![figure 1](https://github.com/OMTS/Citadel/blob/master/ios-dev/tutorials/layout-tuto-01/layout-tuto-01-pic-1.png)

Lorsque la hauteur de la vue parente augmente, on souhaite profiter de l'espace supplémentaire
pour augmenter égalememt la taille de la vue enfant, mais pas trop.
C'est ce "pas trop" qui nous intéresse ici : comment permettre d'augmenter la taille de la vue enfant en la limitant à une taille maximum.

![figure 2](https://github.com/OMTS/Citadel/blob/master/ios-dev/tutorials/layout-tuto-01/layout-tuto-01-pic-2.png)

## Résolution :

Dans interface builder, on peut procéder comme suit : (cf. figure 3)

 1. On positionne les vues dans la configuration de taille parente minimale,

 2. On ajoute les contraintes suivantes :
	- contraintes d'espacement C1 telle que :
		- relation = égalité,
		- priorité = 999,
	- contrainte de hauteur C2 telle que :
		- hauteur <= taille maximum souhaitée,
		- priorité = 1000,
	- contraintes d'espacement C3,
	- contrainte de ratio C4 si l'on souhaite que la vue enfant conserve sa forme lors de l'agrandissement.

![figure 3](https://github.com/OMTS/Citadel/blob/master/ios-dev/tutorials/layout-tuto-01/layout-tuto-01-pic-3.png)

Mais, mais... Que se passe t'il ?!?

## Explication :

Lorsque la hauteur de la vue parente est minimale (= H min),
les contraintes C1, C2 et C3 cohabitent paisiblement.

Lorsque la hauteur de la vue parente augmente, les contraintes C1 et C3 tirent sur la vue enfant, augmentant sa hauteur, jusqu'à ce que celle-ci atteigne sa hauteur maximum spécifiée par la contrainte C2. Dès lors la cohabitation pacifique n'est plus possible.

Le layout engine utilise alors les priorités définies sur les 3 contraintes pour choisir lesquelles cèdent le pas aux autres. La priorité de la contrainte C1 étant la plus faible (999 contre 1000 pour C2 & C3), c'est elle qui cède. Dès lors, la vue enfant conserve sa taille maxi souhaitée et c'est l'espacement supérieur qui s'agrandit.
