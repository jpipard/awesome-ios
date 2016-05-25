### Où est ce que je peux voir le code ?

Dans ce Playground : https://github.com/OMTS/citadel/tree/master/ios-dev/playgrounds/BadDeepLinking.playground 

Tiré de ce projet :  [Guestbusters](https://github.com/OMTS/GuestbustersiOSApp)

### Pourquoi c'est moche ?

Pour deux raisons : 
- Premièrement parce qu'on revient manuellement à la page d'origine de l'app (changement de tab de la tabBar + popToRootViewController + dismissViewController (au cas ou))
- Ensuite parce qu'on passe des paramètres a un view controller intermédiaire afin que lui affiche celui qui es la cible.

### Suggestions d'alternatives propres
#### Afficher quoi qu'il arrive le viewController cible en modal
##### Avantage : 
- On élimine les deux problèmes précédents.

##### Inconvénients : 
- On ne respecte pas l'architecture d'origine de l'app.
- On pourrait ouvrir le viewController cible alors qu'on est déjà sur ce lui ci ou sur un détail de celui ci.

#### Ré-instancier le "entry point" du story board + déclarer un protocol a suivre pour les view controllers qui gèrent le deep linking.
##### Avantages : 
- On évite le premier problème grâce a la ré-instanciation.
- Le deuxième problème est géré proprement grâce a un protocol.

##### Inconvénients : 
- On perd le contexte des autres onglets dans le cas d'une tabBar (contenu non sauvegardé perdu)
- Difficulté de mise en place du protocol.

