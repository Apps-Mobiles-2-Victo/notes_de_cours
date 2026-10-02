---
title: "Base de données locale avec Room"
---

# Base de données locale avec Room


## Installation de Room


Room est une bibliothèque de persistance de données pour Android Jetpack Compose. Elle fournit une couche d'abstraction entre votre application et une base de données SQLite.


Pour utiliser Room, vous devez d'abord ajouter des dépendances au projet.


## Ajouts dans le fichier `build.gradle.kts` principal

La ligne à ajouter dépend de la version de Kotlin utilisée dans le projet.


## Retrouver la version de Kotlin du projet

Si votre projet utilise des catalogues de versions (présence d'un fichier `gradle/libs.versions.toml`), la version de Kotlin est disponible à cette ligne :


```toml title="Fichier libs.versions.toml"
[versions]
...
kotlin = "2.2.10"
```


Sinon, la version de Kotlin est disponible à cette ligne dans le build.gradle.kts principal :


```kotlin title="Fichier build.gradle.kts principal"
id ("org.jetbrains.kotlin.android") version "2.2.10" apply false
```


## Retrouver la version de KPS correspondante



Room nécessite l'utilisation de KPS (Kotlin Symbol Processing) pour générer le code nécessaire à l'interaction avec la base de données. Vous trouverez la liste des versions de KPS (Kotlin Symbol Processing) sur le site <https://github.com/google/ksp/releases>.


>Choisissez celle dont le numéro débute par votre numéro de version de Kotlin.


Par exemple, pour Kotlin 2.2.10, il faut utiliser KPS *2.2.10-2.0.2*.


## Ajout de KSP au fichier build.gradle.kts principal


Dans le fichier `build.gradle.kts` principal (aussi appelé top-level build.gradle file), soit celui présent directement à la racine du projet, ajoutez ceci en prenant soin d'utiliser la version de l'API KPS qui correspond à votre version de Kotlin.


```kotlin title="Fichier build.gradle.kts principal"
plugins {
    ...
    // KSP nécessaire pour Room
    // Utiliser la version qui correspond à la version de Kotlin : https://github.com/google/ksp/releases
    id ("com.google.devtools.ksp") version "2.2.10-2.0.2" apply false 
}
```


## Fichier gradle.properties

Vous devez ajouter la ligne suivante dans le fichier `gradle.properties` du projet (à la racine) :

`android.disallowKotlinSourceSets=false`

(symptôme: erreur de compilation liée à `kotlin.sourceSets`)

    Avant de poursuivre, il faut **resynchroniser le projet**.

## Ajouts dans le fichier build.gradle.kts du module


Dans le fichier `app/build.gradle.kts`, ajoutez ceci (utiliser la dernière version stable de Room disponible sur le site <https://developer.android.com/jetpack/androidx/releases/room>). :


```kotlin title="Fichier app/build.gradle.kts"
plugins {
    ...
    // pour Room avec KSP -> requiert une entrée dans le build.gradle.kts principal
    id ("com.google.devtools.ksp")
}
...
dependencies {
    ...
     // pour Room
    val room_version = "2.8.5"
    implementation("androidx.room:room-runtime:$room_version")
    implementation("androidx.room:room-ktx:$room_version")
    annotationProcessor("androidx.room:room-compiler:$room_version")
    ksp("androidx.room:room-compiler:$room_version")
    // fin pour Room
}
```

    Il faut **resynchroniser le projet** une fois les modifications apportées au fichier

Si le ksp() dans la dernière configuration apparaît en rouge, vérifiez si :

* Vous avez ajouté l'instruction requise dans le bloc plugin (voir au début de l'extrait pour le fichier `app/build.gradle.kts`).
* Vous avez utilisé la version qui correspond à votre version de Kotlin dans le fichier `build.gradle.kts` principal.
* Vous avez lancé la synchronisation (même si vous l'avez fait, il faut parfois **synchroniser le projet** à nouveau).


## Pour plus d'information


### * [« Enregistrer des données dans une base de données locale à l'aide de Room » - Android Developers](https://developer.android.com/training/data-storage/room?hl=fr) 


# Modèle pour représenter les données (classe d'entité)

Il est possible de générer vos tables dans une BD SQLite sans même avoir à utiliser du code SQL ni même un outil de gestion de base de données.

Chaque table sera définie dans une classe Kotlin précédée de l'annotation `@Entity`. On dira de cette classe que c'est une entité de données ou encore un modèle de données, parfois également appelée classe d'entité.

* Toutes les entités de données seront placées dans un dossier nommé `data`.
* Ce dossier sera au même niveau que le fichier `MainActiviy.kt`, par exemple `app/src/main/java/com/monnom/monprojet/data/Categorie.kt`.
* Pour créer ce dossier dans Android Studio : Clic droit sur son dossier parent /  New  /  Package .
* Par défaut, la table portera le même nom que la classe et chaque colonne de la table portera le même nom que le champ de la classe.
* Puisque la classe d'entité sert à définir des données, on lui ajoutera le mot-clé **data**.
* Les normes dictent que le nom de la classe doit être au singulier et utilise **la casse Pascal**.
    * Mais attention : le nom de la table doit être au pluriel et entièrement en lettres minuscules.


```kotlin title="Fichier data/Categorie.kt"
package com.monnom.monprojet.data

import androidx.room.Entity
import androidx.room.PrimaryKey

@Entity(tableName = "categories")
data class Categorie (
    @PrimaryKey(autoGenerate = true)
    val id : Int = 0,
    val titre: String = "",
    val description: String = "",
)
```


## Table avec clé étrangère


Pour une table qui comprend une clé étrangère :


```kotlin title="Fichier data/Item.kt"
package com.monnom.monprojet.data

import androidx.room.Entity
import androidx.room.PrimaryKey
import androidx.room.ForeignKey

@Entity(
    tableName = "items",
    foreignKeys = [ForeignKey(
        entity = Categorie::class,
        parentColumns = arrayOf("id"),
        childColumns = arrayOf("categorie_id"),
        onDelete = ForeignKey.CASCADE
    )]
)
data class Item(
    @PrimaryKey(autoGenerate = true)
    val id: Int = 0,
    val code: String = "",
    val titre: String = "",
    val description: String = "",
    val prix: Double = 0.0,
    val categorie_id : Int
)
```


### Pour plus d'information

* [« Définir des données à l'aide d'entités Room » - Android Developers](https://developer.android.com/training/data-storage/room/defining-data?hl=fr)
* [« Entity » - Android Developers](https://developer.android.com/reference/androidx/room/Entity)


# Le DAO : couche intermédiaire entre l'application et la BD

Plusieurs cadres d'application offrent une couche d'abstraction entre l'application et la base de données, généralement sous forme de classes qui représentent les tables de la BD. Cette couche d'abstraction est connue sous l'acronyme ORM (Object Relational Mapper).


Avec Jetpack Compose et Room, la couche d'abstraction utilise une interface DAO (Data Access Object ou objet d'accès aux données).


Grâce à la **classe d'entité**, Room est capable de générer lui-même les requêtes INSERT, UPDATE et DELETE pour gérer les données. Il suffit d'utiliser l'annotation appropriée (@Insert, @Update ou @Delete) et de passer une instance du modèle en paramètre à la fonction.


Ces fonctions doivent être exécutées sur leur propre fil d'exécution (thread) pour ne pas bloquer l'application. C'est pourquoi les fonctions doivent utiliser le mot-clé `suspend`.


Vous aurez besoin de requêtes SQL lorsque Room ne peut pas deviner vos besoins précis, par exemple pour les requêtes SELECT. À ce moment, la fonction utilisera l'annotation @Query. La fonction retournera l'information sous le type `Flow`, soit un flux de données asynchrone observable.


Le nom de l'interface du DAO – et du fichier – se terminera par `Dao`. Lorsque le DAO interagit avec une seule table, le nom sera sous la forme `<Entite>Dao`, par exemple `CategorieDao`.

Tous les DAO seront placés dans un dossier nommé `data`.


```kotlin title="Fichier data/CategorieDao.kt"
@Dao
interface CategorieDao {

    @Insert(onConflict = OnConflictStrategy.IGNORE)
    suspend fun insererCategorie(categorie: Categorie)

    @Update
    suspend fun mettreAJourCategorie(categorie: Categorie)

    @Delete
    suspend fun supprimerCategorie(categorie: Categorie)

    @Query("SELECT * FROM categories")
    fun listerCategories(): Flow<List<Categorie>>
}
```


## Ordre des enregistrements

Lorsqu'une requête peut retourner plus d'un enregistrement, il est important de spécifier dans quel ordre les enregistrements doivent être placés.


## Pour plus d'information


* [« Accéder aux données à l'aide des DAO Room » - Android Developers](https://developer.android.com/training/data-storage/room/accessing-data?hl=fr)


* [« Écrire des requêtes DAO asynchrones » - Android Developers](https://developer.android.com/training/data-storage/room/async-queries?hl=fr)


* [« Créer le DAO » - Android Developers](https://developer.android.com/codelabs/basic-android-kotlin-compose-persisting-data-room?hl=fr#5)


# Le dépôt de données (repository)


Un dépôt de données (repository) sert d'intermédiaire entre le ViewModel et les sources de données. Il peut, par exemple, combiner Room et une API ou appliquer des règles avant de transmettre les données.


Pour une petite application, un dépôt qui ne fait que relayer les appels du DAO est facultatif. L'exemple suivant utilise donc directement le DAO dans le ViewModel.


## Pour plus d'information


* [« Implémenter le dépôt » - Android Developers](https://developer.android.com/codelabs/basic-android-kotlin-compose-persisting-data-room?hl=fr#7)

# La classe qui hérite de RoomDatabase


Tous les  **DAO** seront réunis dans une classe qui représente la base de données en tant que telle.


Le code utilise le patron de conception du singleton, c'est-à-dire qu'il y aura un et un seul objet instancié.


La base de données peut porter n'importe quel nom. Une bonne pratique consiste à lui donner le même nom que l'application.


La classe qui définit la base de données de même que le fichier dans lequel elle est codée porteront un nom qui débute par le nom de la base de données et qui se termine par "Database". Ex : MonprojetDatabase.



Le fichier sera placé dans le dossier `data`.


La classe est abstraite parce que Room génère son implémentation concrète à la compilation, y compris les méthodes qui donnent accès aux DAO. L'appel à `build()` fournit une instance de cette implémentation.


```kotlin title="Fichier data/MonprojetDatabase.kt"
@Database(
    entities = [Categorie::class, Item::class],
    version = 1,
    exportSchema = false
)
abstract class MonprojetDatabase : RoomDatabase() {
    abstract fun categorieDao(): CategorieDao
    abstract fun itemDao(): ItemDao

    companion object {
        private var instance: MonprojetDatabase? = null

        fun getDatabase(context: Context): MonprojetDatabase =
            synchronized(this) {
                instance ?: Room.databaseBuilder(
                    context,
                    MonprojetDatabase::class.java,
                    "monprojet_database"
                ).build().also { instance = it }
            }
    }
}
```


## Pour plus d'information


* [« Créer une instance de base de données » - Android Developers](https://developer.android.com/codelabs/basic-android-kotlin-compose-persisting-data-room?hl=fr#6)

* [« Create ROOM Schema Export Directory » - Medium](https://medium.com/@vontonnie/create-room-schema-export-directory-7066d427eae8)


# Utiliser le DAO via le ViewModel


Le ViewModel obtient le DAO depuis la base de données et l'utilise pour lire ou modifier les données. Le fichier sera placé dans le dossier `ui`.


`AndroidViewModel` est une variante de `ViewModel` qui reçoit l'objet `Application` dans son constructeur. Le contexte de l'application permet ici d'obtenir la base de données; il ne faut pas lui transmettre le contexte d'un composable, qui peut être recréé.


Le DAO retourne un `Flow` pour la liste des catégories. Le ViewModel le transforme en `Flow<CategorieUiState>` avec `map`, puis Compose le collecte directement avec `collectAsState`. Il n'est pas nécessaire de le convertir en `StateFlow` avec `stateIn` lorsque Compose est son seul consommateur. Pour un état local qui ne dépend pas de Room, `mutableStateOf` suffit.


Les opérations d'écriture du DAO étant `suspend`, le ViewModel les appelle dans une coroutine.

```kotlin title="Fichier ui/CategorieViewModel.kt"
import kotlinx.coroutines.flow.Flow
import kotlinx.coroutines.flow.map

class CategorieViewModel(application: Application) : AndroidViewModel(application) {
    private val categorieDao = MonprojetDatabase.getDatabase(application).categorieDao()
    val uiState: Flow<CategorieUiState> = categorieDao.listerCategories()
        .map { categories -> CategorieUiState(categories) }

    fun insererCategorie(categorie: Categorie) = viewModelScope.launch {
        categorieDao.insererCategorie(categorie)
    }

    fun mettreAJourCategorie(categorie: Categorie) = viewModelScope.launch {
        categorieDao.mettreAJourCategorie(categorie)
    }

    fun supprimerCategorie(categorie: Categorie) = viewModelScope.launch {
        categorieDao.supprimerCategorie(categorie)
    }
}

data class CategorieUiState(
    val categories: List<Categorie> = emptyList()
)
```


Pour simplifier cet exemple, `AndroidViewModel` donne accès au contexte de l'application. Dans une application plus grande, on injecte généralement le DAO ou le dépôt dans un `ViewModel` afin de faciliter les tests.


La fonction `viewModel()` nécessite la dépendance Compose pour ViewModel 
* (`implementation("androidx.lifecycle:lifecycle-viewmodel-compose:2.11.0")` dans le fichier `app/build.gradle.kts`).
* importez `import androidx.lifecycle.viewmodel.compose.viewModel` dans le fichier du composable.


```kotlin title="Fichier MainActivity.kt"

import androidx.lifecycle.viewmodel.compose.viewModel

@Composable
fun MainScreen() {
    val categorieViewModel: CategorieViewModel = viewModel()
    MainContent(categorieViewModel)
}

@Composable
fun MainContent(categorieViewModel: CategorieViewModel) {
    val uiState by categorieViewModel.uiState.collectAsState(initial = CategorieUiState())
    Text(text = "Nombre de catégories : ${uiState.categories.size}")
}
```