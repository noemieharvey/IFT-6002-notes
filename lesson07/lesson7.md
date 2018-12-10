# IFT-6002

## Tests Non-Unitaires
### Pourquoi en faire?
- couche bordure difficile à tester
- s'assurer que deux modules fonctionnent bien ensembles (collaboration)
- s'assurer qu'on répond bel et bien aux besoins du client
- tester la performance
- tester la sécurité

### Différent types de tests
#### Classification par raison d'échec
![](agileTestQuadrant.png)

- La version présentée en cours est un peu différente:
    - Supporting the team: drive développement
- Quadrant Inférieur Gauche:
    - indique comment programmer l'application
    (maintenabilité)
- Quadrant Inférieur Droit:
    - indique si l'application répond à des critères de nature technologique
    (caractéristiques non-fonctionnel)
- Quadrant Supérieur Gauche:
    - indique si l'application répond à la demande formulée par le client qui donne des cas d'utilisation de l'application (caractéristiques fonctionnel)
- Quadrant Supérieur Droit:
    - indique si l'application réussit à répondre à un besoin auprès de l'utilisateur de manière adéquate
    (production de valeur)

#### Classification par degré d'intégration
- degré: distance entre point d'injection et le point de sonde
- existe plusieurs catégorisation
    - Traditionnelle: unitaire, service, end-2-end
    - Google: small, medium, large
- on utilise habituellement une pyramide
    - représente deux choses
        - quantité relative du nombre de tests de chaque catégorie
        - dégradé de degré
            - il existe des small moins small que d'autres
##### Définition des degré
- small:
    - teste moins qu'une composante
    - pas de technologie impliquée
- medium:
    - teste une composante dans son ensemble
    - connection
- large:
    - teste plus d'une composante à la fois

##### Pourquoi vouloir plus de small
- moins coûteux
- plus rapide
- plus précis (moins d'explosion combinatoire)
- plus le test est large, plus il y aura plusieurs raisons d'échoué de manière non-intentionnelle
    - éviter le testing fragile problem
        - syndrome dans lequel:
            - je roule les tests
            - après un petit changement
            - tous les tests se mettent à échouer
            - les tests devraient fournir de la confiance au programmer, pour faciliter la maintenance de l'application
            - si les tests brisent constamment, ils ne donnent pas confiance aux programmers
- lorsqu'on a plus de tests bout en bout que de tests unitaires:
    - ice cream cone testing
- proportions souhaitables des différents degrés:
    - 70% small
    - 20% medium
    - 10% large

### Interesting Quotes
- plus un test est large, moins il doit être précis
- les assertions doivent être reliés aux raisons d'échouer

### Algorithme de l'écriture de tests automatisés
1. Identifier la raison d'échec
1. Trouver le plus petit degré d'intégration possible pouvant démontrer l'échec
    - plus le test est de faible degré, plus le retour sur investissement sera grand:
        - découverts plus tôt
        - identifie clairement le problème
        - exécution plus rapide
        - diminue la fragilité
        - plus facile de couvrir les différents branchements
        - maintenabilité accrue
        - impose une pression sur le desgin (principe sous-jacent au TDD - avoir du feedback sur nos décisions architecturales)
1. Écrire le test
