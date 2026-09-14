---
title: "SimAI 32"
date: 2025-03-24
category: projects
summary: "An embedded traffic-sign recognition system"
cover:
  image: cover.png
  relative: true
showtoc: true
layout: single
tocopen: true
tags: ["project", "embedded systems"]
keywords: ["AI", "ESP32", "computer vision"]
---

## Project overview

For this project, I developed an intelligent embedded system able to process and interpret data from a driving simulator. The goal was to deploy a traffic-sign recognition model trained on the GTSRB dataset, which contains more than 50,000 images across 43 classes.

The system runs on an ESP32 acting as an HTTP server. Every 100 ms, the simulator sends a 32x32 RGB PNG image together with telemetry data such as speed, odometer, radar and traffic-light information.

Three modes are available: Label predicts the sign class, Managed Speed estimates the speed, and Speed-Break controls virtual acceleration and braking.

## Neural network

I designed a compact convolutional neural network with a target size below 300 KB. It reached approximately 97% accuracy after training, while remaining small enough for an ESP32 with limited memory.

The project involved data preprocessing, model optimisation, post-training quantisation and quantisation-aware training. The final model was deployed as compressed C code for an embedded target.

![ESP32](1.jpg)

![Model](4.png)

![Training results](9.png)

![Confusion matrix](7.png)