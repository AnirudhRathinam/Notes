## Contents

- [Overview](#Overview)
- [Recursion](#Recursion)
  * [Recursive Thinking](#Recursive-thinking)
  * [Recursion in Memory](#Recursion-in-memory)
  * [Recursion and Iteration](#Recursion-and-Iteration)
- [Heading](#heading-2)
  * [Sub-heading](#sub-heading-2)
    + [Sub-sub-heading](#sub-sub-heading-2)


## Overview

Dynamic Programming is basically optimizing recursion. There are some recurrences where the same exact computations are done multiple times. Dynamic Programming speeds things up by storing results after they are computed so we dont compute them all over again.

To solve a recurrence using DP, the recurrence must show the properties of overlapping subproblems and optimal substructure - these properties will be covered in the Introduction to DP section. Since DP is about solving recurrences, we will first look at recursive thinking.

------------------------

## Recursion

### Recursive Thinking

**Recursion and Mathematical Induction**

The idea behind recursion is the same as mathematical induction and strong mathematical induction. Following the same concept as mathematical induction, for recursion we apply the following thought process:

- Let's say we want to write a function to solve a problem where the input is x. The function that we need to write is f(x).
- We assume that f(...) always returns the correct answer for any values less than input x.
  - So f(x-1) returns the correct answer, f(x-2) returns the correct answer and so on
- We use this to write the function f(x).

So to solve a problem with recursion we do the following:

1. If I can solve the program in one step, I solve it. This is the base case
2. Otherwise I do just 1 step of the problem to reduce the size and then call the recursive function on the remaining unsolved problem.
 - Here we assume the recursive function we call for the remaining unsolved portion of the problem always returns the correct result because the problem size is smaller than n
 - The reason this works is the same reason induction works.
 
 ------------------
 
 **A simple problem solved using recursion**

**Problem statement:** Find the largest element in a non empty array of integers recursively. For example if a = [1,6,3,5,8,2,4,9] the result should be 9.

**Solution**

We are given an array of 'n' elements. We need to return the largest element in those 'n' elements as the answer.

Let us consider the approach for recursion.

1. If the problem can be solved in 1 step, solve it.
 - In this case if the array has only 1 element (n == 1), then the largest element is the only element and we can return it.
 - So if pointers i = start of the array and j = end of the array, if i == j then there is just one element. In that case we return a[i].
2. Otherwise do just 1 step of the problem to reduce the size and then call the recursive function on the remaining problem
 - We know based on our assumption that f(n-1), f(n-2) ... all return the correct answer.
 - So if pointers i = start of the array and j = end of the array, the answer is max( a[i], f(i+1, j) ).
 - We know f(i+1, j) returns the correct answer because the problem size here is n-1.

The code is as follows:

    public int solve(int[] a, int i, int j) {
        if(i==j)
            return a[i];
        else
            return Math.max(a[i], solve(a, i+1, j));
    }

----------------------------------

**A slightly more complex problem solved using recursion**

**Problem statement:** Solve the [tower of hanoi](https://www.geeksforgeeks.org/c-program-for-tower-of-hanoi/) problem recursively. If you are unfamiliar with the problem use the link or just google it - it is a famous problem.

**Solution**

We are given 'n' disks which are on the first rod (I will call it src). We need to move all 'n' disks to the target bar (I will call it tgt) with the help of a temporary bar (I will call it tmp).

Let us consider the approach for recursion.

1. If the problem can be solved in 1 step, solve it.
 - If the number of disks we need to move from src to tgt == 0 then there is no work to do. The problem is solved
2. Otherwise do just 1 step of the problem to reduce the size and then call the recursive function on the remaining problem
 - Let f(n, A, B, C) be our function. This function takes n disks from A and puts them on C such that it obeys the rules.
 - So we know f(n-1, A, B, C) works, we know f(n-2, A, B, C) works and so on based on the assumption that the function just works for all smaller input sizes.
3. So we do the following:
 - Take the first n-1 disks, and put them from src to tmp by calling f(n-1, src, tgt, tmp)
 - Next we take the nth disk and add it to tgt
 - Then we take the n-1 disks from tmp and add it to tgt by calling f(n-1, tmp, src, tgt)

And thats it... we have solved the problem. The code is below:

    public void solve() {
        // Move disks from A to C following rules of tower of hanoi
        int[] a = new int[] {0,1,2,3,4,5,6,7,8};
        int[] b = new int[9];
        int[] c = new int[9];

        hanoi(9, a, b, c);
    }

    private void hanoi(int n, int[] src, int[] tmp, int[] tgt) {
        if(n==0)
            return;
        else {
            hanoi(n-1, src, tgt, tmp);    // move n-1 disks from src to tmp
            tgt[n-1] = src[n-1]; src[n-1] = 0;    // move last disk from src to tgt
            hanoi(n-1, tmp, src, tgt);    // move n-1 disks from tmp to tgt
        }
    }


### Recursion in Memory

### Recursion and Iteration
