# IFT-6002

## Résumé cours précédent
- Contexte
    - modèle
        - principes
    - pattern
    - complexité
    - qu'est ce qui sera impacté (changement)
    - CRUD (problème est lui même couplé)
        - peu importe le nombre de couche d'abstraction (architecture) les extrémités resteront couplés
        - utiliser abstraction réduit le couplage
            - pas toujours vrai...
            - vrai si le problème n'exige pas que les deux entités se connaissent
            - pas de solution miracle qui réduit systématiquement le couplage
                - raison pour laquelle les approches architecturales possède toujours une portion "contexte" pour expliquer quand nous devons les utiliser
- types d'objets
    - Object Value
        - immuable
        - comparé par son état
        - possède une valeur foncière
    - Reference object
        - comparé par son identifiant unique
            - ou adresse mémoire à défaut de ne pas avoir d'identifiant unique
- Primitive obsession
    - utiliser des primitives partout (se binder sur un type de base)
    - anti-pattern
    - préférable d'utiliser le vocabulaire du domaine
    - cacher ces types de base derrère des classes qui vont les exploiter
- modèle anémique
    - modèle du domaine dont le comportement est séparé de ses données
        - problème? Ça dépend du contexte (heavy business core => modèle riche)
        - sinon pas de problème
- domaine
    - corps de connaissance (body of knowledge)
    - capture de la connaissance des règles d'affaire du domaine
    - représenter la connaissance des experts du secteur d'activité dans des instructions machines
    - ne doit pas contenir de technologie
        - exemple: @inject en spring boot ne devrait pas se retrouver dans le domaine
        - pure java


## Modèle hexagonal
- modèle hexagonal (plugin)
    - couche domaine
        - couche indépendante
        - rôle: appliquer des règles d'affaire
        - ne connaît pas l'infrastructure ni le contexte dans lequel s'exécuter
    - couche application (service)
        - fourni le contexte
        - rôle de faire des units of work ou des transactions
            - faire un rollback
        - problème d'appeler les classes des services
            - l'organisation du projet devrait crier quel problème nous cherchons à régler non pas comment on résout le probème [screaming architecture](https://blog.cleancoder.com/uncle-bob/2011/09/30/Screaming-Architecture.html)
        - orchestrer les objets du domaine
        - peut aussi faire de la communication entre les autres services, mais ce n'est pas l'objectif
        - le domaine ne connaît pas le contexte applicatif dans lequel il s'exécute
    - couche infrastructure (techno - contient vue, databases, controller, etc.)
        - fourni la technologie (infrastructure)
        - le haut: input
        - le bas: output
    - tout est un plugin au domaine
- exemple avec domaine riche
    - Domain Driven Design (DDD)
        - nous utilisons souvent juste la portion architecturale, mais le DDD implique beaucoup plus que ça
            - comment extraire la méthode de penser des experts du secteur d'acitivité
    - clean architecture
        - ne parle que de la partie technique

## Patterns
### Factory
- objet qui possède la logique de création d'objets
- 2 alternatives (problèmes) à résoudre
    - permet de choisir le type approprié selon le contexte
    - algorithme complexe de création
- si on ne fait pas ça:
    - éviter la duplication de code
    - à quoi bon faire de l'héritage si les utilisateurs ont besoins de connaître le type précis à créer (anti-pattern)
    - prochaine fois qu'on rajoute une classe on doit ajouter la logique de création à tous les endroits que l'on crée ces objets vs centraliser le code dans une classe
- difficile avec factory?
    - nom générique
    - différente sortes:
        - assembler, mapper, converter
        - factory method
        - abstract factory
        - factory class
- attention de faire une factory de factory
- y-a-t'il forcément héritage?
    - habituellement, la factory construira des objets fortement liés entre-eux
    - il n'est pas obligatoire cependant
        - pourrait retourner des objets peu liés qui implémentent tous le même interface
    - certains auteurs avancent cependant qu'une factory devrait toujours faire un choix de type (pas le cas dans le cadre du cours)
- créer des objets qui n'existe pas
    - contrairement à repository qui reprennent de l'information dans la BD pour recréer l'objet (reforger les objets - réhydrater)
- une factory possède donc un plan de construction et elle doit prendre une décision


### Repository
- entrepôt
- à ne pas confondre avec factory
- usine =/= entrepôt
- on crée des objets avec la factory et on les entrepose avec le repository
- donne l'impression de persistance en mémoire
- domaine ne devrait pas connaître le repository
- application layer ne sait pas comment c'est persister, mais elle sait que c'est supposé l'être
    - elle sait aussi quand c'est le temps de persister et de retrouver les objets
- il s'agit d'une interface
- une implémentation de cet interface se retrouvera dans la couche infrastructure
- modélisation objet =/= modélisation relationnel
- repository peut avoir à intérargir avec plusieurs tables (relations)
- registre/archive => exemple de nom business pour un repository
- base de donnée modélisée en fonction d'elle-même
- repository prend la persistance et la traduit dans le language du domaine
- ressemble beaucoup à la factory, mais ne prend pas de décision
- défaut repository
    - plus de code
    - plus complexe
    - plus adapté aux problèmes complexes
- for more complex methods see: DDD sample

#### DAO
- différence avec DAO (data access object)
- technologique
- compatible avec repository
- pattern de l'infrastructure
    - persistance pattern
- comment les combiner?
- 1 DAO par table
- contient le SQL qui va exploiter la table
- ne permet pas de cacher la BD, mais de l'exploiter
- le repository permet de cacher la BD

#### active record
- chaque objet représente sa persistance
- utilise un connecteur par composition
- se trouve et se sauvegarde lui-même
- excellent pour des applications CRUD
- défaut:
    - pas de collaboration entre les objets
    - couplage intense entre objet + persistance et controller + domaine

#### DTO
- data transfert object
- transporter des domaines d'une couche à l'autre
- objet de transport
- servent à connecter deux systèmes
- défaire la présentation d'une couche pour le reconstruire dans une autre présentation
- découpler les couches
- objet de transport est neutre
- un assembler ou un mapper est utilisé pour convertir les DTOs en objets de domaine et vice versa
- normalement utilisé entre les couches externes de l'application
- DTO =/= value object
- attention aux méthodes toDTO() dans le domaine
- legit de faire des attributs public, mais la commmunauté préfère des privates attributes avec getters and setters
- ne pas faire un seul assembler pour tous les transitions de couche
    - un assembler pour Ressources -> Application Services
    - un assembler pour Application Services -> domaine
