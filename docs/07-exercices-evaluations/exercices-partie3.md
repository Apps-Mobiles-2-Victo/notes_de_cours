---
title: "Exercices pratiques - Partie 3 (9 à 14)"
---

# Exercices pratiques - Partie 3 (9 à 14)


### 56.1 Gestionnaire de signets - partie 1


Vous devez développer une application Android qui permet de gérer vos signets vers des pages Web (favoris, bookmarks).


Dans cette première version, n'utilisez pas de Scaffold. Nous allons voir plus tard comment bien structurer ce type d'application.


### 1. Créez le modèle de données qui permettra de stocker dans une base de données SQLite l'URL du signet ainsi qu'une
courte description.


## 2. Créez le DAO.


## 3. Créez le dépôt de données.


## 4. Créez la classe qui hérite de RoomDatabase.


## 5. Créez le ViewModel.


### 6. Dans la fonction MainScreen() (la vôtre pourrait porter un nom différent), faites le nécessaire pour faire afficher la liste
des signets. Dans un premier temps, un message approprié apparaîtra à l'écran pour indique que la BD est vide. Par contre, ceci fera en sorte que la base de données sera créée physiquement dans l'émulateur.


### 7. Pendant que l'application est en exécution dans l'émulateur, ouvrez la base de données de l'émulateur à l'aide du
Database Inspector puis exécutez des requêtes SQL pour ajouter ou modifier quelques enregistrements. Voyez les modifications qui se réflètent à l'écran sans nécessiter de rechargement.


## 57. Pour le prochain cours



---


### 57.1 Je me prépare pour l'exercice suivant (un cours)


Vous disposez d' un cours pour acquérir les connaissances théoriques et finaliser cet exercice.


Une fois cet exercice complété, vous devez effectuer vos lectures pour l'exercice suivant.


## 58. Room (suite)



---


### 61.1 Gestionnaire de signets - partie 2


Vous devez poursuivre le développement de votre application Android qui permet de gérer vos signets vers des pages Web.


### 1. Modifiez votre application pour que quelques signets soient automatiquement insérés dans la base de données lors
de sa création.


### 2. Lancez à nouveau votre application afin de voir les signets enregistrés. Si vous ne les voyez pas, c'est que vous avez
oublié de faire le nécessaire pour que la base de données soit recréée.


## 3. Prenez le temps de soigner la structure et l'apparence de votre application.


#### a. L'affichage d'un signet doit être réalisé dans sa propre fonction composable. La liste de signets fera appel à ce composable dans sa boucle.


#### b. Chaque signet doit être affiché dans un rectangle de l'apparence de votre choix. Suggestion : utilisez un


#### Card. Jouez avec les couleurs, bordures, ombrages, espacements, polices ou tout autre aspect de votre
choix afin d'obtenir une apparence professionnelle.


#### c. Comme toujours, il ne doit y avoir aucun texte collé sur le bord de l'écran ou collé sur une bordure.


#### d. Le lien hypertexte doit être cliquable et mener vers la page Web appropriée.


### 4. Effectuez l'internationalisation puis la localisation de votre application. Votre application doit pouvoir être affichée en
français et en anglais. Les données tirées de la base de données n'ont pas besoin d'être internationalisées.


## 62. Pour le prochain cours



---


### 62.1 Je me prépare pour l'exercice suivant (un cours)


Vous disposez d' un cours pour acquérir les connaissances théoriques et finaliser cet exercice.


Une fois cet exercice complété, vous devez effectuer vos lectures pour l'exercice suivant.


## 63. Application avec plusieurs écrans (navigation)



---


### 64.1 Gestionnaire de signets - partie 3


## 1. Votre application doit contenir les pages suivantes.


#### Une page d'accueil. Pour l'instant, elle ne fera qu'afficher un message ou une image de votre choix.


#### Une page qui liste vos signets.


#### Une page pour ajouter un signet. Pour l'instant, elle ne fera qu'afficher « À venir... ».


### 2. Dans la barre du bas, l'application doit présenter des icônes pour naviguer vers chacune des pages. La barre du bas
doit être définie dans un composable placé dans son propre fichier.


## 3. Les icônes de la barre du bas doivent être espacés pour remplir toute la largeur de l'écran.


### 4. Assurez-vous que chaque page ait, en plus du titre de l'application, un sous-titre qui définit ce qu'elle affiche. Vous
pouvez utiliser un simple Text() pour y parvenir.


### 5. Vous devez soigner l'apparence de votre application : couleurs agréables, espacements suffisants, icônes assez gros
ou suffisamment espacés pour les gros doigts, etc.


### 6. OPTIONNEL : vous constatez certainement que la création d'un nouveau projet exige la création de nombreux
fichiers, l'ajout de dépendances, etc.


Pour faciliter votre travail, il serait intéressant de pouvoir avoir un projet modèle qui servirait de base à vos prochains projets.


Je vous propose d'automatiser la tâche qui consiste à créer un nouveau projet à partir d'un projet de base.


Voici quelques pistes de solution :


#### a. Je n'ai pas trouvé de fonctionnalités intégrée dans Android Studio pour effectuer ce type de tâche.


#### Saurez-vous en trouver une?


#### b. J'ai vu un script bash qui semblait faire à peu près cette tâche mais je ne l'ai pas testé :


#### [https://github.com/erdo/commercial-template/blob/main/change_package.sh](https://github.com/erdo/commercial-template/blob/main/change_package.sh)
(discussion reddit qui m'a mené à ce script : [https://www.reddit.com/r/androiddev/comments/1c3txyz/is_there_a_good_way_to_start_a_project_using_a/?](https://www.reddit.com/r/androiddev/comments/1c3txyz/is_there_a_good_way_to_start_a_project_using_a/?) tl=fr )


#### c. La solution passe peut-être par la copie du dossier de base et le [apical_lien_interne]


#### [renommer_un_projet,renommage du projet][/apical_lien_interne]. Saurez-vous trouver la technique la
plus efficace pour y parvenir?


## 65. Pour le prochain cours



---


### 65.1 Je me prépare pour l'exercice suivant (un cours)


Vous disposez d' un cours pour acquérir les connaissances théoriques et finaliser cet exercice.


Une fois cet exercice complété, vous devez effectuer vos lectures pour l'exercice suivant.


## 66. Formulaire d'ajout de données



---


### 70.1 Gestionnaire de signets - partie 4 et questions théoriques


### 1. Afin de bien comprendre le code du ViewModelFactory, demandez à votre outil d'IA favori de vous expliquer ce code.
Posez-lui des questions jusqu'à ce que vous saisissiez bien le rôle de chaque ligne de code.


## 2. Demandez à votre prof de vous remettre une feuille avec les questions théoriques. Répondez-y en équipe de deux.


## 3. Vous avez désormais en main tous les morceaux pour finaliser le CRUD de vos signets.


#### a. Développez le formulaire d'ajout de données et faites le nécessaire pour que le contenu qui y est entré puisse être enregistré dans la base de données.


#### b. Effectuez la validation requise pour l'URL et la description. Assurez-vous qu'il ne soit pas possible d'esquiver les validations en cliquant directement sur le bouton d'enregistrement sans cliquer d'abord
dans les cases de saisie.


#### c. L'application doit retourner automatiquement à la liste des signets après l'ajout.


#### d. Dans la liste des signets, chaque entrée doit afficher une icône d'édition et une icône de suppression.


#### i. Un clic sur l'icône d'édition retrouvera les informations dans la base de données puis les affichera dans le formulaire d'édition. Le bouton d'enregistrement modifiera les données dans
la BD.


#### ii. Lorsque l'usager clique sur l'icône de suppression, une boîte de confirmation affiche le message « Désirez-vous vraiment supprimer le signet xxx ». Les xxx doivent être remplacés
par la description du signet sur lequel l'usager a cliqué. La suppression n'aura lieu que si l'usager clique sur Oui.


#### e. Asurez-vous que l'affichage soit correct dans le cas où un signet a un très long titre ou un très long URL.


#### f. Assurez-vous que toutes les chaînes soient correctement internationalisées et localisées.


#### g. OPTIONNEL : faites en sorte que le même formulaire soit utilisé lors de l'ajout et lors de la modification d'un signet.


## 71. Pour le prochain cours (deux cours)



---


### 71.1 Je me prépare pour l'exercice suivant (deux cours)


Vous disposez de deux cours pour acquérir les connaissances théoriques et finaliser cet exercice.


Une fois cet exercice complété, vous devez effectuer vos lectures pour l'exercice suivant.


## 72. Données distantes (API)



---


### 73.1 API, APK et questions théoriques


## 1. Demandez à votre prof de vous remettre une feuille avec les questions théoriques.  Répondez-y en équipe de deux.


### 2. Au début du second cours consacré à cet exercice, demandez à votre prof de vous remettre une nouvelle feuille avec
les questions théoriques. Répondez-y en équipe de deux.


### 3. Vous devez coder une application qui affiche des données sur les chats. Ces données sont obtenues à partir de l'API
Cat Facts : [https://catfact.ninja](https://catfact.ninja) .


#### a. Pour bien comprendre le type d'informations retournées par l'API, effectuez différents appels à l'aide d'un testeur de requêtes REST comme Postman, Bruno ou curl. Faites vos tests, par exemple, en utilisant les
points de terminaison fact, facts et breeds.


#### b. Testez un appel à l'API erroné afin de voir ce qui se passe lorsque le point de terminaison n'existe pas.


#### c. Écrivez une application Android avec Jetpack Compose qui affiche de façon aléatoire un fait sur les chats.


#### Un premier fait est affiché dès le démarrage de l'application. Un bouton permet d'afficher un autre fait de
façon aléatoire.


#### d. Assurez-vous qu'en cas de problème quel qu'il soit, l'usager soit bien informé de ce qui se passe. Par exemple, modifiez le point d'accès pour simuler ce qui se passerait si l'API changeait et que le point
d'accès n'était plus valide. L'application doit afficher un message explicite. Les messages du genre « Not Found » ne sont pas acceptables.


#### e. Modifiez l'icône d'application de votre application sur les chats.


#### f. Quand votre projet est terminé, générez un fichier APK de production et fournissez ce fichier à un de vos collègues pour qu'il l'installe sur son téléphone (publication en sideload).


#### g. OPTIONNEL : bonifiez votre application pour qu'elle utilise un API qui fournit des photos de chat. À chaque fois que les données sur les chats changent, on aura une nouvelle photo qui apparaîtra. À vous de trouver
un API qui fournit de telles photos.


## 74. Pour le prochain cours (deux cours)



---


### 74.1 Je me prépare pour l'exercice suivant (deux cours)


Vous disposez de deux cours pour acquérir les connaissances théoriques et finaliser cet exercice.


Une fois cet exercice complété, vous devez effectuer vos lectures pour l'exercice suivant.


## 75. Les capteurs



---


### 76.1 Travailler avec des capteurs


Écrivez une petite application Android avec Jetpack Compose qui détecte si un objet est près ou loin du téléphone. Elle utilisera pour cela le capteur de proximité .


### 1. Si le capteur détecte qu'un objet est près, l'écran sera entièrement vert et il sera écrit en gros « PRÈS ». Sinon,
l'écran sera bleu et il sera écrit « LOIN ». À vous de déterminer quelle distance est considérée près ou loin.


### 2. Prenez le temps de tester votre application sur un vrai téléphone car les valeurs retournées par le capteur peuvent
être différentes de celles rendues possibles dans l'émulateur.


## 77. Pour le prochain cours



---


### 77.1 Je me prépare pour l'exercice suivant (un cours)


Vous disposez d' un cours pour acquérir les connaissances théoriques et finaliser cet exercice.


Une fois cet exercice complété, vous devez effectuer vos lectures pour l'exercice suivant.


## 78. Examen 2



---
