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

I'm very excited about this blog, as it took me quite a lot of effort and scavenging through the internet to completely grasp the concept of this technique(That's probably because I have almost zero knowledge in Linear Algebra or I'm just plain dumb). So I feel like I genuinely conquered a challenge, and I really want to share it with someone. But there's no way my CP friends circle will believe it, they'll think I'm just trying to show off :P

So here I am, sharing it on CF. I also created a [personal blog](https://drschwad.github.io/), so that if I ever feel like sharing something again(not only about CP), I can write a blog there. I also added this same post [there](https://drschwad.github.io/2019-08-06-z2-space-xor-trick/), you can read it there if you prefer dark theme. I'll be pleased to hear any thoughts on the blog or if I can improve it in some way ^\_^

## Introduction

Since it concerns Linear Algebra, there needs to be a lot of formal stuff going on in the background. But, I'm too much inconfident on this subject to dare go much deep. So, whenever possible, I'll try to explain everything in intuitive and plain English words. Also, this blog might take a while to be read through completely, as there are quite a few observations to grasp, and the example problems aren't that easy either. So please be patient and try to go through it all, in several sits if needed. I believe it'll be worth the time. In any case, I've written the solutions, codes, and provided links to their editorials(if available). I'll provide more details in the solutions tomorrow and put more comments in the codes, since I'm really tired from writing this blog all day.

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

Now, for each corresponding pair of bits in the two numbers, compare the result of their xor with the result of their sum taken modulo $2$:

|  Bit no.  | First number | Second number | $\oplus$ | Sum | Sum taken $\pmod 2$ |
|:---------:|:------------:|:-------------:|:--------:|:---:|:-------------------:|
| $1$st bit |      $0$     |      $1$      |    $1$   | $1$ |         $1$         |
| $2$nd bit |      $1$     |      $1$      |    $0$   | $2$ |         $0$         |

Notice the similarity between columns $4$ and $6$? So, we can see that taking xor between two numbers is essentially the same as, for each bit positions _separately_, taking the sum of the two corresponding bits in the two numbers modulo $2.$

Now, consider a cartesian plane with integer coordinates, where the coordinate values can only be $0$ or $1.$ If any of the coordinates, exceeds $1,$ or goes below $0,$ we simply take it's value modulo $2.$

This way, there can only be $4$ points in this plane: $(0, 0), (0, 1), (1, 0), (1, 1).$ Writing any other pair of coordinates will refer to one of them in the end, for example, point $(3, 2)$ is the same point as point $(1, 0)$ since $3 \equiv 1$ and $2 \equiv 0$ modulo $2.$

In view of this plane, we can represent the number $2 = (10)\_2$ as the point $(0, 1),$ by setting the first bit of $2$ as the $x$ coordinate and the second bit as the $y$ coordinate in our plane. Refer to this point as $P(0, 1).$ Then, the position vector of $2$ will be $\overrightarrow{OP}$ where $O(0, 0)$ is the origin. Similarly, the position vector of $3$ will be $\overrightarrow{OQ}$ where $Q = (1, 1).$

An interesting thing happens here, if we add the two position vectors, the corresponding coordinates get added modulo $2,$ which actually gives us the position vector of the xor of these two position vectors. For example, adding vectors $\overrightarrow{OP}$ and $\overrightarrow{OQ}$ we get $\overrightarrow{OR}$ where $R(1, 0)$ turns out to be the point corresponding the xor of $2$ and $3.$

This is all there is to it. Transforming xor operations to bitwise addition modulo $2$ and, in some cases, vector addition in this way can be helpful in some problems. Let's see one such problem. Before that, let me explain in short what vector space and $\mathbb{Z}\_2^b$ meant earlier. I apologize to any Linear Algebra fans, since I don't want to write formal definitions here to make things look harder than it is. I'll explain the idea of these terms the way I find them in my mind, kindly pardon me for any mistakes and correct me if I'm wrong.

$\underline{\text{Vector Space}}$: Just a collection of vectors.

$\underline{\mathbb{Z\_2}}$: $\mathbb{Z\_m}$ is the set of remainders upon division by $m.$ So, $\mathbb{Z\_2}$ is simply the set $\\{0, 1\\},$ since these are the only remainders possible when taken modulo $2.$

$\underline{\mathbb{Z\_2^b}}$: A $b-$dimensional vector space consisting of all the different position vectors that consists of $b$ coordinates, all coordinates being elements of $\mathbb{Z\_2}.$ For example, earlier our custom cartesian plane was a two-dimensional one. So, it was $\mathbb{Z\_2^2}.$ $\mathbb{Z\_2^3}$ would be a small $3d-$plane with only $2^3 = 8$ points, all coordinates taken modulo $2.$

So, what we've seen is that the xor-operation is equivalent to vector addition in a $\mathbb{Z}\_2^b$ vector space. See how unnecessarily intimidating this simple idea sounds when written in formal math!

Anyways, the problem:

### Problem 1 (Division 2 - C)
---

> Find the number of non-empty subsets, modulo $10^9 + 7,$ of a given set of size $1 \le n \le 10^5$ with range of elements $1 \le a_i \le 70,$ such that the product of it's elements is a square number.  
[Link to the source](https://codeforces.com/contest/895/problem/C)

If you'd like to solve the problem first, then kindly pause and try it before reading on further.

#### Solution

It's obvious that our solution will build on the constraint on $a_i,$ which is just $70.$

For a number to be square, each of it's prime divisors must have an even exponent in the prime factorization of the number. There are only $19$ primes upto $70.$ So, we can assign a mask of $19$ bits to each array element, denoting if the $i$'th prime occurs odd or even number of times in it by the $i$'th bit of the mask.

So, the problem just boils down to finding out the number of non-empty subsets of this array for which the xor-sum of it's elements' masks will be $0.$

I got stuck here for quite a while ;-; We can try to use dynamic programming, $\text{dp[at][msk]}$ states the number of subsets in $\\{a_1, a_2, \ldots, a_{\text{at}}\\}$ such that the xor-sum of it's elements' masks is $\text{msk}.$ Then,

$$
\text{dp[at][msk] = dp[at - 1][msk] + dp[at - 1][msk ^ mask[at]]}
$$

with the initial value $\text{dp[0][0] = 1}.$ The final answer would be, $\text{dp[n][0]}.$

But, the complexity is $O(n \cdot 2^{19}),$ which is way too high :(

The thing to notice here, is that, even if $n \le 10^5,$ the actual number of different possible $a_i$ is just $70.$ So, if we find the dp for these $70$ different masks, and if for each $1 \le \text{at} \le 70$ know the number of ways to select odd/even number of array elements with value $\text{at},$ then we can easily count the answer with the following dp:

$$
\text{dp[at][msk] = dp[at - 1][msk] * poss[at][0] + dp[at - 1][msk ^ mask[at]] * poss[at][1]}
$$

where, $\text{poss[at][0]}$ is the number of ways to select even number of array elements with value $\text{at},$ and similarly $\text{poss[at][1]}$ for odd number of elements.

#### Reference Code

{% highlight cpp linenos %}
#include <bits/stdc++.h>
 
using namespace std;
 
const int N = 1e5 + 10;
const int MAX_A = 70;
const int TOTAL_PRIMES = 19;
const int MOD = 1e9 + 7;
 
int n;
int poss[MAX_A + 1][2];
const int primes[] = {2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67};
int mask[MAX_A + 1];
int dp[MAX_A + 1][1 << TOTAL_PRIMES];
 
int main() {
	cin >> n;
 
	for (int i = 1; i <= MAX_A; i++) poss[i][0] = 1;
 
	for (int i = 1; i <= n; i++) {
		int a;
		scanf("%d", &a);
 
		int tmp = poss[a][0];
		poss[a][0] = (poss[a][0] + poss[a][1]) % MOD;
		poss[a][1] = (poss[a][1] + tmp) % MOD;
	}
 
	for (int i = 1; i <= MAX_A; i++) {
		for (int p = 0; p < TOTAL_PRIMES; p++) {
			int cnt = 0;
			int at = i;
 
			while (at % primes[p] == 0) {
				at /= primes[p];
				cnt++;
			}
 
			if (cnt & 1) mask[i] |= (1 << p);
		}
	}
 
	int max_mask = 1 << TOTAL_PRIMES;
	dp[0][0] = 1;
	
	for (int at = 1; at <= MAX_A; at++)
		for (int msk = 0; msk < max_mask; msk++) {
			dp[at][msk] = dp[at - 1][msk] * 1LL * poss[at][0] % MOD;
 
			dp[at][msk] += dp[at - 1][msk ^ mask[at]] * 1LL * poss[at][1] % MOD;
			dp[at][msk] %= MOD;
		}
 
	cout << (dp[MAX_A][0] + MOD - 1) % MOD << endl;
 
	return 0;
}
{% endhighlight %}

---

Since the number of different possible masks were just $70$ in the previous problem, we had been able to use dynamic programming for checking all possible xors. But what if the constraint was much bigger, say $10^5.$ That is when we can use Part $2$ of this technique, which, in some cases, works even when the queries are online.

## Part 2: Bringing in Vector Basis

We need a couple of definitions now to move forward. All the vectors mentioned in what follows, exclude null vectors. I sincerely apologize for being so informal with these definitions.

$\underline{\text{Independent Vectors:}}$ A set of vectors $\vec{v_1}, \vec{v_2}, \ldots, \vec{v_n}$ is called independent, if none of them can be written as the sum of a linear combination of the rest.

$\underline{\text{Basis of a Vector Space:}}$ A set of vectors is called a basis of a vector space, if all of the element vectors of that space can be written _uniquely_ as the sum of a linear combination of elements of that basis.

A few important properties of independent vectors and vector basis that we will need later on(I find these pretty intuitive, so I didn't bother with reading any formal proofs. Let me know in the comments if you need any help):

1. For a set of independent vectors, we can change any of these vectors by adding to it any linear combination of all of them, and the vectors will still stay independent. What's more fascinating is that, the set of vectors in the space representable by some linear combination of this independent set stays exactly the same after the change.

2. Notice that, in case of $\mathbb{Z}\_2^b$ vector space, the coefficients in the linear combination of vectors must also lie in $\mathbb{Z}\_2.$ Which means that, an element vector can either stay or not stay in a linear combination, there's no in-between.

3. The basis is actually the smallest sized set such that all other vectors in the vector space are representable by a linear combination of just the element vectors of that set.

4. The basis vectors are independent.

5. For any set with smaller number of independent vectors than the basis, not all of the vectors in the space will be representable.

6. And there cannot possibly be larger number of independent vectors than basis in a set. If $b$ is the size of the basis of a vector space, then the moment you have $b$ independent vectors in a set, it becomes a basis. You cannot add another vector into it, since that new vector is actually representable using the basis.

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
int basis[b]; // basis[i] keeps the index of the vector whose f value is i

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

### Problem 2
---

> Given a set $S$ of size $1 \le n \le 10^5$ with elements $1 \le a_i \lt 2^{20}.$ Find the number of distinct integers that can be represented using xor over the set of the given elements.  
[Link to the source](https://codeforces.com/blog/entry/60003)

#### Solution

Think of each element as a vector of dimension $d = 20.$ Then the vector space is $\mathbb{Z}\_2^{20}.$ We can find it's basis in $O(d \cdot n).$ For any linear combination of the basis vectors, we get a different possible xor of the set. So, the answer would be $2^\text{size of basis}.$ It would fit in an integer type, since size of basis $\le d = 20$ by property $7.$

#### Reference Code

{% highlight cpp linenos %}
#include <bits/stdc++.h>
 
using namespace std;
 
const int N = 1e5 + 10, LOG_A = 20;

int basis[LOG_A];

int sz;

void insertVector(int mask) {
	for (int i = 0; i < LOG_A; i++) {
		if ((mask & 1 << i) == 0) continue;

		if (!basis[i]) {
			basis[i] = mask;
			++sz;
			
			return;
		}

		mask ^= basis[i];
	}
}

int main() {
	int n;
	cin >> n;

	while (n--) {
		int a;
		scanf("%d", &a);

		insertVector(a);
	}

	cout << (1 << sz) << endl;

	return 0;
}
{% endhighlight %}

---

### Problem 3
---

> What is the maximum possible xor for the elements of a subset from a given set?  
[Link to the source](https://codeforces.com/blog/entry/60003)

#### Solution

In this problem, we need to slightly alter the definition of $f(\vec{b}).$ Instead of $f$ being the first position with a set bit, let it be the last position with a set bit.

Now, to get the maximum, we initialize our ```answer``` at 0 and we start iterating the basis vectors starting with the one that has the highest value of $f.$

Suppose, we're at base vector $\vec{b}$ and we find that ```answer``` doesn't have the $f(\vec{b})$'th bit set, then we add $\vec{b}$ with ```answer```. This greedy solution works because $f(\vec{b})$ is the most significant bit at the moment, and we must set it; doesn't matter if all the following bits turn to $0.$

#### Reference Code

{% highlight cpp linenos %}
#include <bits/stdc++.h>
 
using namespace std;
 
const int N = 1e5 + 10, LOG_A = 20;

int basis[LOG_A];

void insertVector(int mask) {
	for (int i = LOG_A - 1; i >= 0; i--) {
		if ((mask & 1 << i) == 0) continue;

		if (!basis[i]) {
			basis[i] = mask;
			return;
		}

		mask ^= basis[i];
	}
}

int main() {
	int n;
	cin >> n;

	while (n--) {
		int a;
		scanf("%d", &a);

		insertVector(a);
	}

	int ans = 0;

	for (int i = LOG_A - 1; i >= 0; i--) {
		if (!basis[i]) continue;

		if (ans & 1 << i) continue;

		ans ^= basis[i];
	}

	cout << ans << endl;

	return 0;
}
{% endhighlight %}

---

### Problem 4 (1st Hunger Games - S)
---

> We have an empty set $S$ and we are to do $1 \le n \le 10^6$ queries on it. Let, $X$ denote the set of all possible xor-sums of elements from a subset of $S.$ There are two types of queries.  
Type $1$: Insert an element $1 \le k \le 10^9$ to the set(If it's already in the set, do nothing)  
Type $2$: Given $k,$ print the $k$'th hightest number from $X.$ It's guaranteed that $k \le \mid X \mid.$
[Link to the source](https://codeforces.com/group/qcIqFPYhVr/contest/203881/problem/S)

#### Solution

A bit like the previous one. For query type $2,$ again we'll iterate through the basis vectors according to their decreading order of $f$ values. Suppose $\vec{b}$ is the one with the hightest $f$ value. Initially we know there are $2^\text{basis size}$ elements in $X.$ So, if $k <= \frac{2^\text{basis size}}{2},$ we set the $f(\vec{b})$'th bit of ```answer``` to $0$ and otherwise to $1.$

#### Reference Code
{% highlight cpp linenos %}
#include <bits/stdc++.h>
 
using namespace std;
 
const int N = 1e6 + 10, LOG_K = 30;
 
int basis[LOG_K], sz;
 
void insertVector(int mask) {
	for (int i = LOG_K - 1; i >= 0; i--) {
		if ((mask & 1 << i) == 0) continue;
 
		if (!basis[i]) {
			basis[i] = mask;
			sz++;
 
			return;
		}
 
		mask ^= basis[i];
	}
}
 
int query(int k) {
	int mask = 0;
 
	int tot = 1 << sz;
	for (int i = LOG_K - 1; i >= 0; i--)
		if (basis[i]) {
			int low = tot / 2;
 
			if ((low < k && (mask & 1 << i) == 0) ||
				(low >= k && (mask & 1 << i) > 0)) mask ^= basis[i];
 
			if (low < k) k -= low;
 
			tot /= 2;
		}
 
	return mask;
}
 
int main() {
	int n;
	cin >> n;
 
	while (n--) {
		int t, k;
		scanf("%d %d", &t, &k);
 
		if (t == 1) insertVector(k);
		else printf("%d\n", query(k));
	}
 
	return 0;
}
{% endhighlight %}

---

### Problem 5 (Division 2 - F)
---

> You're given an array $1 \le a_i \lt 2^{20}$ of length $1 \le n \le 10^5.$ You have to answer $1 \le q \le 10^5$ queries.  
In each query you'll be given two integers $1 \le l \le n$ and $0 \le x \lt 2^{20}.$ Find the number of subsequences of the first $l$ elements of the array, modulo $10^9 + 7,$ such that their bitwise-xor sum is $x.$  
[Link to the source](https://codeforces.com/contest/959/problem/F)

---

### Problem 6 (Education Round - G)
---

> You are given an array $0 \le a_i \le 10^9$ of $1 \le n \le 2 \cdot 10^5$ integers. You have to find the maximum number of segments this array can be partitioned into, such that -  
    1. Each element is contained in exactly one segment  
    2. Each segment contains at least one element  
    3. There doesn't exist a non-empty subset of segments such that bitwise-xor of the numbers from them is equal to $0$  
Print $-1$ if no suitable partition exists.  
[Link to the source](https://codeforces.com/contest/1101/problem/G)

---

## References:

1. [2 Special cases of Gaussian [Tutorial] by AJ_Coder](https://codeforces.com/blog/entry/60003)

2. [General ideas by adamant](https://codeforces.com/blog/entry/48417?locale=en)

3. [Helpful comments for Problem 6 by mohamedeltair](https://codeforces.com/blog/entry/64483?#comment-510750)

4. [Solution idea using this trick to Problem 5 by loomas](https://codeforces.com/blog/entry/58712?#comment-423243)
