---
title: "Exercices pratiques - Partie 1 (1 à 4)"
---

# Exercices pratiques - Partie 1 (1 à 4)


### 19.1 Exploration de Jetpack Compose


### 1. **Installez Android Studio]** sur votre poste de
travail.


### 2. Afin de vous familiariser un peu avec l'environnement, **créez l'application qui affiche Hello
Android à l'écran]** et faites-la tourner dans l'émulateur (pas seulement dans la prévisualisation).


## 3. Ouvrez le **fichier build.gradle.kts du dossier app]** et repérez les configurations minSdk et targetSdk.


#### a. Demandez à votre outil d'IA favori quel est le rôle de chacune?


#### b. Quand on crée un projet, est-ce que c'est le minSdk ou le targetSdk qui est sélectionné sur l'écran de


#### configuration?


#### c. Votre projet pourra-t-il être publié sur Google Play avec cette configuration?


### 4. Créez une nouvelle application nommée Tutoriel. Suivez le
tutoriel https://developer.android.com/jetpack/compose/tutorial?hl=fr


## 20. Pour le prochain cours



---


### 20.1 Je me prépare pour l'exercice suivant (un cours)


Vous disposez d' un cours pour acquérir les connaissances théoriques et finaliser cet exercice.


Une fois cet exercice complété, vous devez effectuer vos lectures pour l'exercice suivant.


## 21. L'état dans Jetpack Compose



---


### 23.1 Jeu de devinette - partie 1


Vous devez développer une application Android avec Kotlin et Jetpack Compose. Il s'agit d'un petit jeu de devinette simpliste.


Je vous suggère pour cet exercice de ne pas faire appel aux outils d'IA générative.


L'application contient un mot codé en dur. Elle affiche la première et la dernière lettre du mot et l'usager doit deviner ce qu'il y a entre les deux. S'il réussit, une
image apparaît pour le féliciter. Sinon, une image illustre qu'il n'a pas réussi.


### 1. Commencez par effectuer une lecture rapide des fiches des chapitres de référence. À ce stade, vous n'avez pas
besoin de tout comprendre. Il s'agit simplement de savoir où retrouver l'information en cas de besoin.


### 2. Créez un nouveau projet. Attention : votre application doit utiliser un nom de domaine en format inverse approprié
(par défaut, on voit com.example.monprojet mais l'utilisation de com.example n'est pas acceptable puisque votre
application pourrait ne pas être identifiée de façon unique dans Google Play).


### 3. Dessinez sur une feuille ce à quoi l'écran principal de votre application devrait ressembler. Vous êtes libres de vos
choix de disposition mais l'écran doit être facile à utiliser et agréable à regarder.


## 4. Importez les images pour illustrer la réussite et l'échec.


#### a. Ces images ne seront utilisées que lors du prochain exercice.


#### b. Les images choisies doivent être libres de droits (suggestions :


#### **Banques_d_images_professionnelles**).


#### c. Assurez-vous que chacune porte un nom en casse serpent.


#### d. Au besoin, conservez l'original de chaque image ainsi que les licences d'utilisation dans le dossier dev de


#### votre application, vous pourriez en avoir besoin au prochain exercice.


## 5. Affichez la première et la dernière lettre du mot (trouvées par programmation - à vous de trouver comment).


## 6. Déclarez une variable d'état qui contiendra le mot saisi par l'usager.


## 7. Faites en sorte que l'usager puisse saisir le mot à deviner. Le traitement sera réalisé dans le prochain exercice.


## 8. Assurez-vous que les textes et autres composables ne soient pas collés au bord de l'écran.


## 9. Testez l'apparence de votre application en mode clair puis en mode sombre.


## 24. Pour le prochain cours



---


### 24.1 Je me prépare pour l'exercice suivant (un cours)


Vous disposez d' un cours pour acquérir les connaissances théoriques et finaliser cet exercice.


Une fois cet exercice complété, vous devez effectuer vos lectures pour l'exercice suivant.


## 25. Déboguer une application Android



---


### 29.1 Jeu de devinette - partie 2


Poursuivez le développement de votre application. Le jeu devient réactif!


Si vous choisissez d'utiliser les outils d'IA générative pour vous aider à trouver les erreurs dans votre code, je vous suggère de toujours commencer par rechercher
la cause de l'erreur pendant au moins 5 minutes avant de faire appel à ces outils. Ceci vous permettra de mieux développer vos capacités de débogage et vous
aurez une meilleure compréhension de votre code.


Dans cet exercice et dans toutes vos applications à venir, le code complet de chaque gestionnaire d'événement, par exemple la réaction à un clic, doit
obligatoirement être placé dans une fonction.


#### Ceci permet d'assurer que vous pratiquez certaines notions enseignées, par exemple le hissage d'état.


#### Ceci met en lumière le fait que le code du gestionnaire d'événement n'est pas un composable. Il n'est donc pas
possible d'y définir d'autres  composables, par exemple un Text().


```kotlin title="Jetpack Compose (Kotlin)"
Button(
    onClick = {
        verifier(...)
    }
) {
    Text(text = "Vérifier")
}
fun verifier(...) {
    // le code complet sera placé ici
}
```


## 1. Ajoutez une barre de titre à l'application. Le titre doit être votre nom.


### 2. Ajoutez un bouton pour permettre de comparer le mot saisi avec le mot recherché. Ce bouton appellera une fonction
qui doit modifier la valeur d'une variable d'état pour indiquer si c'est une réussite ou un échec.


### 3. Mettez un point d'arrêt au début de votre fonction modulable principale et lancez l'application en mode débogage.
Inspectez les différentes variables et paramètres.


### 4. Afin de vous pratiquer à utiliser le Logcat, faites-y afficher la valeur saisie par l'usager. Prenez soin d'utiliser une
étiquette qui vous permettra de retrouver rapidement cette information.


## 5. Selon la valeur de la variable d'état, l'application doit afficher l'image de réussite ou l'image d'échec.


## 6. Si la licence d'utilisation de l'image l'exige, affichez la source de l'image en petit texte italique.


## 7. Dès que le bouton est cliqué, le clavier virtuel doit disparaître.


### 8. Apportez un grand soin à l'apparence de votre application : alignements espacements, couleurs, etc. Au besoin, faites
appel à votre prof qui vous indiquera les éléments visuels à améliorer.


### 9. OPTIONNEL : plutôt que d'utiliser une seule case de saisie pour enter le mot, utilisez une case de saisie par lettre à
deviner.


## 10. Testez votre application sur un vrai téléphone.


## 30. Pour le prochain cours



---


### 30.1 Je me prépare pour l'exercice suivant (un cours)


Vous disposez d' un cours pour acquérir les connaissances théoriques et finaliser cet exercice.


Une fois cet exercice complété, vous devez effectuer vos lectures pour l'exercice suivant.


## 31. Les icônes



---


### 34.1 Jeu de devinette - partie 3


### 1. Demandez à votre outil d'IA favori de vous expliquer les lignes de code données dans la théorie sur les préférences
utilisateur. Raffinez vos demandes jusqu'à ce que vous compreniez bien le code. S'il vous offre des commentaires
que vous jugez aidants, vous pouvez les copier dans votre code mais prenez soin de noter la source dans l'en-tête de
votre code (ex : La majorité des commentaires de ce code ont été générés par claude.ai le 2 septembre 2025).


### 2. À chaque fois que l'usager clique sur le bouton de vérification et que la vérification dit que ce n'est pas le bon mot,
l'application doit mettre à jour une préférence utilisateur qui retient le nombre d'essais réalisés. Ceci permettra de
refermer l'application et de poursuivre le jeu une autre fois.


### 3. La valeur de ce nombre doit être affichée à l'écran en tout temps et être accompagnée d'un libellé approprié puis
d'une icône de votre choix. L'icône apparaîtra en vert en cas de réussite et en une autre couleur de votre choix en cas
d'échec.


### 4. Quand le mot est trouvé, le nombre d'essais doit être affiché à l'écran puis être automatiquement remis à zéro pour la
prochaine partie. À vous de trouver la logique pour y parvenir!


### 5. Essayez de redémarrer l'application avant que le mot ne soit trouvé. Assurez-vous que le nombre d'essais continue
de s'incrémenter à partir d'où vous étiez rendus lors du lancement précédent de l'application.


## 6. Quand votre projet est terminé, générez un fichier APK de production.


### 7. OPTIONNEL : si vous avez en main un téléphone Android, entrez ce lien dans le navigateur de votre téléphone afin d'y
installer une petite application : android.apical.xyz/apptest/app-release.apk (démonstration d'une publication en
sideload).


### 8. OPTIONNEL : si vous avez en main un téléphone Android, scannez ce code QR avec votre téléphone afin d'installer
une autre petite application à partir de son fichier .apk


<div style="text-align: center; margin: 1.5em 0;">
  <img src="../images/page_132_img_01_300x300.png" alt="Illustration" style="max-width: 100%; border-radius: 8px; box-shadow: 0 4px 6px -1px rgba(0,0,0,0.1);" />
</div>


### 9. Fournissez le fichier APK de votre application à un de vos collègues pour qu'il l'installe sur son téléphone (publication
en sideload).


## 35. Pour le prochain cours



---


### 35.1 Je me prépare pour l'exercice suivant (un cours)


Vous disposez d' un cours pour acquérir les connaissances théoriques et finaliser cet exercice.


Une fois cet exercice complété, vous devez effectuer vos lectures pour l'exercice suivant.


## 36. La documentation Kotlin avec KDoc



---
