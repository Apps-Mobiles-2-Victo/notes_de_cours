---
title: "Architecture avec ViewModel"
---

# Architecture avec ViewModel


### 44.1 Ajouter un fichier dans le dossier ui

Dans la vue `Project`, Android Studio montre les dossiers ui et theme fusionnés puisqu'il n'y a rien d'autre que le dossier theme sous ui .

![Illustration](../images/page_153_img_01_306x192.png)


Pour ajouter un fichier dans le dossier ui, vous pouvez passer à la vue `Project Files`.

Faites un clic droit sur le dossier `ui` et choisissez `New / Kotlin Class/File` .


### 44.2 class vs data class


Avec Kotlin, il est possible d’utiliser le mot-clé *data* pour déclarer une classe dont le but premier est de stocker des données.


```kotlin title="Kotlin"
data class MaClasse(
    var unChamp: Int,
    var unAutreChamp: String
)
```


L'avantage, c'est que certaines méthodes sont automatiquement créées pour vous aider à manipuler ces données, par exemple *hashcode(), equals(), copy() et toString()*.


Pour instancier un objet de cette classe :


```kotlin title="Kotlin"
val monObjet = MaClasse(1, "Une donnée")
```


#### Pour plus d'information


* [« Data classes » - Kotlin](https://kotlinlang.org/docs/data-classes.html)


### * [« Kotlin data class — Behind the mask » - Medium](https://proandroiddev.com/kotlin-data-class-behind-the-mask-51a05ad92ae9)

## 44.3 Le ViewModel comme conteneur d'état


Selon la documentation Android :


>La classe ViewModel est une logique métier ou un conteneur d'état au niveau de l'écran. Elle expose l'état au niveau de l'UI et encapsule la logique métier associée. Son principal avantage est qu'elle assure la mise en cache et la persistance de l'état en cas de modification de la configuration.

On dira que le ViewModel est la source unique de vérité ou source unique de référence ou, en anglais, *Single Source Of Truth* (SSOT).

L'utilisation d'un ViewModel dans une application Android facilitera notamment la gestion de données en provenance d'une base de données.

Mais avant de se lancer dans la gestion d'une base de données, regardons comment utiliser le ViewModel dans une application sans BD.


Dans cette fiche :


* [Création du ViewModel](#creation-du-viewmodel)
* [Propriétés](#proprietes)
* [Propriétés de support](#proprietes-de-support)
* [Création de la classe UiState](#creation-de-la-classe-uistate)
* [Logique métier](#logique-metier)
* [Accéder à une variable d'état dans le ViewModel](#acceder-a-une-variable-detat-dans-le-viewmodel)
* [Mise à jour de l'état](#mise-a-jour-de-letat)
* [Accéder au ViewModel dans MainActivity](#acceder-au-viewmodel-dans-mainactivity)
* [Instancier le ViewModel dans un composable plutôt que dans la classe MainActivity](#instancier-le-viewmodel-dans-un-composable-plutot-que-dans-la-classe-mainactivity)
* [Ajustements pour le Preview](#ajustements-pour-le-preview)


### Création du ViewModel


Lorsqu'on travaille avec un ViewModel, on n'aura plus de variables d'état déclarées directement dans les composables.

Chacun des ViewModels de l'application sera placé dans un dossier nommé *ui*  et portera un nom qui se termine par ViewModel.

Le dossier *ui* est au même niveau que le fichier *MainActiviy.kt* , par exemple *app/src/main/java/com/monnom/monprojet/ui/HomeViewModel.kt* .

Un ViewModel est simplement une classe qui hérite de la classe *ViewModel*.

```kotlin title="Fichier ui/HomeViewModel.kt"
import androidx.lifecycle.ViewModel
class HomeViewModel : ViewModel() {
    ...
    // constructeur (à utiliser au besoin)
    init {
        ...
    }
}
```

### Propriétés


La classe comprendra une propriété pour chacune des informations qu'elle doit conserver.

Pour que la modification d'une propriété cause le rafraîchissement de la vue, il faut la déclarer en tant que **variable d'état**.

Ici, pas besoin du mot-clé *remember* puisque la classe ne sera pas réinstanciée à chaque recomposition ni lors de la recréation de l'activité.

Notez que lorsqu'il n'y a aucun spécificateur d'accès, une propriété est considérée publique.


### Propriétés de support


Il est conseillé de créer des propriétés privées. Chaque propriété utilisera une propriété de support (*backing property*) pour fournir une valeur au monde extérieur.


>Par convention, le nom d'une propriété privée débute par une barre en bas (_). Son vis-à-vis public porte le même nom mais sans la barre en bas.



### Classe UiState


La technique recommandée pour déclarer les variables d'état du ViewModel consiste à utiliser une classe spécialisée pour gérer ces valeurs.

Puisque ces valeurs sont rattachées à l'état de l'interface utilisateur (UiState), la classe portera un nom qui se termine par *UiState*.

Le ViewModel utilisera une instance de cette classe comme variable d'état.

> Dans le cadre de ce cours, un ViewModel qui n'utilise pas le UiState de façon appropriée ne sera pas accepté.

Cette classe, qui est en fait une **classe de données** peut être déclarée dans le même fichier que le ViewModel.

Les propriétés de la classe UiState doivent être déclarées avec *val* (lecture seulement) et non avec *var*.


```kotlin title="Fichier ui/HomeViewModel.kt"
class HomeViewModel : ViewModel() {
    ...
}

data class HomeUiState (
    val points: Int = 0,
    val message: String = ""
) {
    val partieTerminee: Boolean
        get() = points >= 5
}
```

Ici, `points` et `message` sont déclarées avec *val* dans la signature du constructeur, donc:

1. Des propriétés de classe vont être créées, avec des accesseurs. (sans *val* se serait des paramètres d'appel de fonction ordinaires)
1. Ces propriétés sont en lecture seule, elles ne peuvent pas être modifiées (avec *var* l'écriture serait possible). .

`partieTerminee` est une propriété calculée qui retourne *true* si le nombre de points est supérieur ou égal à 5. Elle n'est pas stockée dans la classe mais calculée à la demande.

Le ViewModel change l'état de l'application en utilisant une nouvelle instance de la classe `HomeUiState`, via la méthode *copy()*.  Cette méthode est automatiquement créée par Kotlin lorsqu'on déclare une classe de données. Elle permet de créer une copie d'un objet en modifiant seulement certaines propriétés.

On peut désormais ajouter au ViewModel une propriété, nommée ici *uiState*, qui fait référence à une instance de cette classe plutôt qu'à une liste de propriétés distinctes.

Pour un état local qui ne provient pas d'une source de données asynchrone, `mutableStateOf` suffit. Compose observe cette propriété et réexécute les composables qui la lisent lorsqu'elle change. Le setter privé réserve les modifications au ViewModel.

```kotlin title="Fichier ui/HomeViewModel.kt"
class HomeViewModel : ViewModel() {
    // Compose observe cette propriété et réagit à ses changements.
    var uiState by mutableStateOf(HomeUiState())
        private set

    fun jouer() {
        if (!uiState.partieTerminee) {
            uiState = uiState.copy(points = uiState.points + 1)
        }
    }
}
```

Dans un composable, on lit directement `viewModel.uiState`; il n'est pas nécessaire d'appeler `collectAsState()`. Lorsqu'un état doit suivre un `Flow` provenant de Room, on peut plutôt l'exposer comme `StateFlow` dans le ViewModel. Cette situation est présentée dans la fiche [Base de données locale avec Room](../05-donnees-persistance/room-base-de-donnees.md).

### Logique métier

Grâce aux ViewModels, il est possible de coder au même endroit toute la logique métier, séparément du code qui gère l'interface utilisateur.

On ajoutera au ViewModel (et non au UiState) une méthode pour chaque opération sur les données.

Ces méthodes sont le seul endroit où le ViewModel modifie l'état. Le setter privé empêche les composables de le modifier directement.

Évidemment, il ne doit pas y avoir de composables dans le ViewModel. Le ViewModel gère des données mais ne fait pas d'affichage.


### Accéder à une variable d'état dans le ViewModel


Le ViewModel peut lire directement `uiState.nomPropriete`.


```kotlin title="Fichier ui/HomeViewModel.kt"
class HomeViewModel : ViewModel() {
    fun jouer() {
        if (!uiState.partieTerminee) {
            ...
        }
    }
}
```


### Mise à jour de l'état

Ce sont les méthodes du ViewModel qui doivent se charger de modifier la propriété d'état `uiState`.

Pour signaler un changement à Compose, il faut attribuer au `uiState` une nouvelle instance, généralement créée avec `copy()`.

`.copy()` est une méthode qui est automatiquement créée par Kotlin lorsqu'on déclare une classe de données. Elle permet de créer une copie d'un objet en modifiant seulement certaines propriétés.

On précise les propriétés à modifier dans la copie en utilisant le nom de la propriété suivi du signe = et de la nouvelle valeur.  Ce sont des arguments nommés, donc l'ordre n'a pas d'importance. Les propriétés qui ne sont pas mentionnées dans la copie conserveront leur valeur initiale.

Voici un exemple de logique métier qui met à jour l'état :


```kotlin title="Fichier ui/HomeViewModel.kt"
class HomeViewModel : ViewModel() {
    var uiState by mutableStateOf(HomeUiState())
        private set

    fun jouer() {
        if (...) {
            uiState = uiState.copy(points = uiState.points + 1)
        }
    }
}
```


### Modifier un tableau

Dans le cas d'un tableau déclaré avec List<...> dans le UiState

```kotlin title="Fichier ui/HomeViewModel.kt"
data class HomeUiState(
    val monTableau: List<String> = listOf(...),
    ...
}
```

Pour ajouter un élément au tableau (à la fin):

```kotlin title="Fichier ui/HomeViewModel.kt"
uiState = uiState.copy(
    monTableau = uiState.monTableau + nouvelElement
)
```

Pour modifier chaque élément du tableau, il faudra prendre une précaution supplémentaire car à la base, il est immuable.

On le transformera en tableau modifiable auquel on applique une instruction.

```kotlin title="Fichier ui/HomeViewModel.kt"
class HomeViewModel : ViewModel() {
    uiState = uiState.copy(
        monTableau = uiState.monTableau.toMutableList().apply { this[indice] = ... }
    )
}

```



### Modifier plusieurs variables


Pour modifier plusieurs variables, il faut faire le traitement dans un seul `copy()` avec toutes les propriétés à modifier. Il est déconseillé de faire plusieurs copies à la suite, pour éviter des mises à jour incohérentes ou inutiles.


```kotlin title="Fichier ui/HomeViewModel.kt"
uiState = uiState.copy(
    points = uiState.points + 1,
    autreChose = autreValeur
)
```


### Accéder au ViewModel dans MainActivity


L'application peut désormais travailler avec le conteneur d'état.

Une variable, nommée ici viewModel, sera instanciée dans la classe MainActivity et elle sera passée en paramètre à ses descendants.

Notez qu'il est déconseillé de passer un ViewModel en paramètre à des fonctions modulables . Cependant, dans le cadre de ce cours, cette pratique est autorisée afin de faciliter votre travail.

Le composable peut lire directement la propriété `uiState` du ViewModel. Compose observe cette lecture parce que l'état est créé avec `mutableStateOf`.


```kotlin title="Fichier MainActivity.kt"
class MainActivity : ComponentActivity() {
    // instanciation du ViewModel
    private val _viewModel: HomeViewModel by viewModels()
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MonProjetTheme {
                MainScreen( _viewModel )
            }
        }
    }
}
@Composable
fun MainScreen( viewModel: HomeViewModel ) {
    val uiState = viewModel.uiState
    ...
    Button(
        onClick = {
            viewModel.jouer()
        }
    ) {
        Text(text = "Jouer")
    }
    Text(text = "points: " + uiState.points )
    ...
}
```


Compose rafraîchit l'interface lorsque le ViewModel remplace `uiState` par une nouvelle valeur. `collectAsState()` est nécessaire lorsque le composable collecte un `Flow` ou un `StateFlow`, comme dans l'exemple Room.


Pour un `StateFlow` provenant de Room, `.value` permet de lire sa valeur courante dans du code non composable. Dans un composable, il faut plutôt le collecter avec `collectAsState()` pour que l'interface soit réactualisée lors des émissions.


### Instancier le ViewModel dans un composable plutôt que dans la classe MainActivity


Pour éviter de passer le ViewModel en paramètre à une foule de fonctions, il est possible de l'instancier dans le plus petit ancêtre commun, c'est-à-dire dans la fonction composable qui est le plus proche parent des composables qui en ont besoin.


Pour instancier le ViewModel dans un composable, il faudra apporter quelques ajustements au projet.


 >Attention : dans le cadre du cours il ne doit y avoir qu'une seule instance du ViewModel dans l'application. Dans les extraits de code qui suivent, le ViewModel est instancié dans une fonction composable mais pas dans MainActivity.


D'abord, il faut ajouter une dépendance.


Cette ligne doit être ajoutée dans le fichier build.gradle.kts  qui se trouve dans le dossier app .


```kotlin title="Fichier app/build.gradle.kts"
...
dependencies {
    ...
    // Pour instancier le ViewModel dans un composable
    implementation("androidx.lifecycle:lifecycle-viewmodel-compose:2.8.5")
}
```


Une fois la dépendance ajoutée, il faut **resynchroniser le projet pour qu'il tienne compte de l'ajout**.


Pour instancier le ViewModel dans une fonction composable, procédez comme suit.


```kotlin title="Jetpack Compose (Kotlin)"
@Composable
fun MonComposable() {
    val viewModel: HomeViewModel = viewModel()
    ...
}
```


Grâce à la dépendance ajoutée plus tôt, Android Studio sera capable de suggérer le import requis pour permettre l'utilisation de la fonction composable viewModel().


```kotlin title="Jetpack Compose (Kotlin)"
import androidx.lifecycle.viewmodel.compose.viewModel
```


Remarquez l'absence du mot-clé by (délégué de propriété ) lorsque le ViewModel est instancié dans un composable alors qu'il était obligatoire quand il était instancié dans la classe.


### Ajustements pour le Preview


Dans le cas où la fonction composable principale (souvent nommée MainScreen) reçoit le ViewModel en paramètre, il faut faire un petit ajustement si vous désirez utiliser la fonctionnalité de prévisualisation dans votre IDE.


```kotlin title="Fichier MainActivity.kt"
@Preview(showBackground = true)
@Composable
fun DefaultPreview() {
    MonProjetTheme {
        val previewViewModel = viewModel<HomeViewModel>()    // cette ligne nécessite l'ajout de dépendance dans build.gradle.kts (voir plus haut)
        MainScreen( viewModel = previewViewModel )
    }
}
```


> **Source** : 

## 1. * [« Présentation de ViewModel » - Android Developers](https://developer.android.com/topic/libraries/architecture/viewmodel?hl=fr)


#### Pour plus d'information


* [« ViewModel et l'état dans Compose » - Android Developers](https://developer.android.com/codelabs/basic-android-kotlin-compose-viewmodel-and-state?)
hl=fr#0


* [« ViewModel Jetpack Compose Android Simple Example » - Bigknol](https://bigknol.com/jetpack-compose/viewmodel-jetpack-compose-android-simple-example/)


* [« Make sure to update your StateFlow safely in Kotlin! » - Droidcon](https://www.droidcon.com/2021/08/25/make-sure-to-update-your-stateflow-safely-in-kotlin/)


* [« View Model Creation in Jetpack Compose » - dev.to](https://dev.to/vtsen/view-model-creation-in-jetpack-compose-2b9e)


* [« Getting started with Jetpack Compose - StateFlow » - Sentry](https://blog.sentry.io/getting-started-with-jetpack-compose/#stateflow)


## Plus petit ancêtre commun des fonctions qui ont besoin du ViewModel


Je vous illustre ici comment déterminer quel est le plus petit ancêtre commun des composables qui ont besoin du ViewModel.


```kotlin title="Jetpack Compose (Kotlin)"
class MainActivity : ComponentActivity() {
    // instanciation du ViewModel
    private val _viewModel: HomeViewModel by viewModels()
    // *** 1 : Le ViewModel n'est pas utilisé ici, il est seulement instancié
    // puis passé en paramètre à un composable.
    // Sommes-nous dans le plus petit ancêtre commun?
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContent {
            TestPlusPetitAncetreCommunTheme {
                MainScreen(_viewModel)
            }
        }
    }
}
@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun MainScreen(viewModel: HomeViewModel) {
    // *** 2 : Le ViewModel n'est pas utilisé ici, il est seulement reçu en paramètre
    // puis repassé à un composable.
    // Sommes-nous dans le plus petit ancêtre commun?
    Scaffold(
        modifier = Modifier
            .fillMaxSize(),
        topBar = {
            CenterAlignedTopAppBar(
                title = {
                    Text(text = "Test plus petit ancêtre commun")
                },
            )
        }
    ) { innerPadding ->
        MainContent(innerPadding, viewModel)
    }
}
@Composable
fun MainContent(innerPadding: PaddingValues, viewModel: HomeViewModel) {
    // *** 3 : Le ViewModel est utilisé ici pour initialiser le uiState
    // afin de passer le uiState en paramètre.
    // Ceci est le plus petit ancêtre commun des composables qui ont besoin du ViewModel (ou du UiState).
    // Le ViewModel aurait dû être instancié ici.
    val uiState = viewModel.uiState
    Column(
        modifier = Modifier
            .padding(innerPadding)
            .fillMaxWidth().fillMaxHeight(),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Top,
    ) {
        MonBouton(viewModel, uiState)
        MaListe(uiState)
    }
}
@Composable
fun MonBouton(viewModel: HomeViewModel, uiState: HomeUiState) {
    Button(
        onClick = {
            viewModel .ajouterHeure()
        },
        enabled = ! uiState .partieTerminee
    ) {
        Text(text = "Enregistrer")
    }
}
@Composable
fun MaListe(uiState: HomeUiState) {
    val dateTimeFormatter = DateTimeFormatter.ofPattern("H:mm:ss.SSS")
    Column(
        modifier = Modifier
            .padding(all = 25.dp)
            .verticalScroll(rememberScrollState())
    ) {
        uiState .heures.forEach { heure ->
            Text(heure.format(dateTimeFormatter))
        }
    }
    if ( uiState .partieTerminee) {
        Text("Bravo!")
    }
}
```



### 69.1 ViewModelFactory


L'instantiation d'un ViewModel peut être réalisée de différentes façons selon les besoins de l'application.


### Application sans base de données


Quand on instancie un ViewModel dans une application sans base de données, le ViewModel n'a pas besoin de recevoir de paramètre.


On peut procéder comme suit :


```kotlin title="Jetpack Compose (Kotlin)"
class MainActivity : ComponentActivity() {
    private val _viewModel: HomeViewModel by viewModels()
    ...
}
```


ou, pour instancier le ViewModel dans un composable :


```kotlin title="Jetpack Compose (Kotlin)"
@Composable
fun MonComposable() {
    val viewModel: HomeViewModel = viewModel()   // requiert l'ajout d'une **dépendance au projet**
    ...
}
```


L'utilisation de viewModels() ou de viewModel() assure que le ViewModel ne sera pas recréé lors de la prochaine recomposition.


### Application avec base de données


Dans le **modèle proposé jusqu'ici pour un ViewModel qui interagit avec la base de données**, le constructeur a besoin de recevoir l'application en paramètre. Pas de problème, les fonctions viewModels() et  viewModel() se chargeront d'injecter l'objet de type Application dans le constructeur.


Mais si le ViewModel avait besoin d'un autre paramètre?


Il faut savoir que viewModels() et viewModel() ne  permettent pas de passer des paramètres personnalisés. Il faut donc trouver une technique pour y arriver.


L'approche suivante fonctionne mais elle a un défaut de taille : le ViewModel sera recréé à chaque fois que l'activité est recréée. Ce sera le cas notamment quand le téléphone passe du mode portrait au mode paysage et vice-versa.


```kotlin title="Jetpack Compose (Kotlin)"
@Composable
fun MainScreen() {
    val categorieViewModel = CategorieViewModel(monParametre)
    ...
}
```


Pour régler ce problème, il faudra travailler avec un ViewModelFactory, qui permet de passer des paramètres personnalisés au ViewModel.


Cette classe peut être codée dans le même fichier que le ViewModel correspondant.


```kotlin title="Fichier ui/CategorieViewModel.kt"
class CategorieViewModelFactory(
    private val _monParametre: MonType
) : ViewModelProvider.Factory {
    override fun <T : ViewModel> create(modelClass: Class<T>): T { // T représente le type du ViewModel (ex :
CategorieViewModel)
        // Vérifie si la classe reçue en paramètre est de type CategorieViewModel ou un de ses ancêtres
        if (modelClass.isAssignableFrom(CategorieViewModel::class.java)) {
            @Suppress("UNCHECKED_CAST") // pour ne pas avoir le message "Warning: Unchecked cast: CategorieViewModel to T"
            return CategorieViewModel( _monParametre ) as T // crée le CategorieViewModel avec le paramètre requis et retourne
cette instance
        }
        throw IllegalArgumentException("La classe n'est pas du bon type.")
    }
}
```


Il est désormais possible de créer le ViewModel avec viewModel() avec des paramètres personnalisés.


```kotlin title="Jetpack Compose (Kotlin)"
val categorieViewModel: CategorieViewModel = viewModel( factory = CategorieViewModelFactory(monParametre) )
```


### Exemples d'application


Il est possible de coder une application mobile sans utiliser de ViewModelFactory.


Par contre, cette technique pourrait être intéressante des différentes situations :


- Le ViewModel travaille avec un contexte quelconque plutôt qu'avec celui de l'application
- Le ViewModel reçoit un id en paramètre afin d'aller chercher les données d'un enregistrement dès son instanciation
- Le ViewModel a besoin de connaître l'identifiant de l'usager actif dans une application multi-usagers


#### Pour plus d'information


* [« Créer des ViewModels avec des dépendances » - Android Developers](https://developer.android.com/topic/libraries/architecture/viewmodel/viewmodel-factories?)
hl=fr


### * [« Why Use ViewModel Factory? Understanding Parameterized ViewModels » - Medium](https://medium.com/@dilip2882/why-use-viewmodel-factory-understanding-)
parameterized-viewmodels-2dbfcf92a11d
