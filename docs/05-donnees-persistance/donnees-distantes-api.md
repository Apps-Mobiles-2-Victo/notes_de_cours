---
title: "Données distantes et consommation d'API REST"
---

# Données distantes et consommation d'API REST


## 72.1 Application qui se sert d'informations distantes


Dans un monde idéal, une application mobile travaillera avec des données locales afin de pouvoir fonctionner même si elle n'a pas accès à Internet.


De plus, elle synchronisera ces données avec une base de données distante dès qu'elle aura accès à Internet afin d'assurer de ne rien perdre en cas de bris.


Vous pouvez cependant développer une application qui n'utilise qu'une base de données distante si votre application répond à l'une de ces conditions :


- L'application que vous désirez développer n'a pas besoin d'être fonctionnelle en tout temps (donc c'est OK qu'elle ne fonctionne pas hors réseau)
- Vous savez que vos utilisateurs s'en serviront seulement lorsqu'ils ont accès à Internet
- Les données distantes ne sont pas centrales dans l'application. L'application pourra donc fonctionner partiellement sans ces données.


Vous désirez coder une telle application? Suivez ces étapes!


Dans cette fiche :


- [Ajout de dépendances](#ajout-de-dépendances)
- [Permission d'accéder au réseau](#permission-daccéder-au-réseau)
- [Classe pour représenter les données reçues de l'API](#classe-pour-représenter-les-données-reçues-de-lapi)
- [Interface pour accéder à l'API](#interface-pour-accéder-à-lapi)
- [Instance Retrofit](#instance-retrofit)
- [Effectuer un appel à l'API](#effectuer-un-appel-à-lapi)
- [Utiliser les données de l'API dans un composable](#utiliser-les-données-de-lapi-dans-un-composable)
- [Cas des API qui peuvent retourner différents types de données selon la réussite ou l'erreur](#cas-des-api-qui-peuvent-retourner-différents-types-de-données-selon-la-réussite-ou-lerreur)
- [Erreur « Unable to resolve host "....com": No address associated with hostname »](#erreur-unable-to-resolve-host-com-no-address-associated-with-hostname)


### Ajout de dépendances


Pour permettre l'utilisation d'un API qui permettra d'accéder aux données distantes, il faut ajouter des dépendances à votre projet.  Nous utiliserons Retrofit 2, une librairie qui facilite l'appel d'API REST.



Ces lignes doivent être ajoutées dans le fichier *build.gradle.kts*  qui se trouve dans le dossier app .


```kotlin title="Fichier app/build.gradle.kts"
...
dependencies {
    ...
  // Pour appel API
  implementation("com.squareup.retrofit2:retrofit:3.0.0")
  implementation("com.squareup.retrofit2:converter-gson:3.0.0")
}
```


Une fois les dépendances ajoutées, il faut **resynchroniser le projet pour qu'il tienne compte de l'ajout**.


**Note** : si vous obtenez un message du genre « Unresolved reference retrofit2 », rendez-vous dans le menu File / Invalidate Caches .


### Permission d'accéder au réseau


Pour qu'une application Android puisse utiliser une ressource en ligne, il faut ajouter une balise *uses-permission* dans le fichier *AndroidManifest.xml* que l'on retrouve dans le dossier *app/src/main*.



Sans cette permission, le programme se terminera avec le message « Permission denied (missing INTERNET permission?) ».


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


De plus, si vous travaillez avec un URL non sécurisé (http://) pendant le développement, vous devrez le préciser comme suit.

**Important** : il faut remettre cette configuration à false lorsque l'application sera en production.

```xml title="Fichier AndroidManifest.xml"
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          xmlns:tools="http://schemas.android.com/tools">
    ...
    <application
         android:usesCleartextTraffic="true"
        ...
    </application>
</manifest>
```


### Classe pour représenter les données reçues de l'API


Lorsque l'application fera un appel à l'API, elle stockera les données reçues dans une instance d'une classe spécialisée.


Cette classe, qui est une **classe de données**, sera placée dans un dossier nommé  data  qui est au même niveau que le fichier MainActivity.kt , par exemple *app/src/main/java/com/monnom/monprojet/data/Item.kt*.


Pour créer ce dossier dans Android Studio : Clic droit sur son dossier parent /  New  /  Package .


La classe doit avoir une propriété pour chacune des informations reçues. Le nom d'une propriété doit correspondre à une clé JSON reçue.


Par exemple, si l'API retourne un item dont les données sont au format : 

```
{"id": 3, 
 "titre": "abc"}
```

vous devez déclarer une classe avec les propriétés suivantes :


```kotlin title="Fichier data/Item.kt"
data class Item(
  var id : Int?,   // optionnel car on ne le spécifiera pas lors de l'ajout d'un item
  var titre : String,
)
```


La conversion des données JSON retournées par l'API en objets Kotlin utilisés par l'application sera automatisée grâce à l'instruction *addConverterFactory(GsonConverterFactory.create())* dans l'instance Retrofit que nous créerons plus bas.


Notez que si l'API retourne des informations dont la clé n'a pas de propriété correspondante, ces informations ne seront simplement pas traitées par l'application.


Inversement, si la classe contient des propriétés qui ne sont pas retournées par l'API, ces propriétés auront toujours la valeur null.


Mais attention : si vous devez envoyer des données de ce type dans le corps de la requête, par exemple pour ajouter une donnée, une erreur dans le nom des champs des données attendues par l'API générera une erreur 404 (ou 500 selon la façon dont l'API a été programmée).


### Interface pour accéder à l'API


Il faut créer une interface qui fera le lien entre l'application et l'API.


Cette interface sera codée dans un fichier dont le nom se termine par Api, placé dans le dossier service  qui est au même niveau que le fichier *MainActivity.kt* , par exemple *app/src/main/java/com/monnom/monprojet/service/ItemApi.kt*.


L'interface doit définir, pour chaque type de requête à réaliser :


- le verbe HTTP (@GET, @POST, @PUT, @PATCH, @DELETE)
- le point d'accès (endpoint), c'est-à-dire la partie qui suit l'URL de base de la requête à exécuter.
    - Par exemple, si on appelle l'API *https://monapi.com/v1/items* , le point d'accès est items. Avec l'API *https://monapi.com/v1/ajouter.php* , le point d'accès est ajouter.php.
    - Pour un URL qui contient des paramètres dans son chemin , par exemple *https://monapi.com/v1/items/12* , le point d'accès est *items/{id}* . Les paramètres du chemin seront identifiés à l'aide de l'annotation @Path (voir exemple plus bas).
    - Dans le cas où l'URL utilise des paramètres de requête, par exemple *https://monapi.com/v1/items?id=12* , le point d'accès est *items*. Les paramètres de requête seront identifiés à l'aide de l'annotation @Query (voir exemple plus bas).
- le nom de la fonction que l'application utilisera pour appeler l'API, ses paramètres et son type de retour. La fonction est déclarée sans corps : Retrofit effectue la requête lorsqu'elle est appelée, selon le point d'accès défini dans l'interface.
    - Notez que si le type de retour est Response<...> , il sera possible de retrouver le code d'état HTTP retourné par l'API.
- s'il y a lieu, les données à envoyer dans le corps du message, identifiées avec @Body.


Voici un exemple d'interface qui définit quelques appels. Le premier permet de retrouver tous les items et le second, un seul item retrouvé par son identifiant, passé comme paramètre de requête (ex : ?id=12).


Le nom du paramètre n'a pas d'importance. Je l'ai appelé identifiant pour illustrer que ça n'a pas besoin d'être le même nom que dans l'API. Sa valeur sera spécifiée lors de l'appel à cette fonction (voir correspondance de couleur plus bas).


Un troisième appel permet d'ajouter un item dans la base de données distante.


*Attention* : si vous effectuez le mauvais import pour la classe Retrofit, vous obtiendrez une erreur du genre « No type arguments expected for class Response : Closeable. ».


```kotlin title="Fichier service/ItemApi.kt"
import retrofit2.Response
import retrofit2.http.GET
...
interface ItemApi {
    @GET("liste")
    suspend fun retrouverItems(): Response<List<Item>>
    @GET("liste")
    suspend fun retrouverUnItem(@Query("id") identifiant: Int): Response<Item>
    @POST("ajout")
    suspend fun ajouterItem(@Body item: Item): Response<Void>
}
```


Si on avait utilisé un API qui utilise des paramètres de chemin, la seconde requête aurait pris cette forme :


```kotlin title="Fichier service/ItemApi.kt"
@GET("liste/{id} ")
suspend fun retrouverUnItem( @Path("id") identifiant : Int): Response<Item>
```


*Note* : si vous effectuez des recherches sur le Web ou dans des anciens projets, vous rencontrerez parfois des instructions du genre :

```kotlin
fun retrouverUnItem(@Path("id") identifiant: Int): Call<Item>.
```

Ce type de code était utilisé avant l'arrivée des coroutine de Kotlin. Bien qu'il fonctionne encore, il est préférable d'utiliser l'approche avec coroutines (avec le mot-clé suspend, tel qu'illustré plus haut) puisque le code sera plus facile à écrire, à lire et à maintenir.


**Dans le cadre de ce cours, la forme avec Call est interdite.**


### Instance Retrofit


L'application travaillera avec une seule instance de Retrofit.


L'instanciation sera codée dans le fichier service/RetrofitInstance.kt .


On utilisera le mot-clé *object* et non *class*. En Kotlin, le mot-clé object permet de déclarer une classe et d'instancier un singleton de cette classe, tout ça en une seule étape.


Il s'agit de spécifier l'URL de base, d'effectuer l'instanciation en tant que telle et de faire le lien avec l'interface (fichier créé plus tôt, dont le nom se termine par Api).


```kotlin title="Fichier service/RetrofitInstance.kt"
import retrofit2.Retrofit
import retrofit2.converter.gson.GsonConverterFactory
...
object RetrofitInstance {
    private const val BASE_URL = "https://monapi.com/v1/"
    val itemApi: ItemApi = Retrofit.Builder()
        .baseUrl(BASE_URL)
        .addConverterFactory(GsonConverterFactory.create())
        .build()
        .create(ItemApi::class.java)
}
```


Dans cet extrait de code, l'instruction *addConverterFactory(GsonConverterFactory.create())* permet d'automatiser la conversion de données JSON en objets Kotlin et vice versa.


*itemApi* est créée une seule fois, au premier accès à *RetrofitInstance*.


### Effectuer un appel à l'API


Il est maintenant temps de coder les fonctions qui permettent d'appeler l'API. Ces fonctions, qui constituent la logique métier, seront codées dans le ViewModel.


Je leur ai volontairement donné des noms différents de ceux spécifiés dans l'interface afin de mieux illustrer qu'est-ce qui fait quoi.


Chacune de ces fonctions fera appel à la fonction dont le nom a été spécifié dans l'interface. Une fois l'appel réalisé, elle stockera dans une variable d'état les informations retournées par l'API .


Remarquez que dans cet extrait, il n'est pas utile de déclarer le uiState comme un flux puisque l'application n'écoute pas pour recevoir les modifications aux données distantes.

Remarquez l'utilisation de *private set* qui rend la propriété immuable.


```kotlin title="Fichier ui/HomeViewModel.kt"
class HomeViewModel : ViewModel() {
    var uiState = mutableStateOf(HomeUiState())
        private set
    init {
       rechercherItems()
    }
    fun rechercherItems() {
        viewModelScope.launch {
            try {
                val reponse = RetrofitInstance.itemApi.retrouverItems()
                if (reponse.isSuccessful) {
                    uiState.value = uiState.value.copy(
                         _listeItems = reponse.body() ,
                        ...    // on a accès à reponse.code() pour retrouver le code d'état HTTP retourné par l'API
                    )
                }
                else {
                    ...
                }
            } catch (e: Exception) {
                ...
            }
        }
    }
    fun rechercherUnItem (id: Int) {
        ...
        val reponse = RetrofitInstance.itemApi.retrouverUnItem(id)
        ...
    }
    fun insererItem(item: Item) {
        ...
        val reponse = RetrofitInstance.itemApi.ajouterItem(item)
        ...
    }
}
data class HomeUiState(
    private val _listeItems: List<Item>? = listOf(),
    private val _unItem: Item? = null,
    ...
) {
    ...
}
```


Un composable pourra alors appeler une méthode du ViewModel pour réaliser l'appel.


```kotlin title="Jetpack Compose (Kotlin)"
@Composable
fun AfficherItem(viewModel: HomeViewModel, id: Int) {
    Button(
        onClick = {
            viewModel.rechercherUnItem(id)
        }
    ) {
        Text(text = "Rechercher")
    }
    ...
}
```


### Utiliser les données de l'API dans un composable


Une fois l'appel à l'API complété (et les informations stockées dans le ViewModel), un composable pourra afficher les données du ViewModel.


Remarquez la syntaxe pour initialiser le uiState lorsqu'on ne travaille pas avec un flux.




```kotlin title="Jetpack Compose (Kotlin)"
@Composable
fun UnItem(viewModel: HomeViewModel) {
    val uiState by viewModel.uiState
    ...
    Text(text = uiState.unItem?.id?.toString() ?: "---")
    ...
}
```


### Cas des API qui peuvent retourner différents types de données selon la réussite ou l'erreur


Prenons le cas d'un API qui retourne un tableau d'items quand la requête fonctionne ou un message d'erreur si un problème survient.


Ce message d'erreur, initialisé par l'API, est plus précis qu'un simple *Not Found* ou *Internal Server Error*.


Pour avoir accès à ce message, il faut d'abord créer une classe dont les propriétés correspondent aux clés JSON reçues.


```kotlin title="Fichier data/ReponseAvecMessage.kt"
data class ReponseAvecMessage (
    var message: String? = null,
)
```


Dans le ViewModel, il sera possible de retrouver le message d'erreur retourné par l'API à l'aide de manipulations JSON.


```kotlin title="Fichier ui/HomeViewModel.kt"
import com.google.gson.Gson

fun retrouverItems() {
    viewModelScope.launch {
        try {
            val reponse = RetrofitInstance.itemApi.retrouverItems()
            if (reponse.isSuccessful) {
                ...
            }
            else {
                val gson = Gson()
                val reponseAvecMessage: ReponseAvecMessage? = gson.fromJson(
                    reponse.errorBody()?.charStream()?.readText(),
                    ReponseAvecMessage::class.java
                )
                val message = reponseAvecMessage?.message ?: "Aucun message"
                ...
            }
        } catch (e: Exception) {
            ...
        }
    }
}
```


### Erreur « Unable to resolve host "....com": No address associated with hostname »


L'erreur «Erreur « Unable to resolve host "....com": No address associated with hostname » indique qu'il y a un problème avec le serveur DNS qui doit traduire un URL en adresse IP.


Cette erreur est généralement silencieuse. Vous la verrez seulement si vous avez pris soin de réagir à un problème lors de l'appel de l'API.


```kotlin title="ViewModel (Kotlin)"
try {
    val reponse = RetrofitInstance.itemApi.retrouverItems()
    ...
} catch (e: Exception) {
    Log.d("****** ViewModel", "Erreur lors de l'appel de retrouverItems() : ${e.message}")
    ...
}
```


Si vous voyez cette erreur, commencez par vérifier si l'URL est exact à l'aide d'un navigateur Web ou d'un testeur de requêtes REST comme **Postman**, Bruno ou curl . Vous devez concaténer la valeur de la constante BASE_URL avec le point d'accès précisé à la suite du @GET ou du @POST (ex :  *https://monapi.com/v1/liste* ).


Si l'URL est exact, l'erreur pourrait être due à un problème avec l'émulateur. Ceci arrive parfois si on utilise l'émulateur dans différents réseaux, par exemple à l'école et à la maison.


Pour régler ce problème :


- Rendez-vous dans le menu  View  /  Tool Windows  /  Device Manager .
- Parfois, un simple redémarrage de l'émulateur fonctionne. Cliquez sur Stop vis-à-vis l'émulateur que vous utilisez.
- Cliquez ensuite sur Start puis relancez votre application.
- Il peut arriver qu'une action plus costaude soit nécessaire. Toujours dans Device Manager, cliquez sur les trois points
verticaux vis-à-vis l'émulateur que vous utilisez puis choisissez Wipe Data . Relancez ensuite votre application. 


## 72.2 Appeler deux API dans la même application

Si votre application Jetpack Compose a besoin de faire appel à deux API :

- Chaque API aura sa propre **classe pour représenter les données reçues de l'API**.
- Chaque API aura sa propre **Interface pour accéder à l'API**.
- Chaque API aura sa propre instance Retrofit si elle utilise une URL de base différente.


On crée ici une instance Retrofit par API, chacune avec sa propre URL de base.


```kotlin title="Fichier service/RetrofitInstance.kt"
object RetrofitInstance {
    
    private const val BASE_URL_ABC = "https://.../"
    val abcApi: AbcApi = Retrofit.Builder()
        .baseUrl(BASE_URL_ABC)
        .addConverterFactory(GsonConverterFactory.create())
        .build()
        .create(AbcApi::class.java)

    private const val BASE_URL_DEF = "https://.../"
    val defApi: DefApi = Retrofit.Builder()
        .baseUrl(BASE_URL_DEF)
        .addConverterFactory(GsonConverterFactory.create())
        .build()
        .create(DefApi::class.java)
}
```


On pourra faire appel à l'un ou l'autre de ces API en utilisant la propriété correspondante :


```kotlin title="Jetpack Compose (Kotlin)"
val reponse = RetrofitInstance.abcApi.retrouverDonnees()
```

