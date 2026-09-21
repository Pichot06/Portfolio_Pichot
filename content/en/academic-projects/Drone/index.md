---
title: "Drone trajectory planning"
translationKey: drone
date: 2025-02-24
url: "/en/projects/Drone/"
category: projects
layout: single
showtoc: true
tocopen: true
tags: ["Project", "GEII"]
---

# Introduction

For this project, we developed an algorithm allowing a drone to follow a predefined trajectory smoothly and accurately. The goal was to generate waypoints while respecting acceleration, maximum speed and motion-smoothing constraints.

## Trajectories

We studied fifth-degree polynomial trajectories and LSPB trajectories (Linear Segment with Parabolic Blends). LSPB trajectories combine linear segments with parabolic transitions. They are predictable, provide smooth speed changes and adapt well to robotics constraints.
