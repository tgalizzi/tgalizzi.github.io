---
layout: page
title: Erato
description: Réseau de neuronnes pour la génération de poèmes 
img: assets/img/erato.jpg
importance: 1
category: Deep learning
---

***

***Note***
Ce projet a été réalisé pour le cours [**CS 7650 - Natural Language Processing**](https://cocoxu.github.io/CS7650_fall2021/) lors de mon cursus à Georgia Tech. J'ai travaillé en colaboration avec **Victor Galizzi** et **Martin Puig**.

***

## Abstract

La génération de textes convaincants est un défi majeur du traitement automatique des langues. Certaines architectures précédentes, telles que Seq2Seq, ont fourni des résultats intéressants, mais toujours très éloignés du flux naturel de la parole humaine. Dans ce projet, nous discutons de différentes manières de créer des textes encore plus convaincants, dans un contexte qui s'oppose normalement à l'informatique : la poésie. 

## Le contexte du projet

La génération de texte à l'aide de modèles Seq2seq simples a tendance à générer des réponses sèches et banales telles que *"Je ne sais pas"* dans le cas des chatbots. Dans ce projet, nous explorons plusieurs techniques pour générer des textes plus spécifiques en utilisant un contexte, et nous travaillons sur la génération de poésie car la poésie est un exemple parfait de génération de texte qui nécessite verbosité et spécificité pour être crédible. Bien qu'il soit compliqué de trouver des métriques pertinentes pour ce type de tâche, nous nous sommes efforcés de comparer différentes architectures, en utilisant des métriques d'évaluation automatique, ainsi qu'en comparant la qualité de sorties sélectionnées. Nous avons exploré différentes techniques de génération basées sur un contexte d'entrée et une cible variés, en utilisant des exemples de vers, de mots-clés ou d'auteurs de poèmes.

## Les données

Notre base de données contient 573 poèmes de différents poètes et époques, avec 5 attributs différents : auteur, poème, titre, époque, type. Il y a deux époque différentes : **Renaissance**, **Moderne** et trois types différents : **Mythologie & Folklore**, **Nature**, **Amour**. Tout en gardant l'idée dans notre projet, nous n'avons pas utilisé l'attribut type, trop générique, ni l'époques, déjà codé avec l'auteur. 
Nous avons choisi cette base de données plutôt que des ensembles plus importants pour deux raisons principales :

- Il ne contient que des poèmes classiques, par rapport aux grandes bases de données remplies de poèmes contemporains. Les poèmes classiques sont plus structurés, ce qui les rend plus faciles à reconnaître, à évaluer et probablement à générer.
- Notre puissance de calcul était trop modeste pour fine-tune les gros modèles tels que BART et BERT sur de grandes bases de données.
