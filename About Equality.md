Ceci n'est pas un article politique ou philosophique (quoique). L'envie de l'écrire m'est venue suite à plusieurs discussions autour de demandes d'évolution sur NFluent. Celles-ci m'ont fait comprendre qu'il était utile d'expliquer la motivation des parti pris historique dans le design de la librairie...
# De l'égalité
## Notes de lecture
1. Tout d'abord, une précision importante: je parle bien ici de l'égalité au sens informatique, ou devrais-je dire 'aux sens informatique', mais j'aurai l'occasion d'y revenir.
1. NFluent: est une biliothèque d'assertions pour Net (C#...). Elle permet donc d'écrire les conditions (de succès) des tests d'une manière simple et lisible mais aussi d'obtenir des messages d'erreurs précis et explicites quand un test échoue. Voir FluentAssertions ou Shouldly en C#, FestAssert en Java, 
2. Tests unitaires: cherchez TDD sur votre moteur de recherche préféré _(oui, je sais, le TDD n'a pas l'exclusivité des tests unitaires, mais je revendique le droit d'exprimer mes préférences)_.

## Egalité et tests unitaires
Un test unitaire vise à vérifier un comportement de la base de code, en général en vérifiant l'**état du programme** après une série d'actions (souvent en suivant le motif [Arrange-Act-Asset](https://defragdev.com/blog/2014/08/07/the-fundamentals-of-unit-testing-arrange-act-assert.html), ou [Given-When-Then](https://martinfowler.com/bliki/GivenWhenThen.html)).

Ce qui m'intéresse ici est la dernière phase: on va **vérifier** (_assert_) que l'**état** du _system under test_ (ou _sut_ pour faire court) est tel qu'attendu.

Dans la pratique, on va vérifeir la valeur de certains champs et/ou propriétés. Par exemple:

```csharp
...
  var sut = new Customer(...);
  sut.SetIsNew();
  // check we get the new client status
  Assert.Equals(Status.New, sut.Status);
...
```

C'est un cas élémentaire, passons au cas suivant:

```csharp
...
  var sut = new ClientRepository();
  
  var client = sut.RetrieveClient(1234);
  
  // check we get the expected client
  Assert.Equals(client, expectedClient);
...
```


## NFluent première saison
	