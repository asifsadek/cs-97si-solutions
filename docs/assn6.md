---
layout: math
title: "Assignment 6: Basic Graph Algorithms"
---

# Assignment 6: Basic Graph Algorithms 

## 1308 Is It A Tree? (1) 

Property 3 cannot be easily verified directly. Note that if the first two properties are satisfied, the root cannot have more than one path to another node. If there are two paths, they must meet somewhere, and at the meeting node there are two edges pointing at it, violating property 2. Therefore, we only need to rule out the case of having zero path, or in other words, unreachable.

Count the in-degree (how many edges are pointing to it) of each node and check the first two properties. If they are satisfied, find the root, then do a DFS or BFS from the root. If all nodes can be reached, it is a tree.

## 1258 Agri-Net (2) 

Trivial minimal spanning tree.

## 2488 A Knight's Journey (3) 

Naive DFS is fast enough for the given input range. Remember, always search the node with smallest lexicographical order first.

## 1164 The Castle (3) 

Trivial flood fill. Use bit operation to find whether a wall exists.

## 2395 Out of Hay (4) 

Recall that Kruskal's algorithm tries to add edges in the order of their weights. If the last edge added to the spanning tree has weight $w$, the answer to this problem is also $w$, because if we remove all edges with weight at least $w$, the Kruskal's algorithm will stop before completing a single connected spanning tree, which means the graph is not connected.

## 3177 Redundant Paths (4) 

To make the graph biconnected, we first find and shrink all biconnected components into single nodes, leaving a tree. Then, we keep adding edges to this tree and shrinking the newly formed cycle, until the tree only contains one node, which means the graph becomes biconnected.

To find how many edges we have to add, consider the number of leaves in the tree. Each new edge can at most eliminate two leaves, by connecting two leaves without creating a new leaf. This is always possible for a tree with at least 4 leaves. A new edges connecting $a$ and $b$ creates a new leaf if and only if the path between $a$ and $b$ has at most one branch attached to it. If there is no branch, the tree has only two leaves. If there is exactly one branch, at least two leaves $c$ and $d$ is in the branch, and path between $a$ and $c$ has at least two branches, one containing $b$ and another containing $d$. It can be easily verified that a 3-leaf tree needs two edges and a 2-leaf tree needs one, so a tree with $n$ leaves needs $\lfloor \frac{n+1}{2} \rfloor$ edges.

## 1945 Power Hungry Cows (5) 

## 3275 Ranking the Cows (6)

In fact, we have to compare **all undetermined pairs**. This can be derived from the following two properties (they are both quite easy to prove, so the proof is omitted):

1. If two nodes are adjacant in the final total order, it's impossible to determine their order unless they are directly compared.
2. If two nodes don't have determined order, there exists a total order in which they are adjacant.

We can find all determined pairs by flood filling from every node in $O(nm)$, and know how many undetermined pairs there are.

## 1985 Cow Marathon (7) 

For each subtree whose root is $u$, we use tree DP to find the length of the longest path from $u$ to any node in the subtree, which is denoted $L(u)$. Let $C(u)$ be children of $u$, the DP formula is $L(u)=\max_{v\in C(u)}\lbrace w(u,v)+L(v) \rbrace$. The length of the longst path in the subtree which passes $u$ can be expressed as:

$$
P(u)=
\begin{cases}
L(u) &|C(u)|\leq 1\\
\max_{v_1, v_2\in C(u), v_1 \neq v_2} w(u,v_1)+L(v_1)+w(u,v_2)+L(v_2) &|C(u)|\geq 2\\
\end{cases}
$$

The largest $P(u)$ among all nodes is the length of the longest paths in the whole tree.

## 2337 Catenyms (7)

(Very similar to [Leetcode 332: Reconstruct Itinerary](https://leetcode.com/problems/reconstruct-itinerary/description/))

Construct a graph whose nodes are 26 letters, and each word is an edge pointing from its first letter to its last letter. We are asked to find the lexicographically smallest Eulerian trail on this graph.

Recall that for a directed graph to be Eulerian, besides degree criteria, the graph must also be *weakly connected* (that is, the graph is connected if we make all edges undirected). The answer can be obtained by travelling on the graph and greedily taking the lexicographically smallest edge that is not a bridge (otherwise removing it makes the graph disconnected). Because we have to re-run the bridge finding algorithm for every step, the complexity is $O(n^2)$.

**Update**: Found an simple $O(n)$ algorithm (ignoring the time for sorting) in [this Leetcode discussion post](https://leetcode.com/problems/reconstruct-itinerary/discuss/78768/Short-Ruby-Python-Java-C++). Code has been rewritten using this algorithm.

## 2186 Popular Cows (8, challenge problem) 

Find and shrink strongly connected components (SCCs). If the remaining graph is connected and has exactly one node that has no outward edge, every node in the correspoinding SCC in the original graph is reachable from all nodes. In all other cases, the answer is zero.

## 1944 Fiber Communications (8, challenge problem)

First, consider the linear version (where node $1$ and node $n$ are not connected, in contrast to circular) of this problem. For each connected component, its nodes are connected only if its leftmost and rightmost nodes are connected, which requires all nodes between them to be connected. Therefore, the solution is the union of the *range* (the interval between the leftmost node and the rightmost node) of all connected components. This can be solved in $O(n)$ if intervals are sorted.

For the original circular problem, notice that we only need at most $n-1$ connections, so we can always cut the circle open somewhere and transform the problem into the linear version. If we try all $n$ places to cut the circle, the smallest answer among these $n$ linear problems will also be the answer of the original problem. In each iteration, we move the cutting place by only one step, so each iteration affects at most one interval, and it's easy to maintain the sortedness of intervals without sorting them again. The overall complexity is $O(n^2)$.