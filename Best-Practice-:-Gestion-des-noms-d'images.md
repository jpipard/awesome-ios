*Mieux profiter des Enums en swift pour mieux gérer les images dans Images.xcassets*

L'idée générale est de ne plus avoir

```swift
    let image = UIImage(named: "myImageName")
```
 - Le problème avec cette ligne de code est la chaine de caractères (en dur) passée en paramètre du constructeur de ***UIImage***, puisque cela empêche le compilateur de nous venir en aide en cas d'erreur de frappe par exemple.

 - L'autre problème réside dans le fait que ce constructeur retourne un optional ***UIImage*** qui nécessite dès lors un "unwrapping" de la constante image pour utilisation.

 - De même si le nom d'une image change (ce cas ne devrait se produire que très rarement) un refactor complet de notre code pour changer tous les appels à ***UIImage*** serait nécessaire.

**Une meilleure méthode** dans ce cas là consiste à créer une ***UIImage*** de la manière suivante :

```swift
    let image = UIImage(assetIdentifier: .PurpleButton)
```

où .***PurpleButton*** est une valeur de l'Enum ***AssetIdentifier*** et ***UIImage(assetIdentifier: AssetIdentifier)*** un nouveau constructeur de ***UIImage*** prenant non pas une chaine de caractère en paramètre mais une valeur d'Enum.

Pour arriver à ce résultat il convient de créer une extension de ***UIImage*** (Dans un fichier .swift à part) que l'on nommera par convention **UIImageAssetExtension.swift**

Ce fichier sert de déclaration à l'Enum ***AssetIdentifier*** d'implémentation au constructeur de ***UIImage*** décrit plus haut.

```swift 
   extension UIImage {
        enum AssetIdentifier: String {
            case AppIcon = "AppIcon"
            case LaunchImage = "LaunchImage"
            case TabBarCheckIn = "tabbar-checkin-bt"
            case TabBarItemFirstOff = "tabbar-timeline-OFF"
            case TabBarItemFirstOn = "tabbar-timeline-ON"
            case PurpleButton = "bt-purple"
            case PurplePendingButton = "bt-purple-pending"
        }

        convenience init!(assetIdentifier: AssetIdentifier) {
            self.init(named: assetIdentifier.rawValue)
        }
    }
```
Il est alors évident que le développeur devra pour chaque ajout d'une
ressource dans Images.xcassets enrichir l'Enum pour faire la correspondance entre le nom et l'Enum. Ce travail "fastidieux" devra à terme être remplacé par un mécanisme automatisé permettant la génération de l'Enum à chaque ajout de ressource dans Images.xcassets

Projets de référence :  [Guestbusters](https://github.com/OMTS/GuestbustersiOSApp)

Sources : [Swift in Practice (WWDC15)](https://developer.apple.com/videos/wwdc/2015/?id=411)

Playground : https://github.com/OMTS/citadel/tree/master/ios-dev/playgrounds/ImageAssetsPlayground.playground