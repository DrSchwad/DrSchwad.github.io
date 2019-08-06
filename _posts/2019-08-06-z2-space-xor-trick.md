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
## Inspiration

I'm very excited about this blog, as it took me quite a lot of effort and scavenging through the internet to completely grasp the concept of this technique(That's probably because I have almost zero knowledge in Linear Algebra or I'm just plain dumb). So I feel like I genuinely conquered a challenge, and I really want to share it with someone. But I'm afraid my CP friends circle might think I'm just trying to show off :P

So here I am, sharing it on CF. I also created a [personal blog](https://drschwad.github.io/), so that if I ever feel like sharing something again(not only about CP), I can write a blog there. I also added this same post [there](https://drschwad.github.io/2019-08-06-z2-space-xor-trick/), you can read it there if you prefer dark theme. I'll be pleased to hear any thoughts on the blog or if I can improve it in some way ^\_^

## Introduction

Since it concerns Linear Algebra, there needs to be a lot of formal stuff going on in the background. But, I'm too much inconfident on this subject to dare go much deep. So, whenever possible, I'll try to explain everything in intuitive and plain English words. Also, this blog might take a while to be read through, as there are quite a few observations to grasp, and the example problems aren't that easy either. So please be patient and try to go through it all, in several sits if needed. I believe it'll be worth the time. In any case, I've written the solutions, codes, and provided links to their editorials(if available).

Now, the problems that can be solved using this technique are actually not much hard to identify. The most common scenario involves: you'll be given an array of numbers, and then the problem asks for an answer by considering all the xor-sums of the numbers in all possible subsets of the array. This technique can also be used in online-query problems: the problem can provide queries of first type instructing you to insert numbers in the array and in-between those queries, asking for answers in separate queries of second type.

The whole technique can be divided into two main parts, some problems can even be solved by using only the first part(Don't worry if you don't understand them completely now, I explain them in details right below):
1. Represent each given number in it's binary form and consider it as a vector in the $\mathbb{Z}\_2^b$ vector space, where $b$ is the maximum possible number of bits. Then, xor of some of these numbers is equivalent to addition of the corresponding vectors in the vector space.
2. Somehow, relate the answer to the queries of second type with the basis of the vectors found in Part 1.

PS: Does anyone know any name for this technique? I'm feeling awkward referring to it as 'technique' this many times :P If it's not named yet, how about we name it something?

## Part 1: Relating XOR with Vector Addition in $\mathbb{Z}\_2^b$

Let me explain the idea in plain English first, then we'll see what the $\mathbb{Z}\_2^b$ and vector space means. I'm sure most of you have already made this observation by yourselves at some point.

Suppose, we're xor-ing the two numbers $10$ and $12.$ Let's do it below:

$$
\begin{equation*}\begin{array}{r}
1010\\
\underline{\oplus\;1100}\\
0110
\end{array}\end{equation*}
$$

Now, compare for each corresponding pair of bits in the two numbers, compare the result of their xor with the result of their sum taken modulo $2$:

|  Bit no.  | First number | Second number | $\oplus$ | Sum | Sum taken $\pmod 2$ |
|:---------:|:------------:|:-------------:|:--------:|:---:|:-------------------:|
| $1$st bit |      $0$     |      $0$      |    $0$   | $0$ |         $0$         |
| $2$nd bit |      $1$     |      $0$      |    $1$   | $1$ |         $1$         |
| $3$rd bit |      $0$     |      $1$      |    $1$   | $1$ |         $1$         |
| $4$th bit |      $1$     |      $1$      |    $0$   | $2$ |         $0$         |