---
title: "Effets secondaires (Side Effects)"
---

# Effets secondaires (Side Effects)


## Qu'est-ce qu'un effet secondaire?


Selon la documentation Android :


```
Un effet secondaire est un changement d'état de l'application qui se produit en dehors du champ d'application d'une fonction composable.
```

Par exemple, on pourrait utiliser l'effet secondaire **LaunchedEffect()** qui permet d'appeler une fonction asynchrone lorsqu'une condition survient, par exemple lors du chargement initial d'un composable ou encore lorsqu'une variable d'état change de valeur.


Autre exemple : l'effet secondaire **DisposableEffect()** permet d'effectuer des tâches de nettoyage à certaines étapes du cycle de vie.


**Source** : 

* [« Effets secondaires dans Compose » - Android Developer](https://developer.android.com/develop/ui/compose/side-effects?hl=fr)


## LaunchedEffect


L'effet secondaire LaunchedEffect() permet d'effectuer un appel asynchrone lors du premier chargement d'un composable ou encore à chaque fois que la valeur de sa clé est modifiée.


```kotlin title="Jetpack Compose (Kotlin)"
@Composable
fun MainScreen() {
    var nom by rememberSaveable { mutableStateOf("nom par défaut") }
    // ceci sera effectué seulement lors du premier chargement de MainScreen
    LaunchedEffect( Unit ) {
        delay(100) // pour illustrer que LaunchedEffect peut appeler des fonctions asynchrones
        Log.d("MainActivity", "Ceci est fait seulement la première fois. Votre nom : $nom")
    }
    // ceci sera effectué lors du premier chargement de même qu'à chaque fois que nom change de valeur
    LaunchedEffect( nom ) {
        delay(100)
        Log.d("MainActivity", "Ceci est fait à chaque changement de la variable d'état 'nom'. Votre nom : $nom")
    }
    Column {
        TextField(
            value = nom,
            onValueChange = { nom = it },
            label = { Text("Votre nom") }
        )
    }
}
```


#### Pour plus d'information


* [« Jetpack Compose Side Effects — LaunchedEffect With Example » - Medium](https://betterprogramming.pub/jetpack-compose-side-effects-launchedeffect-with-example-99c2f51ff463)


* [« How to Use Render Effects in Jetpack Compose for Stunning Visuals » - Canopas](https://canopas.com/how-to-use-render-effects-in-jetpack-compose-for-stunning-visuals-01287d7f00db)