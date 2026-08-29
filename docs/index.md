---
title: "Notes de cours - Applications mobiles Android avec Jetpack Compose"
---

# Applications mobiles pour Android avec Jetpack Compose

Bienvenue sur le site de référence des notes de cours pour le développement d'applications mobiles Android avec **Jetpack Compose** et le langage **Kotlin**.

Ces notes sont structurées de façon **thématique** afin de faciliter l'apprentissage progressif et la consultation rapide lors du développement de vos projets.

---

## 📚 Table des matières thématique

### 🛠️ 1. Environnement et Outils
* [**Introduction & Présentation du cours**](01-environnement/index.md) : Plan de cours, compétences ministérielles, politique départementale d'évaluation.
* [**Installation & Configuration**](01-environnement/installation.md) : Installation d'Android Studio, Kotlin et outils annexes.
* [**Création d'un premier projet**](01-environnement/premier-projet.md) : Structure d'un projet Android, imports et organisation du code.
* [**Android Studio & Ressources**](01-environnement/android-studio.md) : Guide de l'IDE, fenêtres d'outils, raccourcis et gestionnaire de ressources (`res/`).
* [**Prévisualisation vs Émulateur**](01-environnement/preview-vs-emulateur.md) : Tester avec `@Preview` interactive ou exécuter sur l'émulateur.
* [**Débogage & Dépannage**](01-environnement/debogage-et-depannage.md) : Logcat, points d'arrêt, inspecteur de disposition et résolution des problèmes fréquents (*Troubleshooting*).
* [**Publication d'application**](01-environnement/publication.md) : Génération de clés, signatures et déploiement (APK / Android App Bundle).

---

### 💻 2. Référence Kotlin et Outils de Build
* [**Gestion de projet avec Gradle**](02-kotlin-gradle/gradle.md) : Fichiers `build.gradle.kts`, catalogue de versions `libs.versions.toml`, gestion des dépendances et cache.
* [**Kotlin Fondamentaux**](02-kotlin-gradle/kotlin-bases.md) : Variables (`var`/`val`), types de base, string templates, structures de contrôle (`if`, `when`, `for`, `while`), optionnels et null-safety.
* [**Normes de code, KDoc et Licences**](02-kotlin-gradle/normes-et-kdoc.md) : Conventions de nommage, documentation avec KDoc et génération Dokka.

---

### 🎨 3. Interface utilisateur avec Jetpack Compose
* [**Bases de Jetpack Compose**](03-interface-compose/jetpack-compose-bases.md) : Paradigme déclaratif, fonctions `@Composable` et cycle de recomposition.
* [**Composants d'interface (UI)**](03-interface-compose/elements-ui.md) : `Text`, `Column`, `Row`, `Box`, `Image`, `AsyncImage`, `Icon`, `Surface`, `Card`, `Button`, `Switch`, `Checkbox`, etc.
* [**Modificateurs (Modifiers)**](03-interface-compose/modificateurs.md) : Espacement, dimensionnement, arrière-plan, bordures, clics, alignement.
* [**Mise en page avec Scaffold**](03-interface-compose/mise-en-page-scaffold.md) : Barre supérieure `TopAppBar`, barre inférieure `BottomAppBar`, `FloatingActionButton` et gestion des fenêtres de contenu.
* [**Saisie et Clavier virtuel**](03-interface-compose/saisie-et-clavier.md) : Champs de saisie (`TextField`, `OutlinedTextField`), focus et masquage du clavier.
* [**Couleurs, Thèmes et Hasard**](03-interface-compose/couleurs-et-ressources.md) : Palette Material 3, thèmes clair/sombre et génération aléatoire.
* [**Icônes et Images**](03-interface-compose/icones-et-images.md) : Bibliothèque Material Symbols et ressources graphiques vectorielles.
* [**Fenêtres Popup et Dialogues**](03-interface-compose/fenetres-popup.md) : Boîtes de dialogue `AlertDialog` et fenêtres contextuelles.
* [**Listes dynamiques**](03-interface-compose/listes-dynamiques.md) : Listes performantes avec `LazyColumn` et `LazyRow`.

---

### 🔄 4. Gestion de l'état et Architecture
* [**L'état dans Compose**](04-etat-architecture/etat-dans-compose.md) : Utilisation de `remember`, `mutableStateOf` et persistance au changement de configuration (`rememberSaveable`).
* [**État avancé & State Hoisting**](04-etat-architecture/etat-avance.md) : Élévation d'état, listes d'état mutables `mutableStateListOf` et flux unidirectionnel (UDF).
* [**Architecture MVVM avec ViewModel**](04-etat-architecture/viewmodel.md) : Séparation stricte de l'UI et de la logique métier, cycle de vie du `ViewModel`.
* [**Effets secondaires (Side Effects)**](04-etat-architecture/effets-secondaires.md) : `LaunchedEffect`, `rememberCoroutineScope` et exécution asynchrone sécurisée.
* [**Programmation asynchrone & Coroutines**](04-etat-architecture/programmation-asynchrone.md) : Tâches de fond et coroutines Kotlin dans l'écosystème Compose.

---

### 💾 5. Persistance et Données
* [**Préférences utilisateur**](05-donnees-persistance/preferences-utilisateur.md) : Stockage clé-valeur avec DataStore Preferences.
* [**Système de fichiers de l'émulateur**](05-donnees-persistance/systeme-fichiers.md) : Explorateur de fichiers, répertoires internes de l'application et bases de données SQLite.
* [**Base de données locale avec Room**](05-donnees-persistance/room-base-de-donnees.md) : Entités (`@Entity`), DAO (`@Dao`), base de données (`@Database`) et requêtes CRUD.
* [**Room avancé**](05-donnees-persistance/room-avance.md) : Observation réactive en continu avec `Flow`, tris et filtres dynamiques.
* [**Formulaires CRUD**](05-donnees-persistance/formulaires-crud.md) : Formulaires d'ajout, de validation, de modification et de suppression de données.
* [**Données distantes & API REST**](05-donnees-persistance/donnees-distantes-api.md) : Appels réseau HTTP, sérialisation JSON et gestion des autorisations Internet.

---

### 🚀 6. Fonctionnalités avancées et Matériel
* [**Lecture Audio & Sons MP3**](06-fonctionnalites-avancees/audio-mp3.md) : Intégration de MediaPlayer / ExoPlayer (Media3) pour effets sonores et musique.
* [**Navigation multi-écrans**](06-fonctionnalites-avancees/navigation.md) : Navigation Compose, `NavHost`, `NavController` et passage d'arguments entre destinations.
* [**Internationalisation (i18n)**](06-fonctionnalites-avancees/internationalisation.md) : Traduction des textes, fichiers `strings.xml` et localisation.
* [**Liens hypertextes**](06-fonctionnalites-avancees/liens-hypertexte.md) : Intégration de liens cliquables et ouverture du navigateur web.
* [**Capteurs de l'appareil (Sensors)**](06-fonctionnalites-avancees/capteurs.md) : Utilisation des capteurs physiques (accéléromètre, luminosité, etc.).
* [**Optimisation des performances**](06-fonctionnalites-avancees/optimisation.md) : Réduction des recompositions inutiles, annotations `@Stable` / `@Immutable`.
* [**Notifications**](06-fonctionnalites-avancees/notifications.md) : Canaux de notifications Android, création d'alertes et permissions.

---

### 📝 7. Exercices et Évaluations
* [**Exercices pratiques - Partie 1**](07-exercices-evaluations/exercices-partie1.md) : Exercices 1 à 4 (Bases Compose, Saisie, Clavier, Préférences).
* [**Exercices pratiques - Partie 2**](07-exercices-evaluations/exercices-partie2.md) : Exercices 5 à 8 (Popups, ViewModel, Audio MP3 et Examen formatif formel).
* [**Exercices pratiques - Partie 3**](07-exercices-evaluations/exercices-partie3.md) : Exercices 9 à 14 (Room, Navigation, Formulaires CRUD, API REST, Capteurs).
* [**Examens & Épreuve finale**](07-exercices-evaluations/examens.md) : Critères d'évaluation, contextes de réalisation et devis de l'épreuve finale.

