# IFT-6002

## Différence Mapper, Build, Factory
### Mapper
- terme générique pour dire que nous passons de la forme A d'un objet à la forme A'
- objectif: faire la correspondance entre les attributs des deux classes

### Factory
- 2 niveaux de factory
    - pattern: factory method, abstract factory
    - le concept de production d'objets
- habituellement dans le domaine
- on veut créer pour la première fois une entité
- ne sert pas à reconstruire une instance qui existait déjà auparavant

### Builder
- construire des objets step-by-step
- typiquement, `with`
    - setters qui retourne un pointeur sur l'instance manipulée pour pouvoir chaîné les appels
    - on désire pouvoir construire un objet sans nécessairement tout spécifier l'ensemble des attributs

### Remarque
- Il n'y a pas de problème architectural à faire un `new` dans un Mapper se situant entre la couche de domaine et l'application service.
- La présence de factory qui ne servent qu'à faire des `new` représente un certain "smell" qu'un mapper devrait être utilisé.

## SOLID Principles
### C'est quoi?
- 5 principes qui sont de base en Orienté Object (OO)
- aide à contrôler le niveau de cohésion et de couplage au sein d'une application

### Quels sont les principes?
- S: Single Responsibility Principle (SRP) - principe de cohésion
    - une classe ne devrait avoir qu'un seul acteur
    - une classe ne devrait avoir à changer pour une seule raison
- O: open/close principles (OCP)
    - ouvert à l'extension
    - fermé à la modification
        - cherche à éviter le ripple effect
- L: Liskov Substitution Principle (LSP)
    - si un client (C) dépend d'une classe de base (B), selon Liskov, on pourrait substituer (B) par n'importe quel de ses enfants à n'importe quel moment.
    - revient à dire: ne pas créer des enfants qui pourrait venir briser le contrat de la classe de base
- I: Interface Segregation Principle (ISP)
    - jamais un client ne devrait avoir besoin que d'une partie d'un interface
- D: Dependency Inversion Principle (DIP)
    - un module de haut niveau ne devrait pas dépendre directement d'un module de bas niveau
        - UI = bas niveau
        - haut niveau = conceptuel
    - il devrait y avoir une abstraction entre les deux - établir un contrat
    - l'abstraction ne devrait pas dépendre de détails
        - contrat générique
- Note:
    - L et D ne sont pas des principes en soit, mais stipule seulement comment l'Orienté Objet devrait être utilisé
        - polymorphisme

#### IS-A Principle
- on ne peut faire de l'héritage si on ne peut pas répondre à la question: is a?
    - human is a mammal
    - dog is a mammal
    - dog is not a human
    - human is not a dog

#### I-A Principle
- instabilité/abstraction
    - couplage afférent: combien de classes externes dépendent de la classe en question
    - couplage efférent: combien de classes internes au module dépendent de classes externes
        - Ces deux calculs peuvent aussi être fait au niveau du module
- rapport être couplage efférent et couplage afférent
- plus l'instabilité tend vers 0, moins la classe devrait changer
    - cas extrême
        - classe qui ne possède que du couplage afférent: ne devrait pas changer (ex: classe profonde dans le domaine - instabilité de 0)
        - classe qui ne fait que dépendre sur d'autres: peut changer autant qu'elle le veut (ex: main - instabilité de 1)
        - classe 1/1
            - très abstrait et très instable
            - classe inutile
        - classe 0/0
            - classe concrète qui doit être très stable
- on désire habituellement que l'ensemble des classes se regroupe dans les zones 0/1 et 1/0.
- ratio: (nb_total_classes_concretes/nb_total_classes)/(nb_total_classes_abstraites/nb_total_classes)
