# IFT-6002

## Summary Mini-Test

### Unit Tests
- on teste le comportement des objets
- une chose importante pour les unit tests et de bien faire la séparation des concerns
    - il est inutile de tester des comportements si ceux-ci réfèrent à un comportement d'une autre couche (tester les formats de request dans le domaine au lieu de le faire au niveau de l'interface où ceux-ci devraient être déjà testés avant même de descendre dans l'architecture.)
    - il est aussi inutile de tester des choses qui sont déjà testées dans une autre couche.
- pourquoi tester des comportements?
    - raison d'être du code: problème que l'on cherche à résoudre.
    - les comportements sont plus stables que les sorties directement.
    - critiquer l'architecture
- TDD: tests réflètent le code et vice-versa
    - autre raison: écrire le moins de code possible permettant de faire passer les tests qui s'assurent que les comportements attendus sont accomplis.
    - évite de laisser des bouts de code non-testés
- Processus TDD:
    1. écrire un test qui va échouer
    1. écrire le code qui permet de faire passer le test (minimum de code possible)
    1. refactoring
- Un test possède 3 parties:
    - given: precondition
    - when: tâche exécutée (méthode appelée)
    - then: attente
- core test unitaire en OO:
    - unité: la classe
- les tests unitaires testent les unités de manière isolée
- on teste un seul comportement par test
    - raisons:
        - puisque le test devrait échoué pour une seule raison
        - permet de documenter le code et les comportements remplis par les différentes classes
- le nommage est important
    - raisons:
        - aide à écrire de bons tests
        - quand il échoue on sait ce qui ne fonctionne pas et on peut les comparer (variantes)
        - documentation du code encore une fois
- la première raison d'écrire un test unitaire est d'orienter mon développement et documenter l'application
- la deuxième raison c'est parce qu'ils nous permettent de faire du design d'architecture - nous pousse à réfléchir à comment segmenter les comportements et les attributes entre différentes composantes
- la troisième raison est de s'assurer qu'il n'y a pas de bugs

### Architecture
- classe encapsule la logique avec les données
- Tell Don't Ask (TDA)
    - montrer les bris d'encapsulation
- Loi de Demeter
- faible couplage et forte cohésion
    - trouver le meilleur compromis lorsque l'on design un logiciel
- on utilise l'abstraction pour inverser les chaînes de dépendances
    - permet d'éviter le ripple effect lorsqu'une composante ayant un cycle de vie différent du reste de l'application change
    - par contre nous devenons fortement lié sur un contrat qui a été établi entre l'abstraction et les classes qui en dépendent
        - un changement de cet interface impactera fortement le reste de l'application
    - on va préférer l'utilisation d'interface plutôt de classes abstraites.
        - favoriser la composition par rapport à l'héritage
    - les interfaces intégrés devraient être au même niveau d'abstraction
    - pour décider de où nous devrions intégrer des abstractions:
        - cycle de vie
        - stabilité de l'interface à intégrer (je devrais mettre une abstraction, mais je ne sais pas comment définir cet interface => préférable de repousser à plus tard d'instaurer le contract)
        - degré d'intimité
        - probabilité que les choses changent (utilité)
        - impact si les changements surviennent
