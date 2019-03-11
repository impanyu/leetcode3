# leetcode3
# Summary
1. Make good use of map or set
2. Find ways to transform a problem or find its dual problem if possible
3. For each loop, make it clear which states variables are changed each time and what is the quit condition.
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
10. zero indexed array mid principle: size is odd, n/2 and (n-1)/2 are both mid
                                      size is even, n/2 is right mid, (n-1)/2 is left mid.
11. 




