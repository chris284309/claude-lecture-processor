---
date: 2026-01-03
tags:
  - source/transcript
  - type/atomic
  - topic/machine-learning
  - domain/computer-science
status: active
source: "[[Transcript Lecture 01]]"
slides: [12, 13]
---

# Neural networks learn hierarchical feature representations

Neural networks organize information in layers, where each layer extracts increasingly abstract features from the input. Early layers detect simple patterns (edges, colors), while deeper layers combine these into complex concepts (faces, objects). This hierarchical organization emerges automatically through training, allowing the network to build sophisticated representations without manual feature engineering.

## Slides

![[Course/Lecture 01/Folien Bilder/Slide_12.png]]
![[Course/Lecture 01/Folien Bilder/Slide_13.png]]

## Context

*From Lecture 01 on Introduction to Deep Learning*

## Related

- [[Backpropagation enables efficient gradient computation through networks]]
- [[Loss functions measure prediction error to guide learning]]
- [[Activation functions introduce non-linearity enabling complex mappings]]

## Source

> "Each layer learns to detect patterns that are useful for the next layer. The early layers find edges and textures, the middle layers combine them into parts, and the final layers recognize whole objects." — Lecturer

From: [[Transcript Lecture 01]]
