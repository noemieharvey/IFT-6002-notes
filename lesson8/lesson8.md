# IFT-6002

## Micro-Service
- un micro-service est un service qui est indépendemment déployable
- si un micro-service A dépend d'un micro-service B
    - nous ne pouvons appeler directement le micro-service B via les tests
        - sinon on crée une contrainte temporelle dans l'ordre de déploiement des micro-services et ils ne sont donc plus des micro-services par définition.
        - pour ce faire, nous ferons des tests pour A qui vont utiliser un double de B (mock) qui s'attend à ce que B agissent d'une manière donnée
        - nous produirons ensuite une suite de tests que nous fournirons à B qui assurons que B répond au contrat qu'il a pris avec le micro-service A.
        - B conservera ces tests en plus de ses propres tests et s'assura ainsi de respecter son contrat avec A.
        - ceci est l'essence même d'un test de collaboration.

## Professionalism in Software Development
- [Talk Uncle Bob](https://www.youtube.com/watch?v=p0O1VVqRSK0)
- Responsabilité de la propreté du code?
    - développeur ou manager?
        - un professionnel ne devrait pas se cacher derrière des délais ou un manager pour justifier ses décisions
        - analogie avec médecin
        - il s'agit d'un problème que nous nous sommes nous même créé
            - par notre silence
            - par notre manque de caring
            - par notre incapabilité d'expliquer notre point
        - dans beaucoup de cas il y a un problème de communication
    - nous sommes souvent dans une position subordonnée, tandis que nous devrions être considérés comme des professionnels.
        - d'un autre côté, nous ne prenons pas nos possibilités
        - les développeurs devraient se départir de notre orientation technologique et plus se concentrer sur le client et les besoins d'affaires.
    - un autre problème est relié à la formation continue
        - two way responsibility
            - manager: promote this behavior
            - develop: engage itself into learning new things

## Apprendre à apprendre
- modèle de Dreyfus
- 5 stades d'apprentissage:
    1. Être un débutant:
        - suivre la recette
        - comment faire?
        - éprouve des difficultés à accomplir des tâches relativement simples
    1. Débutant avancé
        - piège
        - peut résoudre les problèmes connus
        - avec le temps et non l'expérience
        - une expérience => invalider le modèle mental
        - fausse confiance en soi
        - ne se rend pas compte des problèmes de sa recette
        - rare sont ceux qui atteignent plus que le ce niveau de maîtrise
    1. Compétent
        - model conceptuel
        - véritable compréhension
        - recette non pas d'importance
        - il peut créer ses propres recettes on the fly
        - difficulté à déterminer ce qui est important dans le contexte
        - connaissances non contextualisées
    1. Performant
        - capacité à s'auto-améliorer
        - si on obtient un échec, nous savons immédiatement où est le problème
        - boucle complète de retrospection (inspection/adaptation/transparence)
        - appliquer des maximes
        - ne se satisfait pas de la recette
        - veut comprendre les subtilités
    1. Expert
        - 2-3% de la population mondiale va attendre ce niveau dans une compétence quelconque
        - fonctionne à l'intuition
        - diagnostic différentiel
        - très difficile pour eux de fournir des recettes aux débutants
        - le cerveau a éliminé les recettes -> pour devenir des abstractions (intégré l'information)
- syndrôme de Dunning-Kruger
    - plus on pense en savoir -> moins on n'en sait vraiment
    - plus on en sait -> moins on pense en savoir (imposteur)
- promouvoir l'apprentissage délibéré
- biais de confirmation
- Shu - Ha - Ri
    - Shu: sagesse
        - suivre un maître qui fournit des recettes
    - Ha: arrête la tradition
        - aller voir d'autres maîtres
        - explorer
    - Ri: transcendance
        - avoir sa propre école 
