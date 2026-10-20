---
title: "Formulaires de données (Ajout, Modification, Suppression)"
---

# Formulaires de données (Ajout, Modification, Suppression)


Via le DAO, on peut effectuer des opérations CRUD (Create, Read, Update, Delete) sur les données de la base de données.

Les requêtes SQL pour *Create*, *Update* et *Delete* sont automatiquement générées par Room à partir des annotations `@Insert`, `@Update` et `@Delete` placées sur les méthodes du DAO.

Par contre, pour *Read*, il faut écrire une requête SQL avec l'annotation `@Query` sur la méthode du DAO. On peut avoir à obtenir une liste, ou un seul élément à partir d'un identifiant.

Le `@Delete` généré par Room supprime l'enregistrement correspondant à l'objet passé en paramètre.  Si vous ne possédez que l'identifiant, vous devrez soit:
- écrire manuellement la requête de suppression, soit
- d'abord retrouver l'objet complet dans la base de données à l'aide d'une requête SQL avec l'annotation `@Query` sur la méthode du DAO.

Les requêtes *Create* et *Update* sont généralement effectuées à partir d'un formulaire de saisie de données. Le formulaire peut être utilisé pour ajouter de nouvelles données ou pour modifier des données existantes.  

Une validation des données saisies est souvent nécessaire avant de les enregistrer dans la base de données. 

## Validation

C'est dans un ViewModel que la logique de validation sera codée.

Les bonnes pratiques de programmation demandent d'utiliser un ViewModel propre à la page du formulaire.


Ceci permet de mieux séparer les responsabilités (*Single Responsibility Principle*) :


- Un ViewModel gère la liste et les opérations CRUD avec la base de données.

- L'autre gère l'état du formulaire et sa validation (il n'accédera pas à la base de données)


Je vous suggère d'ajouter la validation à la méthode qui met à jour la valeur saisie. En effet, si vous travaillez avec deux méthodes séparées, vous n'aurez pas de garantie que le _uiState.update qui met à jour la valeur saisie soit terminé avant qu'une autre méthode, telle la validation, tente d'utiliser cette valeur.


Voici un exemple:


```kotlin title="ViewModel"
class FormulaireCategorieViewModel : ViewModel() {
    ...
    fun ajusterEtValiderTitre(titre: String) {
        var messageErreur= ""
        if (titre.length > 100) {
            messageErreur = "Le titre doit comporter 100 caractères ou moins."
        }
        _uiState.update {
            it.copy (
                _titre = titre,
                _messageErreurTitre = messageErreur
            )
        }
    }
}
```


```kotlin title="Jetpack Compose (Kotin)"
OutlinedTextField(
    value = ...,
    onValueChange = {
        formulaireCategorieViewModel.ajusterEtValiderTitre(it)
    },
    label = { Text("Titre") },
    ...,   // autres configurations, par exemple choix du clavier
    isError = formulaireCategorieUiState.messageErreurTitre != "",
)
```


Pour afficher le message, vous pouvez utiliser l'attribut supportingText du composant OutlinedTextField. Il est possible d'y mettre un composable conditionnel qui affichera le message d'erreur seulement si celui-ci n'est pas vide.


```kotlin title="Jetpack Compose (Kotin)"
OutlinedTextField(
    value = ...,
    onValueChange = {
       formulaireCategorieViewModel.validerEtAjusterTitre(it)
    },
    label = { Text("Titre") },
    ...,   // autres configurations, par exemple choix du clavier
    isError = formulaireCategorieUiState.messageErreurTitre != "",
    supportingText = {
        if (formulaireCategorieUiState.messageErreurTitre != "") {
            Text(formulaireCategorieUiState.messageErreurTitre)
        }
    }
)
```


Attention : ceci fait en sorte que la validation n'aura lieu que lorsque le texte est modifié.


À vous de vous assurer que la validation ait lieu même si rien n'est entré dans la case de saisie alors que cette information est obligatoire.


### Internationaliser les messages d'erreur


Je vous propose une technique pour internationaliser les message d'erreur de validation dans le ViewModel.


On sait que pour **retrouver une chaîne internationalisée**, il est possible d'utiliser stringResource.


```kotlin title="Composable (Kotlin)"
val message = stringResource(R.string.le_titre_est_requis)
```


Ceci ne fonctionne cependant que dans un composable, ce qui n'est pas le cas des méthodes du ViewModel.


On peut alors utiliser un contexte avec getString().


Dans le cas où le ViewModel hérite de AndroidViewModel et reçoit une référence à l'application en paramètre, on pourra faire ceci :


```kotlin title="ViewModel (Kotlin)"
val message = application.applicationContext.getString(R.string.le_titre_est_requis)
```

<!-- STROTTIER déplacer vers une section avancée
Mais, lorsque possible, il est préférable de créer un ViewModel qui hérite de ViewModel plutôt que de AndroidViewModel afin de faciliter les tests unitaires. Le contexte de l'application n'est alors plus disponible.


Pour internationaliser les messages d'erreur, une solution intéressante consiste à initialiser dans le ViewModel seulement l'identifiant de la chaîne.


On changera le UIState comme suit.


L'annotation @StringRes agit comme un garde-fou pour assurer qu'on n'entre pas une valeur qui ne correspond pas à une chaîne internationalisée.


```kotlin title="UIState (Kotlin)"
@StringRes private var _erreurTitreId: Int? = null,
```


Et on initialisera cette variable dans le ViewModel :


```kotlin title="ViewModel (Kotlin)"
var messageErreurId: Int? = null
if (titre.isBlank()) {
    messageErreurId = R.string.le_titre_est_requis
}
...
_uiState.update {
    it.copy (
        ...,
        _erreurTitreId = messageErreurId
    )
}
```


C'est le composable qui se chargera de charger la chaîne à l'aide du contexte.


```kotlin title="Composable (Kotlin)"
Text(stringResource(formulaireCategorieUiState.erreurTitreId!!))
```
-->

### Enregistrement


Dans un formulaire pour ajouter ou éditer des données, un bouton permettra de traiter les données saisies.


Dans cet exemple, les données sont insérées dans la base de données à l'aide d'une méthode du ViewModel.


```kotlin title="Jetpack Compose (Kotlin)"
@Composable
fun FormulaireCategorie(viewModel: CategorieViewModel) {
    val coroutineScope = rememberCoroutineScope()
    ...
    Button(
        onClick = {
            enregistrer(...)
        },
    ) {
        Text(text = "Enregistrer")
    }
    ...
}
fun enregistrer(...) {
    coroutineScope.launch {
        viewModel.insererCategorie(...)
    }
    ...
}
```


Une fois l'enregistrement terminé, il est intéressant de retourner à la page qui liste les données.


```kotlin title="Jetpack Compose (Kotlin)"
coroutineScope.launch {
    categorieViewModel.insererCategorie(...)
    ...    // on pourrait par exemple réinitialiser les données du formulaire
    navController.navigate("...")
}
```


##  Retrouver les données d'un enregistrement à modifier


<!-- STROTTIER - DELETE ME
Avant 2025, la documentation officielle d'Android énonçait clairement qu'il faut passer un id en paramètre à une route plutôt qu'un objet.


Le texte a depuis été mis à jour mais cette affirmation demeure pertinente :

```
La transmission de structures de données complexes sur des arguments est considérée comme un anti-modèle. Chaque destination doit être responsable de charger les données de l'interface utilisateur en fonction des informations minimales nécessaires, telles que les ID des éléments. Cela simplifie la recréation des processus et évite d'éventuelles incohérences dans les données.
```

En effet, si on voulait passer un objet complet :


- On serait confrontés à une limite de taille puisque l'objet sera **passé en paramètre à la route**.


- Les données pourraient être obsolètes si modifiées ailleurs.


Pour ces raisons, on ne passera pas une instance du modèle en paramètre à la route. On travaillera plutôt avec un identifiant.

-->

Les données seront retrouvées à partir de l'identifiant dans la base de données à l'aide du ViewModel.


Vous devrez :


- **Dans le DAO** : ajouter une annotation @Query pour spécifier la requête à effectuer pour retrouver les données à partir d'un identifiant. La requête travaillera avec un paramètre id (identifié par :id dans le SQL). L'annotation @Query sera suivie par une fonction qui retourne un Flow<T>.

```kotlin title="Fichier data/CategorieDao.kt"
@Query("SELECT * FROM categories WHERE id = :id")
fun retrouverCategorie(id: Int): Flow<Categorie?>
```


Remarquez que cette fonction n'a pas besoin d'être suspendue car le mécanisme de flux gère l'exécution asynchrone.


- **Dans le ViewModel** : obtenir le DAO de la base de données et définir une fonction qui appelle le DAO pour retrouver les données.


Remarquez que cette fonction utilise firstOrNull() pour retrouver le premier élément émis par le flux puis arrêter le flux. Si elle utilisait collect(), la fonction demeurerait suspendue tant que le flux n'aurait pas terminé d'émettre des éléments. 

À moins d'être initialisées dans un **LaunchedEffect**, les informations recherchées pourraient donc ne pas encore être disponibles au moment où elles doivent être utilisées.


Cette fois, la fonction doit être suspendue car elle utilise la fonction suspendue firstOrNull().


```kotlin title="Fichier ui/CategorieViewModel.kt"
suspend fun retrouverCategorie(id: Int) : Categorie? {
    return categorieDao.retrouverCategorie(id).firstOrNull()
}
```



La fonction composable qui affiche le formulaire recevra l'identifiant en paramètre et retrouvera les données dans la base de données comme suit :


```kotlin title="Fichier ui/FormulaireCategorie.kt"
@Composable
fun FormulaireCategorie( categorieId: Int , ...) {
    // initialiser les variables d'état du ViewModel dès que categorieId change
    **LaunchedEffect**(categorieId) {
        val categorie = categorieViewModel.retrouverCategorie(categorieId)
        ...
    } 
    ...
}
```