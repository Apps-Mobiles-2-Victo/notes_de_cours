---
title: "Bases de Jetpack Compose"
---

# Bases de Jetpack Compose


### 15.1 Qu'est-ce que Jetpack Compose?


Jetpack Compose est une boîte d'outils qui permet de définir des interfaces utilisateur (UI) pour applications Android écrites avec Kotlin.


Jetpack Compose implémente Material Design
, une bibliothèque spécialisée pour bâtir des interfaces utilisateur.


Auparavant, les interfaces étaient bâties à l'aide de code XML. Avec Jetpack Compose, l'interface sera décrite par programmation à l'aide de **fonctions modulables**.


Si vous avez déjà programmé des applications mobiles pour iPhone avec SwiftUI ou des applications pour iPhone ou Android avec Flutter, vous trouverez plusieurs ressemblances entre ces technologies.


À titre d'exemple, voici une fonction modulable qui permet d'afficher le mot Hello suivi d'une information reçue en paramètre. Chaque fonction modulable est en fait
un élément graphique.


```kotlin title="Kotlin"
@Composable
fun Greeting(name: String) {
    Text(text = "Hello $name!", fontWeight = FontWeight.Bold)
}
```


#### Pour plus d'information


* [« Créez de meilleures applications plus rapidement avec Jetpack Compose » - Android Developers](https://developer.android.com/jetpack/compose?hl=fr)


* [« Tutoriel Jetpack Compose » - Android Developers](https://developer.android.com/jetpack/compose/tutorial?hl=fr)


* [« Qu’est ce que Jetpack Compose ? Conseils, » - mobiskill](https://mobiskill.fr/blog/conseils-emploi-tech/quest-ce-que-jetpack-compose/)


* [« Flutter est mort; Vive Jetpack Compose. » - Kossi Mathias KALIPE](https://fr.linkedin.com/pulse/flutter-est-mort-vive-jetpack-compose-mathias-kalipe-)


* [« The Ultimate Jetpack Compose Cheat Sheet » - HackerNoon](https://hackernoon.com/the-ultimate-jetpack-compose-cheat-sheet)


* [« Introducing the Compose Material Catalog » - Material Design Blog](https://material.io/blog/jetpack-compose-catalog)


### 15.2 Les fonctions modulables


Avec Jetpack Compose, tout ce qui est affiché à l'écran est défini dans une fonction modulable, aussi appelée fonction composable ou simplement composable.


Il s'agit d'une fonction précédée par l'annotation @Composable. Cette fonction appelle généralement d'autres fonctions modulables, par exemple Text() ou Image().


Chaque fonction modulable est en fait un élément graphique.


À titre d'exemple, lors de la création initiale d'un projet, une fonction modulable est définie pour afficher le mot Hello suivi d'une information reçue en paramètre.


```kotlin title="Kotlin"
@Composable
fun Greeting(name: String) {
    Text(text = "Hello $name!")
}
```


#### Pour plus d'information


* [« Lifecycle of composables » - Android Developer](https://developer.android.com/develop/ui/compose/lifecycle)


* [« Compose layout basics » - Android Developer](https://developer.android.com/develop/ui/compose/layouts/basics)


* [« Thinking in Compose » - Android Developer](https://developer.android.com/develop/ui/compose/mental-model)


### 15.3 Par où commence le code de mon application?


Afin de bien comprendre où le code doit être placé, il faut avoir une vue globale du fonctionnement d'une application Android bâtie avec Kotlin et Jetpack Compose.


### Fichier AndroidManifest.xml


C'est ce fichier qui détermine quel autre fichier démarrera l'application.


Voici son code initial. On y voit que la classe de départ
 s'appelle MainActivity. Le point qui précède ce nom indique que la classe fait partie de l'espace de nom
spécifié dans le fichier  app/build.gradle.kts .


```xml title="Fichier AndroidManifest.xml"
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">
    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.HelloWorld"
        tools:targetApi="31">
        <activity
             android:name=" .MainActivity "
             android:exported="true"
             android:label="@string/app_name"
             android:theme="@style/Theme.HelloWorld">
             <intent-filter>
                 <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
             </intent-filter>
        </activity>
    </application>
</manifest>
```


Et voici le code du fichier qui définit cette classe.


```kotlin title="Fichier MainActivity.kt (Kotlin)"
package com.mondomaine.helloworld
import ...
class MainActivity : ComponentActivity () {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContent {
            HelloWorldTheme {
                 Scaffold (modifier = Modifier.fillMaxSize()) { innerPadding ->
                    Greeting (
                        name = "Android"
                        modifier = Modifier.padding(innerPadding)
                    )
                }
            }
        }
    }
}
@Composable
fun Greeting (name: String, modifier: Modifier = Modifier) {
    Text(
        text = "Hello $name!",
        modifier = modifier
    )
}
@Preview (showBackground = true)
@Composable
fun GreetingPreview() {
    HelloWorldTheme {
        Greeting("Android")
    }
}
```


Quelques explications :

- La classe MainActivity hérite de ComponentActivity (). Vous pouvez consulter le code de cette classe en faisant
-  Ctrl +Clic (Windows) ou  ⌘ Cmd +Clic (Mac) sur son nom.
- Dans son constructeur, on commence par exécuter le constructeur de la classe parent. On définit ensuite que l'application utilise le thème nommé HelloWorldTheme . Ce thème est défini dans le fichier
- `app/src/main/java/com.mondomaine.helloworld/ui.theme/Theme.kt` . On peut y accéder facilement en faisant Ctrl +Clic (Windows) ou  ⌘ Cmd +Clic (Mac) sur son nom.
- Le constructeur spécifie ensuite la structure de l'écran ( **Scaffold** ) et son contenu.
- Le contenu est défini par la fonction modulable *Greeting()* .
- Au bas du fichier, on remarque l'annotation @Preview . Ceci permet d'avoir un aperçu en temps réel de l'interface
utilisateur dans l'environnement de développement sans avoir à lancer l'application.


