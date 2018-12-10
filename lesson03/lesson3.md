# IFT-6002

## Test Unitaires - Mocks
- un des principe des tests unitaires est de tester les classes de manière isolée.
- cette tâche est parfois difficile à accomplir lorsque les classes que nous voulons tester dépendent de d'autres classes.
- C'est à ce moment que des Test Double deviennent pratiques
- Test Double
    - Dummy objects
    : are passed around but never actually used. Usually they are just used to fill parameter lists.
    - Fake objects
    : actually have working implementations, but usually take some shortcut which makes them not suitable for production (an in memory database is a good example).
    - Stubs
    : provide canned answers to calls made during the test, usually not responding at all to anything outside what's programmed in for the test.
    - Spies
    : are stubs that also record some information based on how they were called. One form of this might be an email service that records how many messages it was sent.
    - Mocks
    : are what we are talking about here: objects pre-programmed with expectations which form a specification of the calls they are expected to receive.
        - Utilité des mocks:
          1. Isoler les classes du monde extérieur
          1. Piloter les tests pour passer dans les différentes branches possibles
          1. Valider que les bons appels avec les bons arguments ont été fait un nombre précis de fois
- [Référence](https://martinfowler.com/articles/mocksArentStubs.html)
- les tests unitaires doivent considérer les classes comme des boîtes noires (ne pas connaître son implémentation, mais bien connaître son comportement attendu)
- les mocks peuvent être dangereux
    - trop de mocks augmentent la fragilité des tests
    - par contre pas assez de tests augmente aussi la fragilité des tests qui dépendront alors de l'implémentation de plusieurs autres classes.
- les mocks devront être adaptés à tous changement du monde externe pour refléter les changements dans l'implémentation des dépendances
    - c'est la raison pour laquelle nous espérons que les mocks considéreront la chaîne de dépendance de la classe comme un ensemble de composantes fortement liés entre elles.
- mocks génèrent une série de bad smells
    - ils remplacent des dépendances
        - trop de dépendances est signe de mauvaises segmentation et pas assez d'abstraction
- on utilise les mocks pour traverser des layers
- Conclusion: Don't mock concrete classes

"A unit test should test a single codepath through a single method. When the execution of a method passes outside of that method, into another object, and back again, you have a dependency.

When you test that code path with the actual dependency, you are not unit testing; you are integration testing. While that's good and necessary, it isn't unit testing.

If your dependency is buggy, your test may be affected in such a way to return a false positive. For instance, you may pass the dependency an unexpected null, and the dependency may not throw on null as it is documented to do. Your test does not encounter a null argument exception as it should have, and the test passes.

Also, you may find its hard, if not impossible, to reliably get the dependent object to return exactly what you want during a test. That also includes throwing expected exceptions within tests.

A mock replaces that dependency. You set expectations on calls to the dependent object, set the exact return values it should give you to perform the test you want, and/or what exceptions to throw so that you can test your exception handling code. In this way you can test the unit in question easily.

TL;DR: Mock every dependency your unit test touches."

[source](https://stackoverflow.com/questions/38181/when-should-i-mock)
