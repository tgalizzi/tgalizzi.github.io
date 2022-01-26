---
layout: distill
title: Erato
description: Réseau de neuronnes pour la génération de poèmes 
img: assets/img/erato.jpg
importance: 1
category: Deep learning
categories: Deep-Learning NLP
date: 2021-09-28 21:01:00
toc:
  - name: Abstract
  - name: Le contexte du projet
  - name: Les données
  - name: Modèle Seq2Seq
  - name: Bert
---

***Note***:
Ce projet a été réalisé pour le cours [**CS 7650 - Natural Language Processing**](https://cocoxu.github.io/CS7650_fall2021/) lors de mon cursus à Georgia Tech. J'ai travaillé en colaboration avec **Victor Galizzi** et **Martin Puig**.

## Abstract

La génération de textes convaincants est un défi majeur du traitement automatique des langues. Certaines architectures précédentes, telles que Seq2Seq, ont fourni des résultats intéressants, mais toujours très éloignés du flux naturel de la parole humaine. Dans ce projet, nous discutons de différentes manières de créer des textes encore plus convaincants, dans un contexte qui s'oppose normalement à l'informatique : la poésie. 

## Le contexte du projet

La génération de texte à l'aide de modèles Seq2seq simples a tendance à générer des réponses sèches et banales telles que *"Je ne sais pas"* dans le cas des chatbots. Dans ce projet, nous explorons plusieurs techniques pour générer des textes plus spécifiques en utilisant un contexte, et nous travaillons sur la génération de poésie car la poésie est un exemple parfait de génération de texte qui nécessite verbosité et spécificité pour être crédible. Bien qu'il soit compliqué de trouver des métriques pertinentes pour ce type de tâche, nous nous sommes efforcés de comparer différentes architectures, en utilisant des métriques d'évaluation automatique, ainsi qu'en comparant la qualité de sorties sélectionnées. Nous avons exploré différentes techniques de génération basées sur un contexte d'entrée et une cible variés, en utilisant des exemples de vers, de mots-clés ou d'auteurs de poèmes.

## Les données

Notre base de données contient 573 poèmes de différents poètes et époques, avec 5 attributs différents : auteur, poème, titre, époque, type. Il y a deux époque différentes : **Renaissance**, **Moderne** et trois types différents : **Mythologie & Folklore**, **Nature**, **Amour**. Tout en gardant l'idée dans notre projet, nous n'avons pas utilisé l'attribut type, trop générique, ni l'époques, déjà codé avec l'auteur. 
Nous avons choisi cette base de données plutôt que des ensembles plus importants pour deux raisons principales :

- Il ne contient que des poèmes classiques, par rapport aux grandes bases de données remplies de poèmes contemporains. Les poèmes classiques sont plus structurés, ce qui les rend plus faciles à reconnaître, à évaluer et probablement à générer.
- Notre puissance de calcul était trop modeste pour fine-tune les gros modèles tels que BART et BERT sur de grandes bases de données.


## Modèle Seq2Seq

Ce modèle a été formé de deux manières différentes. Tout d'abord, pour les deux modèles, nous commençons nos entrées par l'auteur, pour essayer de capturer les différents styles entre les auteurs. Nous introduisons également un nouveau séparateur, $<sep>$.
Après le jeton séparateur, les modèles diffèrent : 
- Lines : nous divisons chaque poème en lignes, et essayons de prédire la ligne suivante du poème en utilisant la ligne actuelle.   
   **input** : AUTHOR $$<sep>$$ $$line_i$$, **target** : $$ligne_{i+1}$$.
- Mots clés : nous divisons chaque poème en lignes, et essayons de prédire la ligne du poème en utilisant les 3 mots les plus rares de la ligne. **input** : AUTEUR $$<sep>$$ $$w_1$$ $$w_2$$ $$w_3$$, **target** : $$line_{i}$$


Pour créer un poème, nous générons chaque ligne séquentiellement en utilisant la recherche par faisceau. Ensuite, les scores de chaque faisceau sont normalisés, et nous choisissons un faisceau aléatoire pour être notre prochaine ligne en utilisant les scores normalisés. Pendant la génération, nous avons un contexte représenté par des mots clés.
En plus des mots clés que nous donnons aux modèles, nous utilisons également des mots aléatoires de la ligne actuelle pour produire la suivante. Ceci est utile pour ajouter la généralisation et le caractère aléatoire pendant la génération.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/erato/seq1.png" title="example image" class="img-fluid rounded z-depth-1" zoomable="true"%}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/erato/seq2.png" title="example image" class="img-fluid rounded z-depth-1" zoomable="true"%}
    </div>
</div>
<div class="caption">
Comparaison d'auteur
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/erato/seq3.png" title="example image" class="img-fluid rounded z-depth-1" zoomable="true"%}
    </div>
</div>
<div class="caption">
William Shakespear
</div>


## Bart


Nous avons ensuite voulu comparer notre modèle simple avec un modèle NLP de pointe. Nous avons choisi BART \cite{BART}, qui est un encodeur-décodeur transformateur. L'encodeur est un encodeur bidirectionnel similaire à BERT, et le décodeur est un transformateur autorégressif, similaire à GPT. L'utilisation de ce modèle nous permet de tirer profit du pré-entraînement. En utilisant la bibliothèque Hugging face transformers, nous avons chargé un DistilBART pré-entraîné.

Nous affinons ensuite deux modèles de la même manière que celle décrite dans la section GRU, et obtenons un générateur de ligne BART (prenant un auteur et une ligne en entrée et générant une ligne) et un générateur de contexte BART (auteur + contexte pour générer une ligne). Les deux modèles ont été entraînés pendant 8 époques avec une taille de lot de 16.

Après quelques expériences, nous sommes arrivés à notre générateur de poèmes BART final : nous utilisons le générateur de contexte BART pour obtenir une ligne à partir d'un auteur et d'un contexte. Nous utilisons ensuite le générateur de lignes BART pour produire le reste du poème, en donnant séquentiellement au modèle l'auteur et la ligne précédente. Dans les deux cas, la génération est faite en utilisant l'échantillonnage top-k.


<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/erato/poem1.png" title="example image" class="img-fluid rounded z-depth-1" zoomable="true"%}
    </div>
</div>
<div class="caption">
William Shakespear
</div>


<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/erato/poem2.png" title="example image" class="img-fluid rounded z-depth-1" zoomable="true"%}
    </div>
</div>
<div class="caption">
William Shakespear
</div>


<br/><br/>