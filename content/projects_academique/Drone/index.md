---
title: "Drone"
date: 2025-02-24
url: "/projects/Drone/"
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


# Trajectoire 

Pour y parvenir, nous avons étudié deux types de trajectoires : les trajectoires polynomiales de 5ᵉ degré et les trajectoires LSPB (Linear Segment with Parabolic Blends).

🧭 Focus sur les trajectoires LSPB
Les trajectoires LSPB se composent de segments linéaires et de transitions paraboliques. Elles présentent plusieurs avantages majeurs :

Simplicité et prédictibilité : faciles à calculer et à implémenter, elles permettent une planification de mouvement très prévisible. Cela est particulièrement utile dans des contextes industriels, où la répétabilité et la précision sont essentielles.

Transitions fluides : grâce à la combinaison de phases linéaires et paraboliques, les LSPB assurent des changements de vitesse progressifs, limitant les chocs, les vibrations et donc l’usure mécanique.

Grande flexibilité : elles s’adaptent facilement à différentes contraintes de temps, de vitesse ou d’accélération, ce qui les rend adaptées à une large gamme d'applications robotiques.

