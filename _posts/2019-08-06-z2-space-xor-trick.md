---
layout: post
title: A Beautiful Technique for Some XOR Related Problems
subtitle: >-
  A nice idea to view xor in terms of addition in $\mathbb{Z}_2$ and use linear
  algebra that cracks quite a few problems
tags:
  - Competitive Programming
  - Linear Algebra
comments: true
published: true
---
Working on it!
|  Bit no.  | First number | Second number | $\oplus$ | Sum | Sum taken $\pmod 2$ |
|:---------:|:------------:|:-------------:|:--------:|:---:|:-------------------:|
| $1$st bit |      $0$     |      $0$      |    $0$   | $0$ |         $0$         |
| $2$nd bit |      $1$     |      $0$      |    $1$   | $1$ |         $1$         |
| $3$rd bit |      $0$     |      $1$      |    $1$   | $1$ |         $1$         |
| $4$th bit |      $1$     |      $1$      |    $0$   | $2$ |         $0$         |