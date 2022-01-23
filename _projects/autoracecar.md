---
layout: post
title: Autonomous Race Car
description: Système de navigation autonome pour voiture miniature
img: assets/img/rc-car.jpg
importance: 1
category: Robotique
---

***
Ce projet de recherche à été réalisé pour le cours **special project** lors de mon cursus à Georgia Tech, avec le laboratoire de robotique [The DREAM lab](https://dream.georgiatech-metz.fr) de Georgia Tech Lorraine. 
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

Pour pouvoir contrôlé la voiture, la première étape est une tâche de **computer vision**. La voiture, équipé d'une caméra, film en continu son envirronement. L'objectif est d'utiliser ces images afin d'extraire la route et utiliser ces données pour le contrôl de la voiture.

Afin d'accomplir cette tâche, nous avons testé de nombreux algorithmes pour voir ce qui fonctionnerait le mieux. Bien que toutes les méthodes ne se soient pas avérées viables, nous avons trouvé que les étapes suivantes étaient les plus efficaces pour la détection de la route : 
- Cartographie en perspective inverse (birds eye view) à partir des paramètres de la caméra.
- Segmentation des couleurs
- Détection des contours de Canny
- Transformée de Hough
- Filtre de Kalman

### Cartographie en perspective inverse


<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.html path="assets/img/rc/birds.png" title="birds" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        <p> <br> La méthode "Birds eye view" a pour but de projecter l'image de la caméra sur le plan du sol. De cette façon, les lignes de la route seront parallèle. Cette méthode a pour but de faciliter les traitements suivant.  </p>
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



### Segmentation des couleurs

Une fois la projection obtenue, nous voulons pouvoir detecter ce qui constitu la route, et ce qui n'est pas la route (par exemple, l'herbe sur le bord). La segmentation de couleur permet par clustering, de classer les pixels qui sont la route et ceux qui ne le sont pas, par continuité d'une zone en face de la voiture (le rectangle bleu de la vidéo).

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <video controls>
            <source src=”assets/img/rc/seg.mp4 type=video/mp4>
        </video>
    </div>
</div>
<div class="caption">
    La segmentation sur l'image de la route après projection.
</div>

### Détection des contours de Canny

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.html path="assets/img/rc/canny.png" title="canny" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        <p> <br> 
        La détection des contours de Canny est un algorithme de détection des contours en plusieurs étapes. En le réglant correctement, nous avons pu l'utiliser afin d'extraire les bords de la route.  </p>
    </div>
</div>




### Transformée de Hough

### Filtre de Kalman


<br/><br/>
<br/><br/>
<br/><br/>