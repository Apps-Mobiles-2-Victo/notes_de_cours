---
title: "Installation et outils de développement"
---

# Installation et outils de développement


### 2.1 Installation d'Android Studio


Android Studio est un environnement de développement intégré (IDE : Integrated Development Environment) spécialement conçu pour développer des applications
natives pour Android.


Il est basé sur IntelliJ, l'application de la suite JetBrains pour développer des applications notamment en Java et en Kotlin.


Android Studio est complètement gratuit.


Pour l'installer, rendez-vous sur https://developer.android.com/
 puis téléchargez la version qui correspond à votre système d'exploitation.


### 2.2 Installation de Kotlin


Si vous travaillez avec l'environnement de développement intrégré (IDE) IntelliJ ou Android Studio, vous
n'avez pas besoin d'installer Kotlin. Tout ce qui est nécessaire a été installé avec l'IDE.


<div style="text-align: center; margin: 1.5em 0;">
  <img src="../images/page_009_img_01_55x54.jpeg" alt="Illustration" style="max-width: 100%; border-radius: 8px; box-shadow: 0 4px 6px -1px rgba(0,0,0,0.1);" />
</div>


Dans le cas contaire, vous pouvez installer Kotlin en suivant les instructions ici :
https://kotlinlang.org/docs/command-line.html
.


## 3. Gradle



---


### 11.1 Vous avez dit Android?


Android est un système d'exploitation pour apparails mobiles basé sur Linux et développé par le Open Handset Alliance
.


La version la plus utilisée a été développée par Google. C'est une version propriétaire.


#### Pour plus d'information


« What is Android - The platform changing what mobile can do. ». Android Developers. https://www.android.com/what-is-android/


« Android (operating system) ». Wikipedia. https://en.wikipedia.org/wiki/Android_(operating_system)


### 11.2 Les versions d'Android


Les concepteurs d'Android ont pris l'habitude de nommer les différentes versions à l'aide d'un nom de dessert. Les noms suivent un ordre alphabétique à mesure
que les versions augmentent.


Voici une liste de ces versions tirée de Wikipédia
.
1


#### Numéro Nom
Niveau
API


#### Date de
sortie
Nouveautés


#### 1.0
---
1
septembre
2008


#### 1.1
Petit Four
2
février 2009


#### 1.5
Cupcake
3
avril 2009


#### 1.6
Donut
4
septembre
2009


#### 2.0
Eclair
5-6-7
octobre
2009


#### 2.2
Froyo
8
mai 2010


#### 2.3
Gingerbread
9-10
décembre
2010


#### 3.0
Honeycomb
11-12-
13
février 2011


#### 4.0
Ice Cream Sandwich
14-15
octobre 2011


#### 4.1
Jelly Bean
16-17-
18
juillet 2012


#### 4.4
KitKat
(à l'interne : Key Lime Pie)
19-20
octobre
2013


Lollipop


#### 21-22
novembre
2014


#### 5.0


(à l'interne : Lemon
Meringue Pie)


#### Marshmallow
(à l'interne :
Macadamia Nut
Cookie)


#### 23
septembre
2015


#### 6.0


#### 7.0
Nougat
(à l'interne : New York
Cheesecake)


#### 24-25
août 2016


#### 8.0
Oreo
(à l'interne : Oatmeal
Cookie)


#### 26-27
août 2017


#### 9.0
Pie
(à l'interne : Pistachio
Ice Cream)


#### 28
août 2018


#### 10
Quince Tart
(nom utilisé à
l'interne)


#### 29
septembre
2019
https://developer.android.com/about/versions/10/behavior-
changes-10?hl=fr


#### 11
Red Velvet Cake
(nom utilisé à
l'interne)


#### 30
septembre
2020
https://developer.android.com/about/versions/11/behavior-
changes-11?hl=fr


#### 12
Snow Cone
(nom utilisé à
l'interne)


#### 31-32
octobre
2021
https://developer.android.com/about/versions/12/behavior-
changes-12?hl=fr


#### 13
Tiramisu
(nom utilisé à
l'interne)


#### 33
août 2022
https://developer.android.com/about/versions/13/behavior-
changes-13?hl=fr


#### 14
Upside Down Cake
(nom utilisé à
l'interne)


#### 34
octobre
2023
https://developer.android.com/about/versions/14/behavior-
changes-14?hl=fr


#### 15
Vanilla Ice Cream
(nom utilisé à
l'interne)


#### 35
septembre
2024
https://developer.android.com/about/versions/15/behavior-
changes-15?hl=fr


#### 16
Baklava
(nom utilisé à
l'interne)


#### 36
juin 2025
https://developer.android.com/about/versions/16/behavior-
changes-16?hl=fr


<div style="text-align: center; margin: 1.5em 0;">
  <img src="../images/page_076_img_01_1000x460.png" alt="Illustration" style="max-width: 100%; border-radius: 8px; box-shadow: 0 4px 6px -1px rgba(0,0,0,0.1);" />
</div>


Sculptures à l'effigie des versions d'Android, Silicon Valley, 2015


> **Source** : 

## 1. « Android version history ». Wikipedia. https://en.wikipedia.org/wiki/Android_version_history


#### Pour plus d'information


« Historique des versions d'Android ». Wikipédia. https://fr.wikipedia.org/wiki/Historique_des_versions_d%27Android


### « Android API Levels ». API Levels. https://apilevels.com/
11.3 Quel langage pour programmer une application mobile Android?


Lorsqu'on débute le développement d'une application à l'aide d'une nouvelle technologie, il est important de se baser sur les meilleures pratiques et d'utiliser des
outils récents.


Avant d'écrire la première version de cette formation à l'automne 2023, j'ai pris le temps de bien explorer les technologies existantes afin de choisir celles qui me
semblaient le plus à jour pour développer une application mobile Android.


Côté programmation, j'ai exploré les langages disponibles pour les différents types d'applications Android :


#### Application native à l'aide d'un langage tel que Java ou Kotlin


#### Application hybride (basé sur HTML, CSS et JavaScript) à l'aide d'un cadre d'application (framework) tel que Cordova
ou PhoneGab


#### Application multiplateforme à l'aide d'un cadre d'application tel que Flutter ou React Native


Mon désir était de développer une application native et j'ai choisi **Kotlin]** puisqu'il s'agit du langage de programmation recommandé par Google pour la programmation Android.


Mais le choix du langage n'est pas tout. Il faut également regarder quelles technologies utiliser notamment pour générer l'interface graphique. J'ai arrêté mon choix
sur **Jetpack Compose]**.


#### Pour plus d'information


« What's the best tech stack for your mobile app in 2023? ». Imaginary cloud. https://www.imaginarycloud.com/blog/techstack-mobile-app/


### 11.4 Kotlin, le langage recommandé par Google pour Android


Kotlin est un langage de programmation orienté objet qui permet de développer différents types d'applications :


#### applications natives pour Android


#### applications multi-plateformes pour appareils mobiles


#### applications Web côté client (front-end)


#### applications Web côté serveur (back-end) et services Web


Il s'agit du langage de programmation recommandé par Google pour la programmation Android depuis 2019.


En effet, selon Wikipédia
:


### Kotlin est un langage de programmation orienté objet et fonctionnel, avec un typage dynamique qui


### permet de compiler pour la machine virtuelle Java, JavaScript, et vers plusieurs plateformes en
natif (grâce à LLVM). Son développement provient principalement d'une équipe de programmeurs
chez JetBrains basée à Saint-Pétersbourg en Russie (son nom vient de l'île de Kotline, près de St.
Pétersbourg).


### Google annonce pendant la conférence Google I/O 2017 que Kotlin devient le second langage de


### programmation officiellement pris en charge par Android après Java. Le 8 mai 2019, toujours lors
de la conférence Google I/O, Kotlin devient officiellement le langage de programmation voulu et
recommandé par le géant américain Google pour le développement des applications Android.


> **Source** : 

## 1. « Kotlin (langage) ». Wikipédia. https://fr.wikipedia.org/wiki/Kotlin_(langage)


#### Pour plus d'information


« Get started with Kotlin ». Kotlin. https://kotlinlang.org/docs/getting-started.html


« Formation Kotlin pour les programmeurs ». Android Developers. https://developer.android.com/courses/kotlin-bootcamp/overview?hl=fr


« Kotlin docs ». Kotlin. https://kotlinlang.org/docs/home.html


« Kotlin quick reference ». Alvin Alexander. https://kotlin-quick-reference.com/


« Keywords and operators ». Kotlin. https://kotlinlang.org/docs/keyword-reference.html


« Guide de l'architecture des applications ». Android Developers. https://developer.android.com/topic/architecture?hl=fr


### « Principes de base d'Android avec Compose ». Android Developers. https://developer.android.com/courses/android-basics-compose/course?hl=fr
11.5 Quel IDE pour programmer une application mobile Android?


Pour développer un projet Android, plusieurs environnements de développement intégrés (IDE) sont disponibles.


Les plus utilisés sont :


#### Android Studio


#### IntelliJ


IntelliJ est un IDE très populaire qui permet de développer des programmes en Java, en Kotlin et en bien d'autres langages.


Android Studio est également très populaire mais comme son nom l'indique, il ne permet que le développement d'applications Android.


Android Studio est basé sur IntelliJ.


Certains préféreront Android Studio pour les fonctionnalités spécialisées qu'il offre et surtout pour sa gratuité.


D'autres préféreront IntelliJ pour sa polyvalence.


Il est à noter que les développeurs d'Android donnent l'avantage à Android Studio pour certaines fonctionnalités récentes, qui ne seront disponibles dans IntelliJ que
plus tard.


## 12. Création d'un projet Android



---
