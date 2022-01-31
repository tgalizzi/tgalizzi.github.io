---
layout: post
title: Autonomous Race Car
description: Système de navigation autonome pour voiture miniature
img: assets/img/rc/rc-car.jpg
importance: 1
categories: Robotique ComputerVision
category: Robotique
---

***
Ce projet de recherche a été réalisé pour le cours **special project** lors de mon cursus à Georgia Tech, avec le laboratoire de robotique [The DREAM lab](https://dream.georgiatech-metz.fr) de Georgia Tech Lorraine. 
J'ai travaillé en colaboration avec **Victor Galizzi**, **Martin Puig** et **Jackson Crandell**.

***

## Notre objectif

Ce projet avait pour but de développer une voiture RC autonome sans utiliser l'apprentissage profond. Au lieu de cela, des moyens plus traditionnels ont été utilisés, tels que la vision par ordinateur et les algorithmes de contrôle. 
Notre objectif était de permettre à la voiture de réaliser des tours autour du lac faisant face à Georgia Tech Lorraine.


<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/rc/gtl.jpeg" title="gtl" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    Georgia Tech Lorraine
</div>


## La detection de la route 

Pour pouvoir contrôler la voiture, la première étape est une tâche de **computer vision**. La voiture, équipé d'une caméra, film en continu son envirronement. L'objectif est d'utiliser ces images afin d'extraire la route et utiliser ces données pour le contrôle de la voiture.

Afin d'accomplir cette tâche, nous avons testé de nombreux algorithmes pour voir ce qui fonctionnerait le mieux. Bien que toutes les méthodes ne se soient pas avérées viables, nous avons trouvé que les étapes suivantes étaient les plus efficaces pour la détection de la route : 
- Cartographie en perspective inverse (birds eye view) à partir des paramètres de la caméra.
- Segmentation des couleurs
- Détection des contours de Canny
- Transformée de Hough
- Filtre de Kalman

<br/><br/>

### Cartographie en perspective inverse


<div class="row justify-content-sm-center">
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.html path="assets/img/rc/birds.png" title="birds" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm-8 mt-3 mt-md-0">
        <p> La méthode "Birds eye view" a pour but de projecter l'image de la caméra sur le plan du sol. De cette façon, les lignes de la route seront parallèle. Cette méthode a pour but de faciliter les traitements suivant.  </p>
    </div>
</div>

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/rc/birds2.png" title="example image" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/rc/birds3.png" title="example image" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

<div class="caption">
    La méthode de projection sur un exemple réelle de la route.
</div>


<br/><br/>

### Segmentation des couleurs

Une fois la projection obtenue, nous voulons pouvoir detecter ce qui constitu la route, et ce qui n'est pas la route (par exemple, l'herbe sur le bord). La segmentation de couleur permet par clustering, de classer les pixels qui sont la route et ceux qui ne le sont pas, par continuité d'une zone en face de la voiture (le rectangle bleu de la vidéo).

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <video controls width="800">
            <source src="/assets/img/rc/seg.mp4" type="video/mp4">
        </video>
    </div>
</div>
<div class="caption">
    La segmentation sur l'image de la route après projection.
</div>

<br/><br/>

### Détection des contours de Canny

<div class="row justify-content-sm-center">
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.html path="assets/img/rc/canny.png" title="canny" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm-8 mt-3 mt-md-0">
        <p> La détection des contours de Canny est un algorithme de détection des contours en plusieurs étapes. En le réglant correctement, nous avons pu l'utiliser afin d'extraire les bords de la route.  </p>
    </div>
</div>

<br/><br/>


### Transformée de Hough



<div class="row justify-content-sm-center">
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.html path="assets/img/rc/hough.png" title="hough" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm-8 mt-3 mt-md-0">
        <p> La transformée de Hough peut être utilisée pour détecter n'importe quelle forme. Dans notre situation, nous l'avons utilisée pour détecter les bords de la route. A chaque image de la vidéo filmée par la camera, cet algorithme nous renvoie un nombre important de "lignes" candidates comme bords de route. Les lignes blanches sont la sortie de l'algorithme de Hough. Seules les lignes avec un petit angle ont été sélectionnées comme une observation correcte.</p>
    </div>
</div>

<br/><br/>


### Filtre de Kalman


<div class="row justify-content-sm-center">
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.html path="assets/img/rc/kalman.png" title="hough" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm-8 mt-3 mt-md-0">
        <p> Le filtre de Kalman est une méthode de filtrage qui permet, à partir d'observations bruitées (ici, la sortie de l'algorithme de Hough), d'estimer l'état d'un système dynamique. Ici, nos mesures nous permettent grâce au filtre de Kalman d'estimer les véritables lignes de la route de manière séquentielle, a chaque image de la caméra.</p>
    </div>
</div>

<br/><br/>

### Le résultat

Après toutes ces étapes, nous avons une mesure des bords de la route. En reprojetant nos bords de route dans le plan de la caméra, nous avons une segmentation complète. Voici un exemple de ce que donne notre méthode en situation réelle.


<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <video controls width="800">
            <source src="/assets/img/rc/kalman.mp4" type="video/mp4">
        </video>
    </div>
</div>
<div class="caption">
    Extraction de la route.
</div>


## Le contrôle

Nous avons commencer par un contrôle très simple, de suivi de ligne.
Ainsi, la voiture se rapproche du centre de la route tout en suivant son axe.

Malheuresement, ce projet a été stoppé net par la crise sanitaire. Le premier confinenement nous a empêché de mener ce projet jusqu'au bout. Notre méthode, testée sur le simulateur airsim, semblait très prometteuse.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <video controls width="800">
            <source src="/assets/img/rc/simu.mp4" type="video/mp4">
        </video>
    </div>
</div>
<div class="caption">
    Aperçu de notre contrôle autonome sur le simulateur airsim.
</div>

<br/><br/>
