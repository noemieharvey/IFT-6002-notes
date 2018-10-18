# IFT-6002
## Cours 2

### Interesting Readings

### Interesting Comments
- en tant qu'ingénieur logiciel pourquoi allons-nous toujours dans les extrêmes (une structure trop complexe ou un système où tout est couplé) au lieu de simplement prédire où les changements se produiront et d'y inclure les abstractions nécessaires pour éviter les impactes néfastes de ces changements.

### Couplage vs Cohésion
- Définitions
  - Couplage
    : degré de dépendance entre les choses
        - on veut du couplage faible puisqu'il sera plus facile de mettre à jour les modules si nous avons besoin de faire des modifications
        - problème
          : couplage trop élevé A -> B -> C <- D + C <- E donc si on modifie C tous les classes subiront des possiblement des conséquences. (ripple effect - un changement à une classe pourra se propager dans les classes qui l'utilise). Ce n'est pas simplement un problème de maintenabilité mais aussi que le code n'est peu ou pas réutilisable. Dans l'exemple ci-dessus, nous pourrions réutiliser C, mais pour réutiliser A dans une autre application, nous devrons aussi apporter B, C, D et E pour qu'elle fonctionne.
        - solution
          : définir des interfaces entre les modules qui seront concis et bien définis
  - Cohésion
    : degré avec lequel les éléments d'un module s'associe à un même concept
- Il s'agit des deux grands axes guidant la construction d'un système. Ces notions se font parfois face. Nous devons donc trouver un compromis.
- Exemples extrêmes:
    - God Class
      : tout coder dans une seule classe, donc aucun couplage, mais peu de cohésion
    - Faire une classe pour chaque méthode, mais nous aurons beaucoup de couplage.
- Ce que l'Orienté Object (OO) a apporté
    - polymorphisme
      : Nous a apporté la gestion des dépendances. Si nous avons un élément C qui connaît une classe polymorphique parent P, nous pouvons donc implémenter P1 et P2 des classes polymorphiques enfants sans rien modifier au code présent.
          - favorise la réduction du couplage puisque C ne connaît pas les classes P1 et P2 directement ni les éventuelles classes futures (P3, P4, etc.)
          - C est donc couplé à un concept plutôt qu'au détail de l'implémentation d'une classe donnée.
          - un problème du polymorphisme est que nous devons penser très sagement à l'implémentation du concept (parent) qui deviendra très difficile et complexe à modifier une fois le système déployé.
    - héritage
    - encapsulation
      : nous encapsulons des données et la logique qui permet de transformer ces données
      [ADD] et par le fait même l'état de l'objet lorsqu'il sera utilisé
    - comment modéliser en OO
      - [ADD] un paradigme nous permettant de réfléchir à nos applications de la même manière que nous pensons au monde réel.
      - réduit la complexité
      - facile la communication avec les clients et les autres programmeurs
      - plus il y a de complexité, il est habituellement préférable d'abstraire et de se rapprocher des concepts du domaine.
        - Complexité -> Abstraction -> Concepts -> Domaine
        - Plus nous nous rapprochons du domaine réel, plus nous réduisons les risques d'impact des changements qui seront moins fréquents/probables.
        - Ainsi, la probabilité du logiciel de changer sont les mêmes que les conditions du problème de changer.
    - DANGER du OO
      : effets de bord
        - il est possible d'empêcher ces effets en utilisant judicieusement l'encapsulation.
    - USE CASE - Calculer un total à payer

        ```{pseudo}
        UI::CalculerTotalÀPayer()
          CalculDeTotal.calculerTotalImpayé(client_id);

        CalculDeTotal:
          calculerTotalImpayé(client_id);
            factures = sql(Connector.execute(SELECT * FROM factures WHERE ...))
            for facture in factures:
              if facture.getStatus() == factureStatusEnum.IMPAYÉ:
                total += facture.getTotal() + facture.getMontantÀPayé();
        Facture: ...
        ```

    - Si le UI dépend directement de CalculTotal qui lui dépend directement du connecteur BD, changer la BD risque de changer son appelant.
      - les impacts des changements architecturaux se propagent dans le sens inverse des directions des dépendances.
      - rajouter des abstractions vient briser les chaînes de dépendances et donc isoler les classes plus sujettes à changer dans leur propre monde.
    - Nous devrions faire des abstractions dans un logiciel lorsqu'une dépendance entre deux modules/classes qui n'ont pas le même cycle de vie doit être établit. L'abstraction en question sera nommée le contrat qui définira les responsabilités qui devront être remplies par les deux composantes.
    - L'introduction d'abstraction vient inverser la chaîne de dépendance.
    - Il faut donc bien choisir ou mettre des abstractions dans le système.
    - Les abstractions en question seront des interfaces ou des classes abstraites (si nous devons introduire des attributs).
    - Nous devrions toujours designer l'abstraction en fonction des besoins de la classe utilisatrice.
    - Pour minimiser les problèmes dans la définition d'abstraction et de définir des méthodes avec le même niveau de détail que le nom du contrat.
    - En OO, on ne modélise pas en fonction de ce qu'on a, mais en fonction de ce que l'appelant a besoin.
    - Une abstraction est un concept du monde réel en omettant d'y inclure des détails qui ne sont pas nécessaire à l'appelant
    - il existe des abstractions pure et non-pures.
      - nous priorisons généralement l'utilisation d'abstraction pure (interface) - capacité de faire quelque chose ou d'occuper un certain rôle
      - nous devons nous demander si la classe utilisatrice doit connaître l'identité des classes qui ont été abstraites.

### Tell Don't Ask
- Références
    - [Martin Fowler](https://www.martinfowler.com/bliki/TellDontAsk.html)
    - [Pragmatic Programmers](https://pragprog.com/articles/tell-dont-ask)
- **N'ALLER PAS CHERCHER LES DONNÉES D'UNE CLASSE POUR PRENDRE UNE DÉCISION DESSUS.**
- ce qui fait mal dans une architecture
  : l'effet d'avalanche
- faire traîner des données, nous introduisons un risque potentiel d'avalanche.
- ne pas créer des classes jalouses - qui n'ont pas assez d'attributs elle-mêmes donc elle cherche constamment à emprunter les attributs des classes qu'elle utilise.
- USE CASE - Calculer un total à payer - version corrigée

    ```{pseudo}
    CompteClient:
      - factures: List<Factures>
      calculerTotalImpayé(client_id);
        total_amount = 0;
        for facture in factures:
            total_amount += facture.getTotalImpayé()
    ```

- Ceci nous amène à concevoir des UIs en tant que plugin à la couche domaine.
- Comment résoudre des problèmes de Tell Don't Ask
  : retracer l'emplacement des données et apporter le code à sa source.
- la manière de savoir si un architecture et bien fait, c'est de driller dans les méthodes et de voir les contextes devenir progressivement plus concrètes (moins abstraite) de la méthode appelante vers la méthode appelée.
- Si deux choses dépendent l'une de l'autre directement sans abstraction, cela implique que nous estimons que les deux composantes possèderont le même cycle de vie.
- Loi de Déméter
  : Toute méthode (M) d'un objet (O) ne devrait appeler que des méthodes provenant:
    - de sa propre classe (O)
    - des paramètres fournis à (M)
    - des objets créés par (M)
    - les objets membres de (O)
  - impossible de toujours la respecter sur l'ensemble d'un système, mais chaque fois que nous la respectons pas nous devrions nous interroger si nous sommes en train d'introduire une contrainte indue dans l'architecture.
- La loi de Déméter et Tell Don't Ask ne s'applique que lorsqu'on parle à des objets et non pas à une structure de donnée.
