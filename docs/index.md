# [leetcode_2019](https://impanyu.github.io/leetcode_2019)

## [Code Snippet](https://impanyu.github.io/algorithm_snippet)

## Summary
1. Make good use of map or set
2. Find ways to transform a problem or find its dual problem if possible
3. For each loop, make it clear which states variables are changed each time and what is the 
   a. the condition after quit
   b. the condition before first enter
   c. the condition before each loop
4. For each loop, make as simple as possible of each step of change, do not use parallel update and multiple if statement, it's easy to get confused and bug prone. Try to make the condition judgement simple.
5. string to int: stoi or iss
6. int to string: oss
7. init an array to 0: int ms[128] = {}
   init an array to v:#define FILL(x,v) memset(x,v,sizeof(x))
8. For bfs/dfs/backtracking, there are some infomation to keep through out the process:
    a.graph structure
    b.node visit order
    c.node information
9. Try to form a image for the solution, especially the finally state/solution of the problem if possible.
10. Zero indexed array mid principle: size is odd, n/2 and (n-1)/2 are both mid
                                      size is even, n/2 is right mid, (n-1)/2 is left mid.
11. All programs are dynamic systems in which there are time varying states. Device which those states are and how to shift between each states is essential to algorithm design.
12. Encoding of a pair of number E(a,b) = a*(max_value+1)+b
13. For limited number of elements, a set can be represented by a bitmap
14. For a full complete tree with n layers, the number of nodes is 2^p-1 and the number of nodes in the last layer is 2^(p-1)
15. Utilize those constant quantity in problem, such as limited number of alphabet or limit of numbers in the array to reduce the searching space
16. max_element() for find max element in an array, vector or other container
17. In c++, priority_queue is maximum queue, whereas in java priority_queue is min queue. 
18. There is a type of problem, in which we need to readapt the original spatial order to new spatial order, such as read a matrix/tree in some new order.  
19. __builtin_clz(n) get the number of lead zeros in n's binary representation.
20. For a type of problem which want to get a value a given another value b, and b is monotonous increasing or decreasing with a, we can treat a is a index with b and solve it like a typical binary search problem(1041)
21. Pay attention to over mature loop, where the loop conduct an extra step before quit 
22. tolower() and toupper()
23. isalpha() and isdigit()

## Categorized Problems
1. ### [Two Pointers](two_pointers.md)
2. ### [BT_BST](bt_bst.md)
3. ### [Binary Search](binary_search.md)
4. ### [Math](math.md)
5. ### [String](string.md)
6. ### [DP](dp.md)
7. ### [Bit Manipulation](bit_manipulation.md)
8. ### [Stack](stack.md)
9. ### [Hash_Table](hash_table.md)
10. ### [Array](array.md)
11. ### [Greedy](greedy.md)
