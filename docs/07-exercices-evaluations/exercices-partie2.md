---
title: "Exercices pratiques - Partie 2 (5 à 8 & formatif)"
---

# Exercices pratiques - Partie 2 (5 à 8 & formatif)


### 40.1 Couleur de fond


Comme dans tous vous exercices formatifs ou sommatifs, assurez-vous de bien citer vos sources lorsque vous vous inspirez de code trouvé sur Internet ou généré par une IA ou lorsque vous utilisez une technique différente de celle enseignée.


Ceci assurera que lors de travaux sommatifs, vous puissiez encore citer ces sources afin de limiter les risques d'être accusés de plagiat.


### 1. Demandez à votre prof de vous remettre la feuille qui contient le code à corriger. Notez-y les corrections à apporter
puis remettez la feuille à votre prof .


## 2. Créez une nouvelle application nommée Couleur de fond.


#### a. Assurez-vous que le nom de domaine en format inverse ne débute pas par com.example.


#### b. L'application doit comporter une barre de titre avec le titre de votre choix.


#### c. La couleur de fond de l'application doit pouvoir être modifiée dynamiquement en cliquant sur un bouton.


#### Elle pourra prendre l'une ou l'autre parmi trois couleurs de votre choix. Afin de simplifier l'exercice, ne vous
souciez pas de la couleur de fond de la barre de titre pour l'instant.


#### d. À propos du bouton qui permet de modifier la couleur de fond :


#### i. Il doit être centré en hauteur et en largeur.


#### ii. Le texte du bouton doit être en gros caractères.


#### iii. Assurez-vous que l'espacement entre le texte et les bords du bouton soit intéressant.


#### L'espacement par défaut n'est pas acceptable ici.


#### iv. Un clic modifiera immédiatement la couleur. À vous de déterminer comment la couleur sera choisie (au hasard, prochaine couleur disponible, etc.).


#### e. Vous devez faire un minimum de découpage dans votre code.


#### i. Assurez-vous que le contenu de l'application (paramètre content du scaffold ou partie entre accolades) soit dans sa propre fonction.


#### ii. De même, le code de chaque gestionnaire d'événement, par exemple la réaction à un clic, sera dans une fonction.


#### iii. Notez qu'à moins d'avis contraire, ces deux exigences valent pour toutes vos applications à venir et ce, même lorsque ce n'est pas mentionné.


#### f. Lorsque la nouvelle couleur est connue, une fenêtre popup (Popup() ou Snackbar()) affiche de l'information sur cette couleur.


#### g. Retravaillez maintenant votre application pour que la couleur de fond apparaisse également sous la barre de titre.


#### h. Assurez-vous de documenter correctement vos fonctions (modulables ou non) avec KDoc.


#### i. Générez la documentation de votre code à l'aide de Dokka.


### j. Assurez-vous qu'une fois l'application terminée, il ne reste aucune directive import inutilisée.
41. Pour le prochain cours



---


### 41.1 Je me prépare pour l'exercice suivant (un cours)


Vous disposez d' un cours pour acquérir les connaissances théoriques et finaliser cet exercice.


Une fois cet exercice complété, vous devez effectuer vos lectures pour l'exercice suivant.


## 42. Examen formatif formel



---


### 42.1 Grille de correction


Voici la grille de correction qui sera utilisée à l'examen formatif formel en Applications mobiles 2.


Dans tout le code que vous écrirez, vous devez respecter les techniques, les pratiques et les normes de nomenclature enseignées en classe. Plusieurs sont implicites, par exemple bien découper le code, bien nommer les ressources, mention claire de code emprunté pour tout code inspiré de contenu trouvé en dehors des notes de cours.


25 points Excellent Satisfaisant Minimal Faible Insuffisant


Il y a au moins un aspect non conforme.


Il y a au moins deux aspects non conformes.


Il y a au moins trois aspects non conformes.


Il y a au moins quatre aspects non conformes.


Les éléments graphiques répondent à ce qui a été demandé et sont correctement configurés.


Éléments graphiques


8 points


6 points


5 points


3 points


La programmation répond à ce qui a été demandé : choix des types de données, initialisations, paramètres, conversions de type, conditions, boucles, traitements, contenu du Scaffold et des gestionnaires d'événements dans leurs propres fonctions, etc.


Il y a au moins une erreur.


Il y a au moins deux erreurs.


Il y a au moins trois erreurs.


Il y a au moins quatre erreurs.


#### Structure du code
et logique


12 points


9 points


5 point


15 points


L'apparence générale (espacements, tailles, couleurs et autres mises en forme, utilisation du innerPadding du Scaffold) répond à tout ce qui a été demandé. L'ensemble donne une apparence esthétique.


#### Apparence
générale et documentation Dokka


Il y a au moins un aspect non conforme ou non esthétique.


Il y a au moins deux aspects non conformes ou non esthétiques.


Il y a au moins trois aspects non conformes ou non esthétiques.


Les commentaires KDoc sont correctement formulés et la documentation Dokka est bien générée.


2 points


1 point


3 points


Les standards et les bonnes pratiques ont été respectés. Par exemple : respecter la nomenclature, éviter la codification en dur des couleurs lorsque le contexte s'y prête, nom de domaine en format inverse approprié, aucune directive import inutilisée, etc.


Il y a au moins un standard ou une bonne pratique non respectée.


#### Respect des
standards et des bonnes pratiques


1 point


Plus d'une réponse est presque complète ou presque exacte.


Au moins une réponse est presque complète ou presque exacte.


Les réponses sont insatisfaisantes.


#### Questions de
compréhension


Chaque réponse est complète et exacte.


ou


3 points


Au moins une réponse est insatisfaisante.


2 points


1 point


### 42.2 Examen formatif formel


Cet examen sera auto-corrigé en classe au prochain cours.


Vous devez tout de même remettre votre travail sur la plateforme électronique du cours.


Puisqu'il s'agit d'un examen formatif, il n'y a aucun point attribué mais une grille de correction vous est fournie à titre indicatif.


## 1. Petite application


#### a. Vous devez créer une application Android avec Jetpack Compose qui permet d'afficher des bananes à l'écran. Nommez votre projet au format NomPrenom-FormatifFormel.


#### b. L'image à afficher est disponible ici : Android-BananePourFormatifFormel-fjdksf54f.svg.


#### c. L'application doit avoir votre nom dans sa barre de titre.


#### d. Au départ, il n'y aura rien à l'écran sauf la barre de titre.


#### e. Il y aura également dans la barre de titre une icône en forme de + qui permet d'augmenter le décompte des bananes à afficher. L'application affichera les bananes l'une sous l'autre, centrées en largeur.


#### f. N'oubliez pas qu'afin de faciliter la lecture du code, le contenu du Scaffold devra être placé dans sa propre fonction composable. De même, les gestionnaires d'événement seront eux aussi dans leur propre
fonction.


#### g. À chaque fois qu'une banane est ajoutée, elle apparaît à l'écran et un message de confirmation apparaît dans un Snackbar avec le texte « La banane no X a été ajoutée », où X représente le nombre de bananes
affichées. Il doit être possible de faire disparaître ce message à l'aide d'un clic.


#### h. L'écran doit pouvoir défiler verticalement mais la barre de titre, elle, ne défilera pas.


#### i. Assurez-vous que toutes vos fonctions soient bien documentées puis générez la documentation à l'aide de Dokka.


## 2. Questions de compréhension. Inscrivez les réponses dans un fichier texte nommé au format NomPrenom-


#### comprehension.txt .


#### a. Quand on désire travailler avec le Preferences DataStore dans une fonction non composable, expliquez dans vos propres mots pourquoi nous obligés de passer les objets preferencesUtilisateur et scope en
paramètre à la fonction non composable? Soyez précis dans votre réponse.


#### b. Lorsqu'on fait du hissage d'état, le paramètre qui permet de modifier la valeur d'une variable d'état est


### sous la forme onMaVariableDEtatChange = { maVariableDEtat = it }. Expliquez clairement, dans vos
propres mots, d'où le mot it prend sa valeur et pourquoi il porte ce nom. ────────── Semaine 4 ──────────


## 43. État persistant



---


### 45.1 Le pouce rapide


### 1. Vous désirez développer une petite application inutile, juste pour passer le temps. Elle permet de vérifier à quel point
votre pouce est rapide pour cliquer sur un bouton.


#### a. Vous devez afficher le titre de l'application dans la barre de titre.


#### b. L'application présente un bouton sur lequel il faut cliquer rapidement.


#### c. Elle doit utiliser un ViewModel comme conteneur d'état. Son UiState devra retenir notamment une


#### **liste** d'heures, vide au départ (je vous conseille
d'utiliser listOf()).


#### d. À chaque clic, l'application retient l'heure à laquelle le clic a eu lieu. La date et l'heure courantes peuvent être obtenues facilement à l'aide de LocalDateTime.now().


Pour bien mettre à jour le ViewModel, vous devez faire l'ajout dans _uiState.update et mettre le code dans it.copy. Prenez soin d'utiliser la bonne syntaxe pour mettre à jour un tableau.


#### e. La liste des heures apparaît sous le bouton. Si la liste est trop longue, elle peut défiler mais le bouton ne doit pas bouger. Vous devez soigner le format d'affichage des heures.


#### f. Le but est de faire deux clics ultra-rapprochés le plus rapidement possible. Je vous laisse le soin d'établir le délai à atteindre pour dire que les deux clics ont été assez rapprochés. Je vous laisse le soin de trouver
la technique pour soustraire deux dates.


#### g. Une fois ce délai atteint, il n'est plus possible de cliquer pour poursuivre. Le jeu se termine et l'application affiche un message à cet effet.


#### h. OPTIONNEL : Modifiez votre application pour que le délai à atteindre soit saisi à l'écran. Rappel : toutes les variables d’état doivent être gérées par un ViewModel associé à un UiState.


#### i. DÉFI SUPPLÉMENTAIRE : organisez votre interface pour que lorsque le nombre de dates est plus long que l'espace disponible à l'écran, le défilement soit fait automatiquement de façon à voir la dernière date
entrée.


### 2. OPTIONNEL : Prenez le code présenté dans la fiche
« **survivre_a_la_recreation_de_l_activite** » et convertissez-le pour qu'il utilise un ViewModel. Lancez l'application dans l'émulateur et modifiez l'orientation du téléphone afin de vérifier si l'état est conservé quand l'activité est recréée.


## 46. Pour le prochain cours



---


### 46.1 Je me prépare pour l'exercice suivant (un cours)


Vous disposez d' un cours pour acquérir les connaissances théoriques et finaliser cet exercice.


### Une fois cet exercice complété, vous devez effectuer vos lectures pour l'exercice suivant.
47. Faire jouer des sons MP3



---


### 48.1 Jeu Simon


Vous devez coder un jeu Simon pour Android avec Jetpack Compose (voir un exemple ici : [https://www.memozor.com/fr/jeux-du-simon/jeu-du-simon](https://www.memozor.com/fr/jeux-du-simon/jeu-du-simon) ).


Voici le fonctionnement normal du jeu :


#### Le jeu présente quatre boutons de couleurs. Chaque bouton est associé à un son.


#### Le système « illumine » une séquence de boutons tout en faisant jouer les sons correspondants.


#### L'usager doit cliquer sur les boutons afin de reproduire la séquence.


#### S'il réussit, un bouton supplémentaire est ajouté à la séquence à reproduire.


#### Le jeu se poursuit tant que l'usager ne fait pas d'erreur dans la séquence.


Le jeu sera simplifié afin de ne pas être trop long à coder.


Voici les consignes à respecter :


#### Toutes les variables d'état doivent être gérées à l'aide d'un ViewModel et d'un UiState.


#### Les boutons sont de simples rectangles placés en damier (deux par ligne). Suggestion : utiliser des Box().


#### Dans un premier temps, la séquence à reproduire, stockée dans le ViewModel, est codée en dur (ex : bouton 1,
bouton 3, bouton 1, bouton 2).


#### Le jeu ne fera pas « illuminer » les boutons selon la séquence. L'usager devra donc deviner la séquence à reproduire
et non reproduire ce qu'il aura vu.


#### Le jeu se termine dès que l'usager fait une erreur. S'il réussit la séquence, le jeu se termine après le dernier clic. C'est
le ViewModel (ou son uiState) qui est responsable de dire si le jeu est terminé ou non.


#### Soignez l'apparence des boutons afin que le jeu ait une apparence professionnelle. Par exemple, ajoutez un ombrage,
une bordure, un point lumineux pour donner un effet 3D. Le code pour afficher un bouton sera placé dans sa propre fonction composable.


#### Une fois le jeu fonctionnel, modifiez-le pour que la séquence à reproduire soit générée au hasard. Ceci sera codé
dans une méthode du ViewModel.


#### OPTIONNEL : ajustez le jeu pour qu'il génère d'abord une série d'un seul bouton à cliquer. Une fois la série
correctement reproduite par l'usager, il en ajoutera un 2e puis un 3e, etc., comme dans le vrai jeu. Ici encore, le jeu se termine quand l'usager fait une erreur. S'il est très bon, la séquence peut devenir très longue.


#### OPTIONNEL : quand l'usager clique sur le bouton « Démarrer », les boutons s'illuminent selon la séquence.
L'illumination peut être une modification dans l'opacité de la couleur, l'ajout d'une bordure ou tout autre effet visuel de votre choix. Une fois la séquence jouée, un texte invite l'usager à cliquer sur les boutons pour reproduire la séquence.


#### OPTIONNEL : associez un son à chacun des boutons, comme dans le vrai jeu (vous pouvez utiliser ces sons :
[https://sounddino.com/en/effects/notes/](https://sounddino.com/en/effects/notes/) ). Le son sera entendu quand l'application montre la séquence à reproduire de même que quand l'usager cliquera sur les boutons.


#### OPTIONNEL : assurez-vous que les boutons ne soient cliquable que lorsque c'est opportun dans le jeu. Par exemple,
il ne doit pas être possible de cliquer sur les boutons de couleur pendant que le jeu illumine les boutons pour montrer la séquence.


### Quand votre projet est terminé, générez un fichier APK de production et fournissez ce fichier à un de vos collègues pour qu'il l'installe sur son téléphone (publication
en sideload).
49. Pour le prochain cours



---


### 49.1 Je me prépare pour l'exercice suivant (un cours)


Vous disposez d' un cours pour acquérir les connaissances théoriques et finaliser cet exercice.


Une fois cet exercice complété, vous devez effectuer vos lectures pour l'exercice suivant.


## 50. Exercice 8



---


### 50.1 Devinette avec ViewModel


Reprenez votre jeu de devinette. Cette fois, toutes les variables d’état doivent être gérées par un ViewModel associé à un UiState.


Suggestion : commencez par reproduire l'application avec un ViewModel mais sans préférences utilisateur.


Une fois que cela fonctionne, ajoutez les préférences utilisateur. À vous de faire les recherches pour trouver la bonne façon de procéder.


## 51. Pour le prochain cours (deux cours)



---


### 51.1 Je me prépare pour l'exercice suivant (deux cours)


Vous disposez de deux cours pour acquérir les connaissances théoriques et finaliser cet exercice.


Une fois cet exercice complété, vous devez effectuer vos lectures pour l'exercice suivant.


## 52. Examen 1



---
