---
title: "Lecture audio et sons MP3"
---

# Lecture audio et sons MP3


### 47.1 Media3 ExoPlayer


Je vous démontre ici comment ajouter du son à une application Android avec Jetpack Compose.


Je vais créer une petite application qui joue une note de musique quand on appuie sur une touche.


J'utilise Media3 ExoPlayer tel que recommandé par Android
. Il s'agit d'une bibliothèque plus intéressante que le traditionnel MediaPlayer.


### Dépendances


D'abord, il faut ajouter des dépendances dans le fichier  build.gradle.kts  qui se trouve dans le dossier  app .


```kotlin title="Fichier app/build.gradle.kts"
...
dependencies {
    ...
    // pour ExoPlayer
    implementation("androidx.media3:media3-exoplayer:1.8.0")
    implementation("androidx.media3:media3-common:1.8.0")
    implementation("androidx.media3:media3-ui:1.8.0")   // requis seulement si on a besoin de créer les contrôles de lecture
habituels (Play/Pause/Stop)
}
```


Une fois la dépendance ajoutée, il faut **resynchroniser le projet pour qu'il tienne compte de
l'ajout**.


### Ajouter les fichiers de son au projet


Les fichiers MP3 qui seront utilisés doivent être placés dans le dossier monProjet/app/src/main/res/raw .


#### D'abord, donne un nom significatif à chacun des fichiers MP3. Le nom du fichier sera utilisé dans le code en tant
qu'ID de ressource.


#### Ce nom doit être en entièrement en minuscules.


#### Il doit commencer par une lettre et ne doit pas contenir d'espaces ni de caractères spéciaux.


#### Les barres de soulignement sont autorisées (ex : message_recu.mp3)


#### Ce ne doit pas non plus être un mot réservé. Par exemple. do.mp3 n'est pas un nom acceptable.


#### Dans Android Studio, faites un clic droit sur le dossier res et choisissez New / Android Resource Directory .


#### Nommez le dossier raw et associez-le au type de ressource raw .


#### Faites glisser les fichiers dans ce dossier à partir de votre système de fichiers.


### ExoPlayer


Dans le code, il faut instancier au moins un ExoPlayer.


Puisque chaque son à jouer doit avoir son propre ExoPlayer, il est intéressant de placer le code dans sa propre fonction.


```kotlin title="Jetpack Compose (Kotlin)"
/**
 * Initialise un ExoPlayer avec un son.
 *
 * @param context Le contexte.
 * @param media Le nom du fichier MP3 sans l'extension. Le fichier doit être dans le dossier res/raw.
 */
fun initialiserExoPlayer(context: Context, media: String): ExoPlayer {
    val mediaUri = "android.resource://${context.packageName}/raw/$media"
    val mediaItem = MediaItem.fromUri(mediaUri)
    return ExoPlayer.Builder(context).build().apply {
        setMediaItem(mediaItem)
        prepare()
    }
}
```


### Sons en ligne


Il est également possible de travailler avec des sons disponibles directement en ligne.


Il faudra d'abord demander la permission d'utiliser une ressource en ligne en ajoutant cette balise uses-permission
 dans le fichier AndroidManifest.xml que l'on
retrouve dans le dossier app/src/main .


Sans cette permission, le son ne sera jamais joué et on obtiendra ce message dans le logcat : « Unexpected exception loading stream ».


Remarquez que l'usager n'aura pas à donner son accord avant d'accéder à Internet. La permission est une simple déclaration dans le manifeste.


```xml title="Fichier AndroidManifest.xml"
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          xmlns:tools="http://schemas.android.com/tools">
     <uses-permission android:name="android.permission.INTERNET" />
    <application
            ...
    </application>
</manifest>
```


```kotlin title="Jetpack Compose (Kotlin)"
/**
 * Initialise un ExoPlayer avec un son en ligne.
 *
 * @param context Le contexte.
 * @param url L'URL du fichier mp3 en ligne.
 */
fun initialiserExoPlayerEnLigne(context: Context, url: String): ExoPlayer {
    val mediaUri = Uri.parse(url)
    val mediaItem = MediaItem.fromUri(mediaUri)
    return ExoPlayer.Builder(context).build().apply {
        setMediaItem(mediaItem)
        prepare()
    }
}
```


### Instancier et libérer les objets ExoPlayer


Pour instancier l'ExoPlayer qui sera rattaché à un son, qu'il soit en ligne ou non, on aura besoin du contexte de l'application.


On prendra soin de libérer les ressources lorsqu'elles ne sont plus utilisées.


Remarquez qu'ici, l'utilisation de remember est tout à fait correcte puisqu'un si on avait utilisé **rememberSaveable]**, l'objet ExoPlayer n'aurait pas pu être conservé lors de la destruction de l'activité
étant donné qu'il n'est pas sérialisable.


```kotlin title="Jetpack Compose (Kotlin)"
@Composable
fun MainScreen(modifier: Modifier = Modifier) {
    val context = LocalContext.current
    val playerDo = remember { initialiserExoPlayer(context, "son_do") }
    val playerRe = remember { initialiserExoPlayer(context, "son_re") }
    val playerMi = remember { initialiserExoPlayer(context, "son_mi") }
    val playerEnLigne = remember { initialiserExoPlayerEnLigne(context, "https://...") }
    DisposableEffect(Unit) {
        onDispose {
            playerDo.release()
            playerRe.release()
            playerMi.release()
            playerEnLigne.release()
        }
    }
    ...
}
```


### Faire jouer le son


Faire jouer le son peut prendre différentes formes.


Par exemple, si le son est très court, il suffit d'appeler la méthode play. Par contre, il faudra prendre soin de remettre le pointeur de lectuer au début du son pour
permettre de le jouer à nouveau.


```kotlin title="Jetpack Compose (Kotlin)"
/**
 * Fait jouer le son.
 *
 * @param player Objet ExoPlayer rattaché au son à jouer.
 */
fun jouer(player: ExoPlayer) {
    player.seekTo(0)   // remet le son au début pour permettre de le jouer à nouveau
    player.play()
}
```


Dans le cas d'un fichier de son plus long, on pourra avoir quelque chose comme suit :


```kotlin title="Jetpack Compose (Kotlin)"
/**
 * Fait jouer le son.
 *
 * @param player Objet ExoPlayer rattaché au son à jouer.
 */
fun jouer(player: ExoPlayer) {
    if (player.isPlaying) {
        player.stop()   // si on clique pendant que le son joue, on l'arrête
        player.prepare()   // sans ceci, le son ne serait pas prêt à être joué lors du prochain appel à play()
    } else {
        player.play()
    }
}
```


On peut maintenant appeler cette fonction.


```kotlin title="Jetpack Compose (Kotlin)"
Button(
    onClick = {
        jouer(playerMi)
    }
) {
    Text(
        text = "Mi",
    )
}
```


## 48. Exercice 7



---
