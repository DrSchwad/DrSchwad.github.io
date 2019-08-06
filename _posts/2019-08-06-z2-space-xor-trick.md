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
(10)_2\\
\underline{\oplus\;(11)_2}\\
(01)_2
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

In view of this plane, we can represent the number $2 = (10)\_2$ as the point $(0, 1),$ by setting the first bit of $2$ as the $x$ coordinate and the second bit as the $y$ coordinate in our plane. Refer to this point as $P(0, 1).$ Then, the position vector of $2$ will be $\overrightarrow{OP}$ where $O(0, 0)$ is the origin. Similarly, the position vector of $3$ will be $\overrightarrow{OQ}$ where $Q = (1, 1).$

An interesting thing happens here, if we add the two position vectors, the corresponding coordinates get added modulo $2,$ which actually gives us the position vector of the xor of these two position vectors. For example, adding vectors $\overrightarrow{OP}$ and $\overrightarrow{OQ}$ we get $\overrightarrow{OR}$ where $R(1, 0)$ turns out to be the point corresponding the xor of $2$ and $3.$

This is all there is to it. Transforming xor operations to bitwise addition modulo $2$ and, in some cases, vector addition in this way can be helpful in some problems. Let's see one such problem. Before that, let me explain in short what vector space and $\mathbb{Z}\_2^b$ meant earlier. I apologize to any Linear Algebra fans, since I don't want to give formal definitions here to make things look harder than it is. I'll explain the idea of these terms the way I find them in my head, kindly pardon me for any mistakes and correct me if I'm wrong.

$\underline{\text{Vector Space}}$: Just a collection of vectors.

$\underline{\mathbb{Z\_2}}$: $\mathbb{Z\_m}$ is the set of remainders upon division by $m.$ So, $\mathbb{Z\_2}$ is simply the set $\{0, 1\},$ since these are the only remainders possible when taken modulo $2.$

$\underline{\mathbb{Z\_2^b}}$: A $b-$dimensional vector space consisting of all the different position vectors that consists of $b$ coordinates, all coordinates being elements of $\mathbb{Z\_2}.$ For example, earlier our custom cartesian plane was a two-dimensional one. So, it was $\mathbb{Z\_2^2}.$ $\mathbb{Z\_2^3}$ would be a small $3d-$plane with only $2^3 = 8$ points, all coordinates taken modulo $2.$

So, the problem:

### Problem 1
---

> Find the number of non-empty subsets of a given set of size $1 \le N \le 10^5$ with range of elements $1 \le a_i \le 70,$ such that the product of it's elements is a square number.  
[Link to the problem](https://codeforces.com/contest/895/problem/C)

---

If you'd like to solve the problem first, then pause and try it before reading on further.

Since the number of different possible masks were just $70$ in the previous problem, we had been able to use dynamic programming for checking all possible xors. But what if the constraint was much bigger, say $10^5.$ That is when we can use Part $2$ of this technique, which, in some cases, works even when the queries are online.

## Part 2: Bringing in Vector Basis

We need a couple of definitions now to move forward. All the vectors mentioned in what follows, exclude null vectors. I sincerely apologize for being so informal with these definitions.

$\underline{\text{Independent Vectors:}}$ A set of vectors $\vec{v_1}, \vec{v_2}, \ldots, \vec{v_n}$ is called independent, if none of them can be written as the sum of a linear combination of the rest.

$\underline{\text{Basis of a Vector Space:}}$ A set of vectors is called a basis of a vector space, if all of the element vectors of that space can be written _uniquely_ as the sum of a linear combination of elements of that basis.

A few important properties of independent vectors and vector basis that we will need later on(I find these pretty intuitive, so I didn't bother with reading any formal proofs. Let me know if you're finding them hard to digest):

1. For a set of independent vectors, we can replace any one of them with a linear combination of them all. What's more fascinating is that, the set of vectors in the space representable as a linear combination of this independent set stays exactly the same with the new independent set of vectors.

2. Notice that, in case of $\mathbb{Z}\_2^b$ vector space, the coefficients in the linear combination of vectors must also lie in $\mathbb{Z}\_2.$ So, to iterate all the linear combinations of some set of vectors, you just take an element vector or don't take it in the combination, there's no in-between.

3. The basis is actually the smallest sized set such that all other vectors are representable using just the element vectors of that set.

4. The basis vectors are independent.

5. For any set with smaller number of independent vectors than the basis, not all of the vectors in the space will be representable.

6. And for any set with larger number of independent vectors, even if all of the vectors in that space can be represented using it's element vectors, the representation will not be unique.

7. For a $d-$dimensional vector space, it's basis can have at most $d$ vector elements.

With just these few properties, we can experience some awesome solutions to a few hard problems. But first, we need to see how we can efficiently find the basis of a vector space of $n$ vectors, where each vector is an element of $\mathbb{Z}\_2^b.$ The algorithm is quite awesome <3 And it works in $O(n \cdot b).$

### The Algorithm:
---

> This algorithm extensively uses properties $1$ and $4,$ and the rest in the background.  
Suppose at each step, we're taking an input vector $\vec{v_i}$ and we already have a basis of the previously taken vectors $\vec{v_1}, \vec{v_2}, \ldots, \vec{v_{i - 1}},$ and we need to update the basis such that it can also represent the new vector $vec{v_i}.$  
In order to do that, we first need to check if $vec{v_i}$ is representable using our current basis. If it is, then this basis is enough and we don't need to do anything. But if it's not, then we just add this vector $vec{v_i}$ to the set of basis.  
So the only difficuly is, to efficiently check whether the new vector is representable by the basis or not. In order facilitate that, we use property $1$ before inserting any new vectors in the basis, to slightly modify it, without causing any harm to the basis. This way, we can have more control over the form of our basis vectors. So here's the plan:  
Let, $f(\vec{v})$ be the first position in it's binary notation, where the bit is set. We make sure that all the basis vectors' each have a different $f$ value.  
Here's how we do it. Initially, there's no vector in the basis, so we're fine, there's no $f$ value to collide with each other. Now, suppose we're at the $i$'th step, and we're checking if vector $\vec{v_i}$ is representable by the basis or not. Since, all of our basis have a different $f$ value, take the one with the lease $f$ value among them, let's call this basis vector $\vec{b_1}.$  
If $f(\vec{v_i}) < f(\vec{b_1})$ then no matter how we take the linear combination, by property $2,$ no linear combination of the basis vectors' can have $1$ at $f(\vec{v_i}).$ So, it'll be a new basis vector, and since it's $f$ is already different from the rest of the basis vectors, we can insert it into the set as is and keep a record of it's $f$ value.
But, if $f(\vec{v_i}) == f(\vec{b_1}),$ then we must take $\vec{b_1}$ if we want to represent $\vec{v_i}$ by a linear combination, since no other basis vector has $1$ at $f(\vec{v_i}) = f(\vec{b_1}).$ So, we delete $\vec{b_1}$ from $\vec{v_i}$ and move on to $\vec{b_2}.$ Notice that, by changing the value of $\vec{v_i}$ no harm is being done if in some later step we find out $\vec{v_i}$ is actually not representable, we can still insert it with it's new value according to property $1.$
If, after iterating through all the basis vector $\vec{b}$'s and subtracting them from $\vec{v_i}$ when needed, we still find out that $\vec{v_i}$ is not null vector, it means that the new $\vec{v_i}$ has a larger value of $f$ than all other basis vectors. So we insert it into the basis and keep a record of it's $f$ value.  
Here's the implementation, the vectors being represented by bitmasks:

{% highlight cpp linenos %}
int basis[i]; // basis[i] keeps the index of the vector whose f value is i

int sz; // Current size of the basis

void insertVector(int mask) {
	for (int i = 0; i < b; i++) {
		if ((mask & 1 << i) == 0) continue; // i != f(mask), so continue

		if (!basis[i]) { // If there is no basis vector with the i'th bit set, then insert mask into the basis
			basis[i] = mask;
			++sz;
			
			return;
		}

		mask ^= basis[i]; // Otherwise subtract the basis vector from mask
	}
}
{% endhighlight %}

---

Let's view some problems now: