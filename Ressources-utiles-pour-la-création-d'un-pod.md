## Créer un pod

[Tutorial from Tutsplus](http://code.tutsplus.com/tutorials/creating-your-first-cocoapod--cms-24332) (lien référencé par le guide de cocoapods.org)

## Localiser un pod

Dans le fichier *.podspec*, le dictionnaire **s.resource_bundles** défini tous les bundles du pod. Il est possible d'y ajouter les répertoires de localisation :

```ruby
    s.resource_bundles = {
        'BundleName' => ['Pod/Assets/*.png', 'Pod/Localization/*.lproj']
      }
```

Dans une classe du framework, on peut ensuite y accéder ainsi :

```swift
// Get bundle for localized strings
let bundlePath: String! = NSBundle(forClass: MyClass.self).pathForResource("BundleName", ofType: "bundle")
let bundle: NSBundle! = NSBundle(path: bundlePath)
let localizedString = NSLocalizedString("string-key", tableName: nil, bundle: bundle, comment: "")
```

## Poster le fichier *.podspec* sur le dépôt Cocoapods

Afin d'être autorisé à poster le fichier *.podspec*, il faut d'abord ouvrir une session sur son poste au moyen de la commande suivante :

    $ pod trunk register orta@cocoapods.org 'Orta Therox' --description='macbook pro'

Un mail contenant un lien de validation est alors envoyé à l'adresse spécifiée.
