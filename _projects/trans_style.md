---
layout: page
title: Transfert de Style
description: Transfert de style pictural avec des CNNs
importance: 1
category: Deep learning
categories: DeepLearning ComputerVision CNN
img: assets/img/styletrans/starry_night.jpg
---



Ce projet était essentiellement l'implémentation du papier [**Image Style Transfer Using Convolutional Neural Networks** (Gatys et
al., CVPR 2015).](https://www.cv-foundation.org/openaccess/content_cvpr_2016/papers/Gatys_Image_Style_Transfer_CVPR_2016_paper.pdf  )

L'idée derrière ce papier est la génération d'image en utilisant le **contenu** de l'une et le **style** d'une seconde.

Plus précisément, deux fonctions de coût sont combinées pour quantifier le contenu et le style des images. A l'aide d'un CNN déjà entrainé à l'extraction de **features**, le contenu et le style des images sources sont extraits. La descente de gradient se fait alors sur les pixels de l'image générée et non sur les paramètres du CNN.

Voici un premier exemple, où le style de l'image est **Le Cri** de Edvard Munch et mon portrait qui figure sur ma page de présentation.

<div class="row justify-content-md-center">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/styletrans/the_scream.jpg" title="example image" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/prof_pic.jpg" title="example image" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
        <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/styletrans/scream_TO.png" title="example image" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    <b> Le Cri de Edvard Munch </b> avec un portrait.
</div>

***
<br/><br/>


<div class="row justify-content-md-center">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/styletrans/composition_vii.jpg" title="example image" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/styletrans/paris.jpg" title="example image" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
        <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/styletrans/composition_vii_paris.png" title="example image" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    <b> Composition VII de Vassily Kandinsky </b> avec une photo de Paris.
</div>


***
<br/><br/>



<div class="row justify-content-md-center">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/styletrans/the_scream.jpg" title="example image" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/styletrans/paris.jpg" title="example image" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
        <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/styletrans/scream_paris.png" title="example image" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    <b> Le Cri de Edvard Munch </b> avec une photo de Paris
</div>

***
<br/><br/>


<div class="row justify-content-md-center">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/styletrans/starry_night.jpg" title="example image" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/styletrans/paris.jpg" title="example image" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
        <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/styletrans/starry_paris.png" title="example image" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    <b>La Nuit étoilée de Vincent van Gogh </b> avec une photo de Paris
</div>