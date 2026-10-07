---
title: "Drone"
translationKey: drone
date: 2025-02-24
url: "/fr/projects/Drone/"
category: projects
summary:
description:
cover:
  image:
  alt:
  caption:
  relative: true
showtoc: true
draft: false
layout: single
tocopen: true
hidemeta: false
tags: ["projet", "GEII"]
keywords: ["projet", "GEII"]
---

# Introduction

Dans le cadre de ce projet, nous avons développé un algorithme permettant à un drone de suivre une trajectoire prédéfinie de manière fluide et précise. L’objectif principal était de générer une suite de points (waypoints) que le drone puisse suivre tout en respectant diverses contraintes physiques et environnementales, telles que l’accélération, la vitesse maximale ou encore le lissage du mouvement.

### Ce que j'ai réalisé

- **Modélisation dynamique :** Établissement des équations non linéaires du drone en appliquant les lois de la dynamique (Newton et Euler), puis linéarisation du système autour du point d'équilibre en stationnaire à l'aide de scripts Python et SymPy.
- **Mise en place de la simulation :** Utilisation de l'environnement PyBullet (`gym-pybullet-drones`) pour tester et valider le comportement du drone dans un espace.
- **Commande en cascade :** Synthèse et implémentation d'un régulateur PID en boucle fermée. La boucle externe gère la position tandis que la boucle interne stabilise l'attitude, permettant de convertir les consignes de déplacement en vitesses de rotation (RPM) pour chaque moteur.
- **Génération de trajectoires :** Développement d'algorithmes de planification de mouvement en Python, incluant des profils polynomiaux de degré 5 et des profils trapézoïdaux de type LSPB, garantissant des transitions fluides et sans à-coups.
- **Caractérisation expérimentale :** Analyse de la relation entre les signaux de commande PWM et la force de poussée réelle des hélices pour affiner l'inversion du modèle statique et dynamique.

### Technologies utilisées

- **Langage :** Python (NumPy, SciPy, SymPy, Matplotlib)
- **Simulation :** PyBullet
- **Outils :** Git, environnement Linux / VS Code
