---
title: "SimIA"
date: 2025-03-24
url: "/fr/projects/SimIA/"
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



## Explication Du projet

Dans le cadre de ma troisième année à l’IUT de Nice en spécialité Électronique des Systèmes Embarqués (ESE), j’ai participé au projet SIMIA, visant à développer un système embarqué intelligent capable de traiter et d’interpréter des données d’un simulateur de conduite. L’objectif était de mettre en œuvre un modèle d’IA de reconnaissance de panneaux de signalisation à partir du jeu de données GTSRB, composé de plus de 50 000 images réparties en 43 classes. Une attention particulière a été portée au prétraitement des données (normalisation, encodage, gestion des valeurs manquantes) et à l’optimisation des modèles via des techniques de compression, comme la quantification post-entraînement et la quantization-aware training.

Le système cible tournait sur un microcontrôleur ESP32, qui jouait un rôle central en tant que serveur HTTP recevant des images et données du simulateur Unity. Trois modes de fonctionnement étaient proposés : le mode Label, qui prédisait uniquement la classe du panneau ; le mode Managed Speed, qui renvoyait une estimation de la vitesse ; et le mode Speed-Break, où l’ESP32 contrôlait directement l’accélération et le freinage virtuels. Chaque 100 ms, une image PNG RGB 32×32 était transmise à l’ESP32, accompagnée de données de télémétrie (vitesse, odomètre, radar, feu rouge). L’ESP32 devait alors renvoyer une réponse formatée, selon le mode actif.

Ce projet m’a permis d’approfondir mes compétences en IA embarquée, en communication HTTP, en traitement d’image à faible résolution et en adaptation de modèles à des contraintes matérielles strictes, tout en m’initiant à l’interfaçage temps réel avec un simulateur complexe.
