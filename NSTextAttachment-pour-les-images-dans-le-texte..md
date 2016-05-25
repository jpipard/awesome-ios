#### Qu'est ce qu'un NSTextAttachment ? 

Un NSTextAttachment est un composant que l'on peut ajouter à une NSAttributedString.

#### Dans quel cas l'utiliser ?

Il peut étre utilisé pour affiché une image (picto) dans du texte ou pour aligné du texte avec un picto.

#### Avantages

- Un seul UILabel suffit. plutot qu'un UILabel et un UIImageView ou 2 UILabels et un UIImageView.
- Plus besoin de gerer les contraintes d'alignement entre les UIlabels et les UIImageView.
- L'alignement est mieux géré.

##### Ressources

Sources : [iOS Typography (Raizlabs)](http://www.raizlabs.com/dev/2015/08/advanced-ios-typography/)

Playground : https://github.com/OMTS/citadel/tree/master/ios-dev/playgrounds/TextAttachment.playground