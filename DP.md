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

-----------------

### Recursion in Memory


**Introduction**

It is very easy to visualize a recursive program in terms of induction to understand how it works. However 'under the hood' recursive programs run with the help of a stack.

Every time a function is called, it is placed on the top of the stack. Once the function finishes its execution, it is popped from the stack and its return value is returned to its caller which is the next element of the stack that was just under it.

The program finishes execution when the stack becomes empty. The objective of this post is to simulate these series of steps using a stack so we can solve any recursive program iteratively.

To do this, I will be using the three depth first traversals as an example - pre-order, in-order and post-order traversals. We will be performing all 3 traversals iteratively using the exact same technique. This technique can be extended to any recursive function.

-------------

**The General Idea**

When you have a recursive function we can have 'n' calls in addition to the current function's action. Let's look at the 3 traversals in greater detail and see what is going on.

--------

**Pre-Order Traversal**

In the case of pre-order traversal we do the following:

    call the initial function
    perform the function action // explore the root
    make recursive call 1 // explore left subtree
    make recursive call 2 // explore right subtree

Here the computer does the following:

- Place the function on the stack
 - The stack is: [calling function]
- Now we do some action such as processing the root val a certain way etc.
 - The stack is: [calling function]
- When we are done, call recursive function 1. 
 - The stack is: [calling function -> recursive function 1]
- It fully processes recursive function 1 then pops it.
 - The stack is: [calling function]
- Next the stack calls recursive function 2
 - The stack is: [calling function -> recursive function 2]
- The stack processes function 2 and pops it
 - The stack is: [calling function]
- Since there is nothing left to do in the calling function, the stack pops calling function
 - The stack is: []
- Stack is empty so we return.

-----

**In-order Traversal**

In the case of in-order we do the following:

    call the initial function
    make recursive call 1 // explore left subtree
    perform the function action // explore the root
    make recursive call 2 // explore right subtree

Here the computer does the following:

- Place the function on the stack
 - The stack is: [calling function]
- Call recursive function 1. 
 - The stack is: [calling function -> recursive function 1]
- It fully processes recursive function 1 then pops it.
 - The stack is: [calling function]
- Now we do some action such as processing the root val a certain way etc.
 - The stack is: [calling function]
- Next the stack calls recursive function 2
 - The stack is: [calling function -> recursive function 2]
- The stack processes function 2 and pops it
 - The stack is: [calling function]
- Since there is nothing left to do in the calling function, the stack pops calling function
 - The stack is: []
- Stack is empty so we return.

----------

**Post-Order Traversal**

In the case of post order traversal we do the following:

    call the initial function
    make recursive call 1 // explore left subtree
    make recursive call 2 // explore right subtree
    perform the function action // explore the root

Here the computer does the following:

- Place the function on the stack
 - The stack is: [calling function]
- Call recursive function 1. 
 - The stack is: [calling function -> recursive function 1]
- It fully processes recursive function 1 then pops it.
 - The stack is: [calling function]
- Next the stack calls recursive function 2
 - The stack is: [calling function -> recursive function 2]
- The stack processes function 2 and pops it
 - The stack is: [calling function]
- Now we do some action such as processing the root val a certain way etc.
 - The stack is: [calling function]
- Since there is nothing left to do in the calling function, the stack pops calling function
 - The stack is: []
- Stack is empty so we return.

------------

**Observations**

The three recurrences above despite being fairly different are processed in a very similar manner. Some observations can be made about this which are as follows:

1. The caller which has to be processed is the first element on the stack
2. The calling function is popped off the stack only after all its sub-problems (functions it calls) have been popped. It is the last to leave the stack.
3. Each time we process the calling function, the calling function is aware of its own state.
 1. This means it knows what sub-functions it has called, it knows when to execute its own function content, it knows what sub-function to call next and it knows when it should stop.
4. If the stack is empty, it means the caller (and all its sub-functions) have been processed and we are finally done.

What this means is that if we were to simulate the process above in order to convert any recursive function into an iterative process we need to do the same.

1. We must keep track of the state of the calling function and when a subfunction has been processed the state must change accordingly
2. The state that holds the result should be the first element in the stack and the last element out of it.

-------------

### Recursion and Iteration

**Introduction**

Here we will look at a generic method to convert any recursive function into an iterative process. To do so, we will take a look at the three types of dfs tree traversals - preorder, inorder and postorder traversals and convert them into an iterative program in the same exact way. We will do this by simulating how recursion is handled in memory using a stack.

**The Pattern**

**Note to self:** write this in a better way. The way its described now is horrible.

1. Maintain 2 stacks (or two slots on the same stack). The first stack should hold the input of the calling function (this represents the caller) and the second stack should hold a state representing the first recursive call.
 1. Note: The states held in the state stack are states representing the current caller and all the recursive calls the caller makes.
 2. So for traversal, the current caller state = root and the recursive call states = left and right
2. While the stack is not empty do the following:
 1. Pop the stacks and get function, state to be processed.
 2. Look at the state. If a function is to be called recursively, first push the current function in the stack with a state representing its next action and then push the function to be called along with its initial state.
 3. If the state indicates that the current node is to be processed process it.
3. Repeat this until stack is empty.

We will use this pattern to solve the three traversals the same way.

----------

In the case of the traversals, the states are root, left and right. The programs for all 3 are shown below. To keep the programs short and concise I will do the following:

I will declare the function stack and the state stack as global variables:
    
    Stack<Node> functionStack = new Stack<>();
    Stack<String> stateStack = new Stack<>();


And I will declare an helper function to push to both stacks as follows:

    private void push(Node node, String state) {
        functionStack.push(node);
        stateStack.push(state);
    }


-----------

**Preorder Traversal**

The iterative version of preorder traversal using the pattern described above is as follows:

    // root left right
    private void preorder(Node root) {
        push(root, "root");
        while (!functionStack.isEmpty()) {
            Node current = functionStack.pop();
            String state = stateStack.pop();

            if(current == null)
                continue;

            if(state.equals("root")) {
                System.out.println(current.val);
                push(current, "left");
            } else if(state.equals("left")) {
                push(current, "right");
                push(current.left, "root");
            } else if(state.equals("right")) {
                push(current, "end");
                push(current.right, "root");
            }
        }
    }

---------

**Inorder Traversal**

The iterative version of inorder traversal using the pattern described above is as follows:

    // left root right
    private void inorder(Node root) {
        push(root, "left");
        while(!functionStack.isEmpty()) {
            Node current = functionStack.pop();
            String state = stateStack.pop();

            if(current == null)
                continue;

            if(state.equals("left")) {
                push(current, "root");
                push(current.left, "left");
            } else if(state.equals("root")) {
                System.out.println(current.val);
                push(current, "right");
            } else if(state.equals("right")) {
                push(current, "end");
                push(current.right, "left");
            }
        }
    }

--------

**Postorder Traversal**

The iterative version of postorder traversal using the pattern described above is as follows:

    // left right root
    private void postorder(Node root) {
        push(root, "left");
        while (!functionStack.isEmpty()) {
            Node current = functionStack.pop();
            String state = stateStack.pop();

            if(current == null)
                continue;

            if(state.equals("left")) {
                push(current, "right");
                push(current.left, "left");
            } else if(state.equals("right")) {
                push(current, "root");
                push(current.right, "left");
            } else if(state.equals("root")) {
                System.out.println(current.val);
                push(current, "end");
            }
        }
    }
