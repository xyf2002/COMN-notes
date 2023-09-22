3 main techniques
1. Divide and conquer
	* By dividing a problem into smaller sub-problems we can eventually reach a simple base case
2. Greedy approaches
	* Take the immediate best option at each decision point
	* We must prove that the immediate best option is also the best overall
3. Dynamic programming
	* By storing the result for each divide and conquer sub-problem we can avoid recomputing results several times
	* This is applicable in cases where we have a lot of recursion, but there are only a limited number of results. e.g. `f(x, y) -> z` where `0 <= x, y <= 50`
	* Trades off increased memory to decrease computation time

# Basic notions
* **Model of computation** - An abstract sequential computer, called a *Random Access Machine* or *RAM*. Uniform cost for all instructions (not the case IRL!)
* **Computational problem** - A specification of inputs and outputs and the desired relation between them
* **Problem instance** - A particular collection of inputs for a given problem
* **Algorithm** - A method for solving a problem which can be implemented on a computer. There are generally many algorithms for a problem, we will be reasoning about which are "good" (efficient by some metric) or "bad" and how they compare.
* **Program** - A specific implementation of an algorithm

# Run time
* We define this as the number of computation steps performed by the algorithm on a particular instance
	* This depends on our machine model
* Another metric is the number of basic arithmetic operations performed by that algorithm, this ignores things like accessing memory, so only counts the "essential" computation steps
* Both these approaches are abstractions of the actual run time, which is dependent on factors such as the quality of the compiler and the physical hardware used to execute the program

## Worst case run time
The worst-case run time of an algorithm $A$ is the function $T_A:\mathbb{N} \rightarrow \mathbb{N}$ where $T_A(n)$ is the maximum number of computation steps performed by $A$ on an input of size $n$.

We can create a similar definition for other measurements, e.g. space.

## Average case run time
The average-case run time of an algorithm $A$ is the function $AVT_A:\mathbb{N} \rightarrow \mathbb{N}$ where $AVT_A(n)$ is the average number of computation steps performed by $A$ on an input of size $n$.

For a true average-case analysis we would need to know the probability of each input of size $n$ turning up. Usually we assume that all inputs of size $n$ are equally likely.

# Bounds
Given a problem, a function $T(n)$ is an:
* **Upper bound** if there is an algorithm which solves the problem and has a worst-case run time of at most $T(n)$
* **Average-case bound** if there if there is an algorithm which solves the problem and has a average-case run time of at most $T(n)$
* **Lower bound** if every algorithm which solves the problem must use at least $T(n)$ time on some instances of size $n$ 

# Example - remainder of a power
**Problem:** find the remainder of a power
**Input:** Integers $a,m,n$ with $n\geq 1$, $m>1$ 
**Output:** the remainder of $a^m$ divided by $m$, i.e. $a^n\mod{m}$ 

## Attempt 1
The naive approach is to simply calculate $a^n$ then mod $m$

```python
def powerRem1(a, n, m):
	r = a
	for j in range(2, n):
		r = r * a
	return r % m
```

This will almost certainly not work. Even with a small $a,m$ and moderate $n$ there will likely be an overflow
Even without an overflow (e.g. with bignums) $a^n$ will very likely become extremely large, and lead to poor performance.

## Attempt 2
```python
def powerRem2(a, n, m):
	x = a % m
	r = x
	for j in range(2, n):
		r = r * x % m
	return r
```

This is much better, $r$ is always less than $m$ so there is little chance of an integer overflow (except in the case where $(m-1)^2$  is large enough to overflow).
This is still not optimal, as the loop will run $n - 2$ times.

## Attempt 3
```python
def powerRem3(a, n, m):
	if n == 1:
		return a % m
	if n % 2 == 0:
		r = powerRem3(a, n/2, m)
		return r ** 2 % m
	r = powerRem3(a, (n - 1)/2, m)
	return (r ** 2 % m) * a % m
```
This keeps the numbers small, but also avoids the loop, instead the function will only execute $\lg{n}$ times.