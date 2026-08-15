---
title: "Variables d'état avancées et State Hoisting"
---

# Variables d'état avancées et State Hoisting


### 28.1 Hisser l'état (state hoisting)


Lorsqu'une fonction modulable déclare une variable d'état et qu'elle appelle une autre fonction (modulable ou non) qui doit modifier cette variable d'état, il faut
utiliser un mécanisme qui s'appelle hissage d'état (en anglais : state hoisting) pour y parvenir.


Il s'agit de passer en paramètre une **expression lambda]** qui permet de modifier la variable
d'état.


Cette technique est nécessaire puisqu'en Kotlin, les paramètres sont immuables. Il n'est donc pas possible de passer une variable d'état en paramètre à une
fonction qui doit modifier cet état.


Pour éviter d'avoir à hisser l'état trop de fois, le propriétaire d'état (la fonction modulable dans laquelle la variable d'état est déclarée) devra être le plus petit ancêtre
commun des fonctions modulables qui doivent lire ou écrire dans cette variable.


Le nom du paramètre de l'expression lamba sera habituellement du genre onXXXChange où XXX représente le nom de la variable d'état.


Dans le traitement de l'expression lambda, on assignera le mot-clé **it]** à la variable d'état.


```kotlin title="Jetpack Compose (Kotlin)"
@Composable
fun MainScreen() {
    var maVariableDEtat : String by remember { mutableStateOf("") }
    ...
    Button(
        onClick = {
            faireQuelqueChose( onMaVariableDEtatChange = { maVariableDEtat = it })
        }
    ) {
        Text(text = "Appliquer")
    }
}
```


Dans la déclaration de la fonction, il faut préciser les types de l'expression lambda :


#### Après le nom du paramètre suivi de deux points (:), on précisera entre parenthèses le type du paramètre que
l'expression lambda reçoit. Il s'agit du type de la variable d'état.


#### À la suite de la flèche, on précisera le type de la valeur de retour de l'expression lambda, soit **Unit]**.


À l'intérieur de la fonction, on appellera l'expression lambda au moment approprié, en lui passant en paramètre la valeur qui doit être assignée à la variable d'état.


```kotlin title="Jetpack Compose (Kotlin)"
fun faireQuelqueChose( onMaVariableDEtatChange : ( String ) -> Unit) {
    ...
    val nouvelleValeur = ...
    onMaVariableDEtatChange ( nouvelleValeur )   // assigne la nouvelle valeur à la variable d'état
}
```


### Case de saisie simple


Un scénario souvent rencontré pour le hissage d'état consiste à assigner à une variable d'état la valeur d'une case de saisie.


Dans le cas le plus simple, on n'a pas à déclarer la fonction qui reçoit l'expression lambda. C'est la fonction modulable TextField qui le fait pour nous. Il suffit
d'appeler la fonction en lui passant l'expression lambda appropriée.


```kotlin title="Jetpack Compose (Kotlin)"
TextField(
    value = titre,
    onValueChange = { titre = it } ,
    label = { Text("Titre") }
)
```


### Fonction qui affiche une case de saisie


Dans ce second exemple, on travaille avec une fonction modulable qui affiche un texte suivi d'une case de saisie.


Il est ici nécessaire de passer en paramètre la variable d'état (pour afficher sa valeur dans la case) en plus de l'expression lambda (pour modifier la valeur selon ce
qui est saisi).


```kotlin title="Jetpack Compose (Kotlin)"
@Composable
fun MainScreen() {
    var nom by remember { mutableStateOf("") }
    SaisieNom(nom = nom, onNomChange = { nom = it } )
}
@Composable
fun SaisieNom(nom: String, onNomChange: (String) -> Unit ) {
    Column {
        Text(
            text = "Valeur de la variable d'état: $nom",
        )
        OutlinedTextField(
            value = nom,
            onValueChange = onNomChange ,
            label = { Text("Saisir une valeur :") }
        )
    }
}
```


### Hisser l'état deux fois


Parfois, l'état doit être hissé vers une fonction qui, elle aussi, doit hisser l'état vers une autre fonction.


À ce moment, l'expression lambda qui a été fournie lors du premier hissage n'aura pas à être redéfinie lors du second hissage.


```kotlin title="Jetpack Compose (Kotlin)"
@Composable
fun MainScreen() {
    // La varialbe d'état doit être déclarée ici car elle est utilisée dans la barre de titre.
    var points: Int by remember { mutableStateOf(0) }
    Scaffold(
        modifier = Modifier.fillMaxSize(),
        topBar = {
            CenterAlignedTopAppBar(
                title = {
                    Text(text = "Mon application à $points points")
                }
            )
        },
        content = { innerPadding ->
            // Il faut hisser l'état pour que les points puissent être modifiés dans le contenu.
            MainContent(
                modifier = Modifier.padding(innerPadding),
                points,
                onPointsChange = { points = it }
            )
        }
    )
}
@Composable
fun MainContent(
    modifier: Modifier = Modifier,
    points: Int,
    onPointsChange: (Int) -> Unit
) {
    Column(
        modifier = modifier.fillMaxWidth(),
        horizontalAlignment = Alignment.CenterHorizontally,
    ) {
        ...
        Button(
            onClick = {
                // On hisse l'état une seconde fois.
                // Cette fois, pas besoin de fournir l'expression lambda puisqu'elle a déjà été passée
                // en paramètre à cette fonction.
                Jouer(points, onPointsChange )
            }
        ) {
            Text(text = "Jouer")
        }
    }
}
fun Jouer(points: Int, onPointsChange: (Int) -> Unit ) {
    ...
    onPointsChange(points + 1)
}
```


#### Pour plus d'information


* [« État et Jetpack Compose - Hisser un état » - Android Developers](https://developer.android.com/jetpack/compose/state?hl=fr#state-hoisting)


* [« Unlock the Power of State Hoisting in Jetpack Compose » - Medium](https://medium.com/@tenigada/unlock-the-power-of-state-hoisting-in-jetpack-compose-)
574f742c4721


### * [« Où hisser l'état? » - Android Developers](https://developer.android.com/jetpack/compose/state-hoisting?hl=fr)
29. Exercice 3



---


### 79.1 derivedStateOf()


Afin d'améliorer les performances de votre application Android, il est possible d'utiliser derivedStateOf() afiin d'éviter les recompositions superflues.


Prenons l'exemple d'une application qui se termine après 10 itérations. Il n'est pas souhaitable qu'à chaque fois que l'itération est incrémentée, l'application soit
recomposée.


Pour que la recomposition n'ait lieu qu'après les 10 itérations, la variable pourra être déclarée avec derivedStateOf() comme suit :


```kotlin title="Kotlin"
val afficherFinPartie by remember {
    derivedStateOf { iterations > 10 }
}
if (afficherFinPartie) {
    ...
}
```


#### Pour plus d'information


* [« Effets secondaires dans Compose - derivedStateOf : convertir un ou plusieurs objets d'état en un autre état » - Android Developers](https://developer.android.com/jetpack/compose/side-effects?hl=fr#derivedstateof)


* [« Jetpack Compose: remember, mutableStateOf, derivedStateOf and rememberSaveable explained » - Medium](https://stefma.medium.com/jetpack-compose-)
remember-mutablestateof-derivedstateof-and-remembersaveable-explained-270dbaa61b8


### 79.2 mutableListOf comme variable d'état


Selon la documentation officielle de Android
 :


### Attention : Si vous utilisez des objets modifiables tels que ArrayList<T> ou mutableListOf() en tant


### qu'état dans Compose, les utilisateurs verront des données incorrectes ou obsolètes dans votre
application.


Le problème, c'est qu'un objet créé avec mutableListOf() n'est pas observable alors il ne forcera pas le rafraîchissement de l'écran quand il est modifié.


Par exemple, avec ce code, rien n'apparaîtra à l'écran quand le bouton est cliqué.


```kotlin title="Kotlin"
@Composable
fun MainScreen() {
    val  heures : MutableState<MutableList<String>> = remember {
        mutableStateOf( mutableListOf() )
    }
    Column {
        Button(
            onClick = {
                heures .value.add(LocalDateTime.now().toString())   // la modification de la liste ne rafraîchit pas l'écran
            }
        ) {
            Text(text = "Ajouter")
        }
        AfficherItems(heures.value)
    }
}
@Composable
private fun AfficherItems(items: List<String>) {
    Column {
        for (item in items) {
            Text(text = item)
        }
    }
}
```


Il faut donc utiliser une astuce pour forcer le rafraîchissement de l'écran si une variable d'état a été créée avec mutableListOf().


Cette astuce consiste à copier les valeurs de la liste dans une liste temporaire, à effectuer les modifications désirées dans cette liste temporaire puis à réassigner la
liste originale.


Cette réassignation crée une nouvelle copie de la liste et cette fois, Compose détectera le changement et rafraîchira l'écran.


```kotlin title="Kotlin"
Button(
    onClick = {
        val tempo = heures.value.toMutableList()
        tempo.add(LocalDateTime.now().toString())
        heures.value = tempo    // la réaffectation de la liste cause le rafraîchissement
    }
) {
    Text(text = "Ajouter")
}
```


Autre syntaxe équivalente que vous rencontrerez souvent en Kotlin :


```kotlin title="Kotlin"
val tempo = heures.value.toMutableList().apply { add(LocalDateTime.now().toString()) }
heures.value = tempo   // la réaffectation de la liste cause le rafraîchissement
```


ou mieux :


```kotlin title="Kotlin"
heures = heures.value.toMutableList().apply { add(LocalDateTime.now().toString()) }
```


### Version avec la syntaxe « by remember »


Dans l'exemple précédent, la variable d'état n'a pas été créée à l'aide du mot-clé by, ce qui exige l'utilisation de .value pour accéder à sa valeur.


Le code aurait également pu être écrit comme suit.


Remarquez :


#### l'ajout du by


#### l'ajout des deux import


#### le retrait du MutableState<> lors de la déclaration de la variable d'état


#### le retrait du .value lors de son utilisation


#### cette fois, il faut déclarer la variable avec var puisqu'elle sera réassignée (la syntaxe sans le by réassignait
heures.value).


```kotlin title="Kotlin"
import androidx.compose.runtime.getValue
import androidx.compose.runtime.setValue
...
@Composable
fun MainScreen() {
    var heures: MutableList<String> by remember {
        mutableStateOf(mutableListOf())
    }
 
    Column {
        Button(
            onClick = {
                val tempo = heures.toMutableList().apply { add(LocalDateTime.now().toString()) }
                heures = tempo   // la réaffectation de la liste cause le rafraîchissement
            }
        ) {
            Text(text = "Ajouter")
        }
        AfficherItems(heures)
    }
}
@Composable
private fun AfficherItems(items: List<String>) {
    Column {
        for (item in items) {
            Text(text = item)
        }
    }
}
```


### Version avec la syntaxe « var (heures, setHeures) »


Pour compléter l'étude de ce concept, je vous présente une autre version qui utilise une syntaxe différente.


En effet, selon la documentation officielle de Android
 :


### Il existe trois façons de déclarer un objet MutableState dans un composable :


### val mutableState = remember { mutableStateOf(default) }


### var value by remember { mutableStateOf(default) }
val (value, setValue) = remember { mutableStateOf(default) }


Cette fois, le code déclare directement un modificateur (setter) et s'en sert pour effectuer une réaffectation de la liste, ce qui force le rafraîchissement de l'écran.


```kotlin title="Kotlin"
@Composable
fun MainScreen() {
    val (heures, setHeures) = remember {
        mutableStateOf(mutableListOf<String>())
    }
    Column {
        Button(
            onClick = {
                setHeures(heures.toMutableList().apply { add(LocalDateTime.now().toString()) })
            }
        ) {
            Text(text = "Ajouter")
        }
        AfficherItems(heures)
    }
}
@Composable
private fun AfficherItems(items: List<String>) {
    Column {
        for (item in items) {
            Text(text = item)
        }
    }
}
```


> **Source** : 

## 1. * [« État et Jetpack Compose » - Android Developers](https://developer.android.com/jetpack/compose/state?hl=fr)


## 2. * [« État et Jetpack Compose » - Android Developers](https://developer.android.com/jetpack/compose/state?hl=fr)


#### Pour plus d'information


* [« Can I use State<ArrayList> or State<mutableListOf()> for observed by Compose to trigger recomposition when they change? » - Stack Overflow](https://stackoverflow.com/questions/71151322/can-i-use-statearraylistt-or-statemutablelistof-for-observed-by-compose)


### * [« There are three ways to declare a MutableState object in a composable » - Reddit](https://www.reddit.com/r/androiddev/comments/rqdv5g/there_are_three_ways_to_declare_a_mutablestate/)
80. Optimiser l'application



---
