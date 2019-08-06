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

Now, the problems that can be solved using this technique are actually not much hard to identify. The most common scenario involves: you'll be given an array of numbers, and then the problem asks for an answer by considering all the xor-sums of the numbers in all possible subsets of the array. This technique can also be used in some online-query problems: the problem can provide queries of first type instructing you to insert numbers in the array(_without removal_, I don't know how to solve with deletion of elements) and in-between those queries, asking for answers in separate queries of second type.

The whole technique can be divided into two main parts, some problems can even be solved by using only the first part(Don't worry if you don't understand them completely now, I explain them in details right below):
1. Represent each given number in it's binary form and consider it as a vector in the $\mathbb{Z}\_2^b$ vector space, where $b$ is the maximum possible number of bits. Then, xor of some of these numbers is equivalent to addition of the corresponding vectors in the vector space.
2. Somehow, relate the answer to the queries of second type with the basis of the vectors found in Part 1.

PS: Does anyone know any name for this technique? I'm feeling awkward referring to it as 'technique' this many times :P If it's not named yet, how about we name it something?

## Part 1: Relating XOR with Vector Addition in $\mathbb{Z}\_2^b$

Let me explain the idea in plain English first, then we'll see what the $\mathbb{Z}\_2^b$ and vector space means. I'm sure most of you have already made this observation by yourselves at some point.

Suppose, we're xor-ing the two numbers $2$ and $3.$ Let's do it below:

$$
\begin{equation*}\begin{array}{r}
(10)\_b\\
\underline{\oplus\;(11)\_b}\\
(01)\_b
\end{array}\end{equation*}
$$

Now, compare for each corresponding pair of bits in the two numbers, compare the result of their xor with the result of their sum taken modulo $2$:

|  Bit no.  | First number | Second number | $\oplus$ | Sum | Sum taken $\pmod 2$ |
|:---------:|:------------:|:-------------:|:--------:|:---:|:-------------------:|
| $1$st bit |      $0$     |      $1$      |    $1$   | $1$ |         $1$         |
| $2$nd bit |      $1$     |      $1$      |    $0$   | $2$ |         $0$         |

Notice the similarity between columns $4$ and $6$? So, we can see that taking xor between two numbers is essentially the same as, for each bit positions _separately_, taking the sum of the two corresponding bits in the two numbers modulo $2.$

Now, consider a cartesian plane with integer coordinates, where the coordinate values can only be $0$ or $1.$ If any of the coordinates, exceeds $1,$ or goes below $0,$ we simply take it's value modulo $2.$

This way, there can only be $4$ points in this plane: $(0, 0), (0, 1), (1, 0), (1, 1).$ Writing any other pair of coordinates will refer to one of them in the end, for example, point $(3, 2)$ is the same point as point $(1, 0)$ since $3 \equiv 1$ and $2 \equiv 0$ modulo $2.$

In view of this plane, we can represent the number $2 = (10)\_2$ as the point $(1, 0),$ setting the first bit of $2$ as the $y$ coordinate and the second bit as the $x$ coordinate in our plane. Refer to this point as $P(1, 0).$ Then, the position vector of $2$ will be $\overrightarrow{OP}$ where $O(0, 0)$ is the origin. Similarly, the position vector of $3$ will be $\overrightarrow{OQ}$ where $Q = (1, 1).$

An interesting thing happens here, if we add the two position vectors, the corresponding coordinates get added modulo $2,$ which actually gives us the position vector of the xor of these two position vectors. For example, adding vectors $\overrightarrow{OP}$ and $\overrightarrow{OQ}$ we get $\overrightarrow{OR}$ where $R(0, 1)$ turns out to be the point corresponding the xor of $2$ and $3.$

This is all there is to it. Transforming xor operations to bitwise addition modulo $2$ and, in some cases, vector addition in this way can be helpful in some problems. Let's see one such problem. Before that, let me explain in short what vector space and $\mathbb{Z}\_2^b$ meant earlier. I apologize to any Linear Algebra fans, since I don't want to give formal definitions here to make things look harder than it is. I'll explain the idea of these terms the way I find them in my head, kindly pardon me for any mistakes and correct me if I'm wrong.

$\underline{\text{Vector Space}}$: Just a collection of vectors.

$\underline{\mathbb{Z\_2}}$: $\mathbb{Z\_m}$ is the set of remainders upon division by $m.$ So, $\mathbb{Z\_2}$ is simply the set $\{0, 1\},$ since these are the only remainders possible when taken modulo $2.$

$\underline{\mathbb{Z\_2^b}}$: A $b-$dimensional vector space consisting of all the different position vectors that consists of $b$ coordinates, all coordinates being elements of $\mathbb{Z\_2}.$ For example, earlier our custom cartesian plane was a two-dimensional one. So, it was $\mathbb{Z\_2^2}.$ $\mathbb{Z\_2^3}$ would be a small $3d-$plane with only $2^3 = 8$ points, all coordinates taken modulo $2.$

So, the problem:

### Problem 1 ###
---

> Find the number of non-empty subsets of a given set of size $1 \le N \le 10^5$ with range of elements $1 \le a_i \le 70,$ such that the product of it's elements is a square number.
[Link to the problem](https://codeforces.com/contest/895/problem/C)

---

If you'd like to solve the problem first, then pause and try it before reading on further.

Since the number of different possible masks were just $70$ in the previous problem, we had been able to use dynamic programming for checking all possible xors. But what if the constraint was much bigger, say $10^5.$ That is when we can use Part $2$ of this technique, which, in some cases, works even when the queries are online.

## Part 2: Bringing in Vector Basis

