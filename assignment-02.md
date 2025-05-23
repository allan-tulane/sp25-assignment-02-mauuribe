# CMPS 2200 Assignment 2

**Name:** Mauricio Uribe

In this assignment we'll work on applying the methods we've learned to analyze recurrences, and also see their behavior
in practice. As with previous
assignments, some of of your answers will go in `main.py` and `test_main.py`. You
should feel free to edit this file with your answers; for handwritten
work please scan your work and submit a PDF titled `assignment-02.pdf`
and push to your github repository.


## Part 1. Asymptotic Analysis

Derive asymptotic upper bounds of work for each recurrence below.

* $W(n)=2W(n/3)+1$
.  
.  Each split creates two nodes of size n/3. The depth is log3(n), and each level does constant work. Since work doubles every level, it is leaf-dominated. Thus, W = O(n^(log3(2))).
. 
.  
. 
 
* $W(n)=5W(n/4)+n$
.  
.
. Since (5/4)^log(n) grows faster than n, it is leaf-dominated. Therefore, W = O(n^(log4(5))).
. 
.  
. 
.  
.  
. 

* $W(n)=7W(n/7)+n$
.  
. 
. Since (7/7)n = n, the work is balanced. Each level has O(n) work with log n levels. Thus, W = O(n log n).
.  
. 
.  
.

* $W(n)=9W(n/3)+n^2$
.  
.
. Root work is n^2. Each child work is (n/3)^2. Since 9*(n/3)^2 = n^2, the work is balanced across log3(n) levels. Thus, W = O(n^2 log n).
.  
. 
.  
.  
.  
.

* $W(n)=8W(n/2)+n^3$
.  
.
.  
.  Each leaf does n^3 work, and the root also does n^3 work. Therefore, the work is balanced with O(log n) levels. Thus, W = O(n^3 log n).
.  
.  
. 
.  
. 


* $W(n)=49W(n/25)+n^{3/2}\log n$
.  
.  
. 
.  The root n^(3/2) log n dominates the total work because leaves shrink faster. Thus, W = O(n^(3/2) log n).
.  
.  
.  

* $W(n)=W(n-1)+2$
.  
.  
. At each step, 2 units of work are done, so W = O(n).
.  
. 
.  
.  
.  

* $W(n)= W(n-1)+n^c$, with $c\geq 1$
.  
.  
.  
.  At each step, n^c work is done, so total work sums to O(n^(c+1)).
.  
. 
.  
. 

* $W(n)=W(\sqrt{n})+1$
.  
.  
.  Since the size shrinks by square root each time, the depth is log log n, so the total work is O(log log n).
.  
.  
. 
. 


## Part 2. Algorithm Comparison

Suppose that for a given task you are choosing between the following three algorithms:

  * Algorithm $\mathcal{A}$ solves problems by dividing them into
      five subproblems of half the size, recursively solving each
      subproblem, and then combining the solutions in linear time.
    
  * Algorithm $\mathcal{B}$ solves problems of size $n$ by
      recursively solving two subproblems of size $n-1$ and then
      combining the solutions in constant time.
    
  * Algorithm $\mathcal{C}$ solves problems of size $n$ by dividing
      them into nine subproblems of size $n/3$, recursively solving
      each subproblem, and then combining the solutions in $O(n^2)$
      time.

    What are the asymptotic running times of each of these algorithms?
    Which algorithm would you choose?


.  Algorithm A runs in polynomial time, growing on the order of approximately n^2.32
. Algorithm B has exponential growth. Each increase in n doubles the work (approximately), making it infeasible for large n. growing roughly as 2^n
.  Algorithm C’s running time is essentially n^2 times a logarithmic factor. It grows a bit faster than quadratic time but slower than any n^c with c >2. n other words, Algorithm C runs on the order of n^2log n.
.  
. For very large input sizes, Algorithm C is the best choice due to its lowest asymptotic growth rate. It will scale better than Algorithm A and vastly better than Algorithm B as n grows. Algorithm A is a reasonable second choice (polynomial time, though slightly higher degree), whereas Algorithm B should be avoided for large n because of its exponential running time. 
. 
. 



## Part 3: Parenthesis Matching

A common task of compilers is to ensure that parentheses are matched. That is, each open parenthesis is followed at some point by a closed parenthesis. Furthermore, a closed parenthesis can only appear if there is a corresponding open parenthesis before it. So, the following are valid:

- `( ( a ) b )`
- `a () b ( c ( d ) )`

but these are invalid:

- `( ( a )`
- `(a ) ) b (`

Below, we'll solve this problem three different ways, using iterate, scan, and divide and conquer.

**3a. iterative solution** Implement `parens_match_iterative`, a solution to this problem using the `iterate` function. **Hint**: consider using a single counter variable to keep track of whether there are more open or closed parentheses. How can you update this value while iterating from left to right through the input? What must be true of this value at each step for the parentheses to be matched? To complete this, complete the `parens_update` function and the `parens_match_iterative` function. The `parens_update` function will be called in combination with `iterate` inside `parens_match_iterative`. Test your implementation with `test_parens_match_iterative`.


.  
. 



**3b.** What are the recurrences for the Work and Span of this solution? What are their Big Oh solutions?

The work satisfies the recurrence W(n) = W(n-1) + O(1), and the span satisfies S(n) = S(n-1) + O(1).
Thus, both work and span are O(n).

.  
. 



**3c. scan solution** Implement `parens_match_scan` a solution to this problem using `scan`. **Hint**: We have given you the function `paren_map` which maps `(` to `1`, `)` to `-1` and everything else to `0`. How can you pass this function to `scan` to solve the problem? You may also find the `min_f` function useful here. Implement `parens_match_scan` and test with `test_parens_match_scan`

.  
. 



**3d.** Assume that any `map`s are done in parallel, and that we use the efficient implementation of `scan` from class. What are the recurrences for the Work and Span of this solution? 

The work is W(n) = O(n), and the span is S(n) = O(log n).

.  
.  




**3e. divide and conquer solution** Implement `parens_match_dc_helper`, a divide and conquer solution to the problem. A key observation is that we *cannot* simply solve each subproblem using the above solutions and combine the results. E.g., consider '((()))', which would be split into '(((' and ')))', neither of which is matched. Yet, the whole input is matched. Instead, we'll have to keep track of two numbers: the number of unmatched right parentheses (R), and the number of unmatched left parentheses (L). `parens_match_dc_helper` returns a tuple (R,L). So, if the input is just '(', then `parens_match_dc_helper` returns (0,1), indicating that there is 1 unmatched left parens and 0 unmatched right parens. Analogously, if the input is just ')', then the result should be (1,0). The main difficulty is deciding how to merge the returned values for the two recursive calls. E.g., if (i,j) is the result for the left half of the list, and (k,l) is the output of the right half of the list, how can we compute the proper return value (R,L) using only i,j,k,l? Try a few example inputs to guide your solution, then test with `test_parens_match_dc_helper`.



.  
. 





**3f.** Assuming any recursive calls are done in parallel, what are the recurrences for the Work and Span of this solution? What are their Big Oh solutions?

The work satisfies the recurrence W(n) = 2W(n/2) + O(1), and the span satisfies S(n) = S(n/2) + O(1).
Solving the work recurrence gives W(n) = O(n), and solving the span recurrence gives S(n) = O(log n).

.  
. 


 
 


