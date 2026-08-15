---
title: "Optimisation des performances Compose"
---

# Optimisation des performances Compose


### 80.1 Quelques principes d'optimisation


...


#### Pour plus d'information


« Suivez les bonnes pratiques - Reporter les lectures le plus longtemps possible ». Android Developers.
https://developer.android.com/jetpack/compose/performance/bestpractices?hl=fr#defer-reads


### 80.2 Afficher le nombre de recompostion de chaque fonction modulable


Avec IntelliJ, il est possible d'afficher le nombre de fois qu'une fonction modulable est recomposée. Ceci est utile pour cibler les endroits où il y a perte de
performance.


Pour afficher le nombre de recompositions de chaque fonction modulable :


#### Ouvrez l'inspecteur de mise en page (Layout Inspector) : View / Tool Windows / Layout Inspector .


#### Dans la fenêtre Layout Inspector, cliquez sur l'icône View Options for Component Tree (en forme d'oeil) puis choisissez


#### Show Recomposition Counts .


<div style="text-align: center; margin: 1.5em 0;">
  <img src="../images/page_221_img_01_413x212.png" alt="Illustration" style="max-width: 100%; border-radius: 8px; box-shadow: 0 4px 6px -1px rgba(0,0,0,0.1);" />
</div>


#### Lancez votre application **dans un émulateur]**.


#### Naviguez dans votre application et voyez au fur et à mesure :


#### en rouge dans l'aperçu : les zones recomposées


#### à droite du nom de chaque module composable : le nombre de recompositon effectives


#### en gris : le nombre de recompositions ignorées


<div style="text-align: center; margin: 1.5em 0;">
  <img src="../images/page_222_img_01_670x960.png" alt="Illustration" style="max-width: 100%; border-radius: 8px; box-shadow: 0 4px 6px -1px rgba(0,0,0,0.1);" />
</div>


## 81. La programmation asynchrone



---
