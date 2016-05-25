# Best Practice : Gestion des Segues dans les ViewControllers

Comme pour la gestion des noms d'images dans les assets, les segues posent un problème quand il s'agit de les déclencher dans le code d'un ViewController avec un identifiant en chaine de caractères. 
``` swift
performSegueWithIdentifier("FirstViewControllerSegue", sender: nil)
```
Le développeur n'est pas à l'abri d'une erreur de frappe. L'idée est donc de tirer profit du type checking du compilateur et des Enums en Swift pour palier à ce problème.

L'autre avantage dans l'utilisation des Enums est que le compilateur pourra nous avertir si un segue n'est pas encore géré dans 
```swift
func prepareForSegue(segue: UIStoryboardSegue, sender: AnyObject?)
```
Enfin ce best practice est l'occasion de mettre en place (aussi dans un but pédagogique) une utilisation des protocoles extension dans Swift.

Ce best practice se base sur le projet d'exemple [SegueHandler](https://github.com/OMTS/Citadel/tree/master/ios-dev/projects/SegueHandler) 

Le ViewController principal définit un Enum donnant une liste **exhaustive** des identifiants de Segue "enclenchables" par lui. 

```swift 
enum SegueIdentifier: String {
   case FirstViewControllerSegue
   case SecondViewControllerSegue
}
```
Notons que :
* Nous ne déclarons pas les cases comme des chaines de caractères mais utilisant une rawValue de type String.
* La déclaration de cet Enum doit être faite en tout début de la classe du ViewController, avant toute déclaration de properties ou de méthode.

Les déclenchement de Segue se font dans le code de la manière suivante :
```swift
@IBAction func firstButtonTapped(sender: UIButton) {
    performSegueWithIdentifier(.FirstViewControllerSegue, sender: nil)
}
```
**La méthode performSegueWithIdentifier ne prend pas de String en paramètre comme nous en avons l'habitude mais une instance de l'Enum déclarée plus haut. Cette méthode est définie plus loin.**

La préparation des segue se fait de la manière suivante dans la méthode prepareForSegue 
```swift
switch segueIdentifierForSegue(segue) {
    case .FirstViewControllerSegue:
        print("First")
    case .SecondViewControllerSegue:
        print("Second")
}
```
Puisque la liste des case dans l'Enum est exhaustive, le type checking du compilateur détectera les oublis ou les erreurs de frappe.

**La méthode segueIdentifierForSegue est définie plus loin.**

Déclarons à présent un protocole permettant à n'importe quel ViewController de gérer les Segue de cette manière. 
```swift 
protocol SegueHandler {
    typealias SegueIdentifier: RawRepresentable
}

extension SegueHandler where Self: UIViewController, SegueIdentifier.RawValue == String {
    
    func performSegueWithIdentifier(segueIdentifier: SegueIdentifier,
                                    sender: AnyObject?) {
        
        performSegueWithIdentifier(segueIdentifier.rawValue, sender: sender)
    }
    
    func segueIdentifierForSegue(segue: UIStoryboardSegue) -> SegueIdentifier {
        
        guard let identifier = segue.identifier,
            segueIdentifier = SegueIdentifier(rawValue: identifier) else {
                fatalError("Error on segue identifier \(segue.identifier).")
        }
        
        return segueIdentifier
    }
}
```
Le protocole dans sa plus simple expression déclare uniquement un typealias (associatedtype en Swift 3.0) et **le contraint à être conforme à un RawRepresentable.**

Nous utilisons ensuite les protocoles extensions pour définir la méthode 
```swift
performSegueWithIdentifier(segueIdentifier: SegueIdentifier, sender: AnyObject?) 
```
prenant un SegueIdentifier en entrée (et non pas un String) et la méthode 
```swift
segueIdentifierForSegue(segue: UIStoryboardSegue) -> SegueIdentifier
``` 
qui à partir d'une instance de UIStoryboardSegue est capable de retourner un SegueIdentifier valide.

Notons enfin que cette extension de protocole nous permet aussi de contraindre les objets qui veulent être conforme à ce protocole à :
* être des instances de UIViewController
* définir un SegueIdentifier dont la RawValue est obligatoirement une String.

La toute dernière étape est de déclarer notre ViewController comme conforme à ce protocole. 
```swift
class ViewController: UIViewController, SegueHandler {
...
}
```