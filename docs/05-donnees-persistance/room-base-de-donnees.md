---
title: "Base de données locale avec Room"
---

# Base de données locale avec Room


### 53.1 Installation de Room


Room
 est une bibliothèque de persistance de données pour Android Jetpack Compose. Elle fournit une couche d'abstraction entre votre application et une base
de données SQLite.


Pour utiliser Room, vous devez d'abord ajouter des dépendances au projet.


### Ajouts dans le fichier build.gradle.kts principal


La ligne à ajouter dépend de la version de Kotlin utilisée dans le projet.


### Retrouver la version de Kotlin du projet


Si votre projet utilise des catalogues de versions
 (présence d'un fichier gradle/libs.versions.toml ), la version de Kotlin est disponible à cette ligne :


```toml title="Fichier libs.versions.toml"
[versions]
...
kotlin = " 2.0.21 "
```


Sinon, la version de Kotlin est disponible à cette ligne dans le build.gradle.kts principal :


```kotlin title="Fichier build.gradle.kts principal"
id ("org.jetbrains.kotlin.android") version " 2.0.21 " apply false
```


### Retrouver la version de kps correspondante


Vous trouverez la liste des versions de kps (Kotlin Symbol Processing) sur le site https://github.com/google/ksp/releases
.


### Choisissez celle dont le numéro débute par votre numéro de version de Kotlin.


Par exemple, pour Kotlin 2.0.21, il faut utiliser KPS 2.0.21-1.0.28.


### Ajout au fichier


Dans le fichier build.gradle.kts  principal (aussi appelé top-level build.gradle file), soit celui présent directement à la racine du projet, ajoutez ceci en prenant
soin d'utiliser la version de l'API KPS qui correspond à votre version de Kotlin.


```kotlin title="Fichier build.gradle.kts principal"
plugins {
    ...
    // pour Room
    id ("com.google.devtools.ksp") version "2.0.21-1.0.28" apply false // utiliser la version qui correspond à la version de
Kotlin : https://github.com/google/ksp/releases
}
```


Avant de poursuivre, il faut **resynchroniser le projet**.


### Ajouts dans le fichier build.gradle.kts du module


Dans le fichier build.gradle.kts  qui se trouve dans le dossier app , ajoutez ceci :


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
    val room_version = "2.8.0"
    implementation("androidx.room:room-runtime: $room_version ")
     implementation("androidx.room:room-ktx: $room_version ")
    annotationProcessor("androidx.room:room-compiler: $room_version ")
    ksp("androidx.room:room-compiler: $room_version ")
    // fin pour Room
}
```


Si le ksp() dans la dernière configuration apparaît en rouge, vérifiez si :


#### Vous avez ajouté l'instruction requise dans le bloc plugin (voir au début de l'extrait pour le fichier
app/build.gradle.kts).


#### Dans le fichier build.gradle.kts principal, vous avez utilisé la version qui correspond à votre version de Kotlin.


#### Vous avez lancé la synchronisation (même si vous l'avez fait, il faut parfois **synchroniser le projet** à nouveau).


#### Pour plus d'information


### « Enregistrer des données dans une base de données locale à l'aide de Room ». Android Developers. https://developer.android.com/training/data-storage/room?
hl=fr
53.2 Modèle pour représenter les données (classe d'entité)


Il est possible de générer vos tables dans une BD SQLite sans même avoir à utiliser du code SQL ni même un outil de gestion de base de données.


Chaque table sera définie dans une classe Kotlin précédée de l'annotation @Entity. On dira de cette classe que c'est une entité de données ou encore un modèle de
données, parfois également appelée classe d'entité.


Toutes les entités de données seront placées dans un dossier nommé  data .


Ce dossier sera au même niveau que le fichier  MainActiviy.kt , par exemple  app/src/main/java/com/monnom/monprojet/data/Categorie.kt .


Pour créer ce dossier dans Android Studio : Clic droit sur son dossier parent /  New  /  Package .


Par défaut, la table portera le même nom que la classe et chaque colonne de la table portera le même nom que le champ de la classe.


Puisque la classe d'entité sert à définir des données, on lui ajoutera le mot-clé **data]**.


Les normes dictent que le nom de la classe doit être au singulier et utilise **la casse Pascal]**.


Mais attention : le nom de la table doit être au pluriel et entièrement en lettres minuscules.


```kotlin title="Fichier data/Categorie.kt"
@Entity(tableName = " categories ")
data class Categorie (
    @PrimaryKey(autoGenerate = true)
    val id : Int = 0,
    val titre: String = "",
    val description: String = "",
)
```


### Table avec clé étrangère


Pour une table qui comprend une clé étrangère :


```kotlin title="Fichier data/Item.kt"
@Entity(
    tableName = "items",
    foreignKeys = [ForeignKey(
        entity = Categorie ::class,
        parentColumns = arrayOf(" id "),
        childColumns = arrayOf(" categorie_id "),
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
    val categorie_id : Int,
)
```


#### Pour plus d'information


« Définir des données à l'aide d'entités Room ». Android Developers. https://developer.android.com/training/data-storage/room/defining-data?hl=fr


### « Entity ». Android Developers. https://developer.android.com/reference/androidx/room/Entity
53.3 Le DAO : couche intermédiaire entre l'application et la BD


Plusieurs cadres d'application offrent une couche d'abstraction entre l'application et la base de données, généralement sous forme de classes qui représentent les
tables de la BD. Cette couche d'abstraction est connue sous l'acronyme ORM (Object Relational Mapper).


Avec Jetpack Compose et Room, la couche d'abstraction utilise une interface DAO (Data Access Object ou objet d'accès aux données).


Grâce à la **classe d'entité]**, Room est capable de générer lui-même les requêtes
INSERT, UPDATE et DELETE pour gérer les données. Il suffit d'utiliser l'annotation appropriée (@Insert, @Update ou @Delete) et de passer une instance du modèle
en paramètre à la fonction.


Ces fonctions doivent être exécutées sur leur propre fil d'exécution (thread) pour ne pas bloquer l'application. C'est pourquoi les fonctions doivent utiliser le mot-clé
suspend.


Vous aurez besoin de requêtes SQL lorsque Room ne peut pas deviner vos besoins précis, par exemple pour les requêtes SELECT. À ce moment, la fonction utilisera
l'annotation @Query. La fonction retournera l'information sous le type Flow
., soit un flux de données asynchrone observable.


Le nom de l'interface du DAO – et du fichier – se terminera par Dao. Lorsque le DAO interagit avec une seule table, le nom sera sous la forme EntiteDao, par exemple
CategorieDao.


Tous les DAO seront placés dans un dossier nommé  data .


```kotlin title="Fichier data/CategorieDao.kt"
@Dao
interface CategorieDao {
    @Insert(onConflict = OnConflictStrategy.IGNORE)
    suspend fun insererCategorie(categorie: Categorie)
    @Update
    suspend fun mettreAJourCategorie(categorie: Categorie)
    @Delete
    suspend fun supprimerCategorie(categorie: Categorie)
    @Query("SELECT * FROM categories ...")
    fun listerCategories(): Flow<List<Categorie>>
    ...
}
```


### Ordre des enregistrements


Lorsqu'une requête peut retourner plus d'un enregistrement, il est important de spécifier dans quel ordre les enregistrements doivent être placés.


Rappel : il n'est pas acceptable de faire ORDER BY id puisque l'identifiant est une information interne que l'on ne devrait pas présenter à l'usager.


#### Pour plus d'information


« Accéder aux données à l'aide des DAO Room ». Android Developers. https://developer.android.com/training/data-storage/room/accessing-data?hl=fr


« Écrire des requêtes DAO asynchrones ». Android Developers. https://developer.android.com/training/data-storage/room/async-queries?hl=fr


« Créer le DAO ». Android Developers. https://developer.android.com/codelabs/basic-android-kotlin-compose-persisting-data-room?hl=fr#5


### 53.4 Le dépôt de données (repository)


Une autre étape est nécessaire pour gérer les données locales à partir de l'application Android : définir le dépôt de données (en anglais : repository).


Dans une application qui utilise un DAO, l'application passera toujours par le dépôt de données pour accéder aux données. Le dépôt de données est une sorte
d'isolant entre la source de données et le reste de l'application. Il est le seul à savoir d'où proviennent les données qu'il fournit à l'application, par exemple si elles
proviennent directement de la base de données ou de la mémoire cache.


C'est le dépôt qui fera appel aux méthodes définies dans l'interface **du DAO]**.


```kotlin title="Fichier data/CategorieRepository.kt"
class CategorieRepository(
    private val _categorieDao: CategorieDao
) {
    suspend fun insererCategorie(categorie: Categorie) = _categorieDao.insererCategorie(categorie)
    suspend fun mettreAJourCategorie(categorie: Categorie) = _categorieDao.mettreAJourCategorie(categorie)
    suspend fun supprimerCategorie(categorie: Categorie) = _categorieDao.supprimerCategorie(categorie)
    fun listerCategories(): Flow<List<Categorie>> = _categorieDao.listerCategories()
    ...
}
```


#### Pour plus d'information


### « Implémenter le dépôt ». Android Developers. https://developer.android.com/codelabs/basic-android-kotlin-compose-persisting-data-room?hl=fr#7
53.5 La classe qui hérite de RoomDatabase


Tous les  **DAO]** seront réunis dans une classe qui représente
la base de données en tant que telle.


Le code utilise le patron de conception du singleton, c'est-à-dire qu'il y aura un et un seul objet instancié.


La base de données peut porter n'importe quel nom. Une bonne pratique consiste à lui donner le même nom que l'application.


La classe qui définit la base de données de même que le fichier dans lequel elle est codée porteront un nom qui débute par le nom de la base de données et qui se
termine par Database. Ex : MonprojetDatabase.


Le fichier sera placé dans le dossier data .


```kotlin title="Fichier data/MonprojetDatabase.kt"
@Database(
    entities = [
        Categorie::class,
        Item::class,
    ],
    version = 1,
    exportSchema = false   // Room ne générera pas de fichier json de cette version de la BD
)
abstract class MonprojetDatabase : RoomDatabase() {
    abstract fun categorieDao(): CategorieDao
    abstract fun itemDao(): ItemDao
    companion object {
        @Volatile
        private var Instance: MonprojetDatabase? = null
        // obtient une instance de la BD ou la crée si elle n'existait pas
        fun getDatabase(context: Context): MonprojetDatabase {
            return Instance ?: synchronized(this) {
                Room.databaseBuilder(context, MonprojetDatabase::class.java, "monprojet_database")
                    .build()
                    .also { Instance = it }
            }
        }
    }
}
```


#### Pour plus d'information


« Créer une instance de base de données ». Android Developers. https://developer.android.com/codelabs/basic-android-kotlin-compose-persisting-data-room?
hl=fr#6


« Create ROOM Schema Export Directory ». Medium. https://medium.com/@vontonnie/create-room-schema-export-directory-7066d427eae8


### 53.6 Utiliser le dépôt de données via le ViewModel


C'est le ViewModel qui créera la base de données si elle n'existe pas puis qui interagira avec le dépôt de données.


Ce fichier sera placé dans le dossier ui .


Ici, le fait de déclarer le uiState avec MutableStateFlow assure que les informations seront automatiquement mises à jour lorsqu'il y a des changements dans les
données de la BD.


Remarquez que viewModelScope.launch(Dispatchers.IO) retournera une tâche (objet de type Job
), c'est-à-dire une référence (handle) vers une coroutine.


```kotlin title="Fichier ui/CategorieViewModel.kt"
class CategorieViewModel(application: Application) : AndroidViewModel(application) {
    private val _repository: CategorieRepository
    private val _uiState = MutableStateFlow(CategorieUiState())
    val uiState: StateFlow<CategorieUiState> = _uiState.asStateFlow()
   
    init {
        val context = application.applicationContext   // on utilise le contexte de l'application et non le contexte d'un
composable (LocalContext.current dans un Composable) sinon, le ViewModel serait recréé à chaque rotation du téléphone.
        // instancie la base de données et la crée physiquement au besoin
        val db = MonprojetDatabase.getDatabase(context)
        val dao = db.categorieDao()
        _repository = CategorieRepository(dao)
        
        observerCategories()
    }
    fun observerCategories() {
        viewModelScope.launch {
            // .collect permet de récupérer la valeur du flux observable
            _repository.listerCategories()
                .collect { categories ->
                    _uiState.update {
                        it.copy (
                            _listeCategories = categories
                        )
                    }
                }
        }
    }
    fun insererCategorie(categorie: Categorie) = viewModelScope.launch(Dispatchers.IO){
        _repository.insererCategorie(categorie)
    }
    fun mettreAJourCategorie(categorie: Categorie) = viewModelScope.launch(Dispatchers.IO){
        _repository.mettreAJourCategorie(categorie)
    }
    fun supprimerCategorie(categorie: Categorie) = viewModelScope.launch(Dispatchers.IO){
        _repository.supprimerCategorie(categorie)
    }
    ...
}
data class CategorieUiState(
    private var _listeCategories:List<Categorie> = emptyList(),   // Sera initialisé dans le init() du ViewModel puis ajusté
automatiquement si la BD change.
    ...
) {
    val listeCategories: List<Categorie>
        get() {
            return _listeCategories
        }
    ...
}
```


!!! warning "Note : il est généra"
    Note : il est généralement préférable de créer un ViewModel qui hérite de ViewModel plutôt que de AndroidViewModel. Ceci facilite notamment les tests unitaires.
Cependant, AndroidViewModel donne accès au contexte de l'application, ce qui permet d'accéder à la base de données sans devoir créer un **ViewModeFactory]**.


Comme toujours, chaque ViewModel ne doit exister qu'en un seul exemplaire.


Une variable viewModel sera instanciée dans le plus proche parent des composables qui en ont besoin et elle sera passée en paramètre à ses descendants.


Dans cet exemple, elle est instanciée directement dans l'écran principal.


!!! warning "Attention : il faut "
    Attention : il faut **ajouter une dépendance** pour que ce
code fonctionne puisque le ViewModel est instancié dans un composable.


```kotlin title="Fichier MainsActivity.kt"
@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun MainScreen() {
    val categorieViewModel: CategorieViewModel = viewModel()    // la fonction viewModel() se chargera d'injecter
l'application dans le constructeur de CategorieViewModel.
    Scaffold(
        topBar = {
            CenterAlignedTopAppBar(
                title = {
                    Text(text = "Test Room")
                },
            )
        }
    ) {
         MainContent(it, categorieViewModel)
    }
}
```


Notez qu'une approche différente pourra être utilisée **pour les applications avec plusieurs écrans]**.


Les composables qui ont accès au ViewModel peuvent désormais interagir avec la base de données.


```kotlin title="Fichier MainsActivity.kt"
@Composable
fun MainContent(paddingValues: PaddingValues, categorieViewModel: CategorieViewModel) {
    val categorieUiState by categorieViewModel.uiState.collectAsState()
    // ici, on a accès aux données en provenance de la base de données
    val nombreCategories = categorieUiState.listeCategories.size
    ...
}
```


#### Pour plus d'information


### « viewModelScope.launch(Dispatchers.IO) purpose ». Stack Overflow. https://stackoverflow.com/questions/55974539/viewmodelscope-launchdispatchers-io-
purpose
54. Le système de fichiers de l'émulateur



---
