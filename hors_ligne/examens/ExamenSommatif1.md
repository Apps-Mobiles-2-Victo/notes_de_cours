# Applications Mobiles 2 - Examen Sommatif 1<br>Jetpack Compose : Pointage Minigolf

*	Cet examen pratique est noté sur 25 points et compte pour 25% de la note finale.
*	Vous disposez de deux périodes de 50 minutes pour effectuer le travail demandé.
*	Vous devez réaliser seul les étapes demandées.
*	Vous avez droit à toutes vos notes, exercices et à la consultation de documents sur Internet, incluant recherches via udm14.org
*	Vous n’avez pas droit aux outils de communication ni à l'IA.
*   La surveillance d'écran via Exam.net est obligatoire - le lien à suivre est fourni via Teams. Déconnectez-vous seulement après avoir remis votre travail sur Teams.
*   Ne fermez pas l'onglet du navigateur contenant la surveillance d'écran. Faire `Alt-Tab` pour passer aux autres applications sur votre portable.

## Remise du travail

* Remettre le travail demandé sur Teams dans un fichier .zip nommé *NomPrenom_Examen1.zip*.
* Le fichier doit contenir le projet Android Studio complet *sauf* le dossier 'app/build'.

# Énoncé du travail

Vous devez développer une application mobile Android avec Jetpack Compose. Cette application, dont le nom du dossier principal est au format `NomPrenom_Examen1`, permet à un joueur d’inscrire ses résultats pour une partie de minigolf comportant deux trous.

## Présentation de l’écran

L’écran de l’application doit respecter les consignes suivantes. L’impression d’écran fournie est présentée à titre indicatif.

- La barre de titre doit afficher **votre nom**.
- Sous la barre de titre, l’application doit afficher le texte **« Pointage Minigolf »**.
- Ce texte doit être centré.
- L’image fournie sur Teams représentant un parcours de minigolf doit être affichée sous le titre.
- L’image doit occuper toute la largeur de l’écran.

<p align="center"><img src="./examen1.png" alt="Image du parcours de minigolf" style="width: 60%; height: auto;"></p>

## Carte de pointage

Sous l’image, l’application doit présenter une carte de pointage contenant les informations suivantes :

- Trou no 1, normale 2
- Trou no 2, normale 3

La carte de pointage doit être organisée en trois colonnes :

- **Trou**
- **Normale**
- **Nombre de coups**

Chaque ligne doit contenir :

- le numéro du trou;
- la normale du trou;
- une case de saisie permettant d’inscrire le nombre de coups effectués.

Les cases de saisie doivent :

- être alignées l’une sous l’autre;
- accepter une valeur numérique;
- utiliser le clavier numérique;
- avoir une largeur raisonnable.

## Calcul du résultat

La normale totale du parcours est de **5 coups**.

Un bouton portant le texte **« Calculer le résultat »** doit être affiché sous la carte de pointage.

Lorsque l’utilisateur appuie sur ce bouton :

- le clavier doit disparaître;
- l’application doit calculer le nombre total de coups;
- l’application doit comparer le total obtenu à la normale totale;
- un message doit présenter clairement le résultat.

### Exemples de messages

> **Total : 4 coups.**  
> Vous avez joué 1 coup sous la normale.

> **Total : 5 coups.**  
> Vous avez joué la normale.

> **Total : 7 coups.**  
> Vous avez joué 2 coups au-dessus de la normale.

Le message doit être centré sous le bouton.

Aucune validation de saisie n’est requise. Vous pouvez considérer que l’utilisateur entre toujours un nombre valide dans chaque case.

## Apparence de l’application

Vous devez soigner l’apparence générale de l’application, notamment :

- les alignements
- les espacements
- les marges

## Gestion de l’état

Toutes les variables d’état doivent être gérées par un `ViewModel` associé à un `UiState`.

Le `UiState` doit contenir les propriétés suivantes :

- le nombre de coups du premier trou;
- le nombre de coups du deuxième trou;
- le message de résultat.

Le `ViewModel` doit permettre :

- de modifier le nombre de coups du premier trou;
- de modifier le nombre de coups du deuxième trou;
- de calculer le résultat et mettre à jour le message de résultat.

Les composables ne doivent pas calculer directement le total ni le résultat de la partie. Ces calculs doivent être effectués par le `ViewModel`.

## Organisation du code

- L’écran doit utiliser un `Scaffold`.
- Le contenu du `Scaffold` doit être réalisé dans sa propre fonction composable.
- Le calcul du résultat doit être effectué par le `ViewModel`.
- La fonction composable principale doit observer le `UiState` fourni par le `ViewModel`.