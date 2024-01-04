# GRAPH THEORY
## Objectives
* To learn what a **graph** is and how it is used.
* To implement the graph abstract data type using multiple internal representations.
* To see how graphs can be used to solve a wide variety of problems.

In this chapter we will study graphs. Graphs are a more general structure than the trees we studied in the last chapter; in fact you can think of a tree as a special kind of graph. Graphs can be used to represent many interesting things about our world, including systems of roads, airline flights from city to city, how the Internet is connected, or even the sequence of classes you must take to complete a major in computer science. We will see in this chapter that once we have a good representation for a problem, we can use some standard graph algorithms to solve what otherwise might seem to be a very difficult problem.

## Graphs
In computer science, a **graph** (also known as a network) is a collection of **nodes** (also known as vertices) and **edges** (also known as links or relations). We can think of nodes as things while the edges describe the relationships between those things. The study of graphs is known as graph theory and it fits into the larger field of network theory, which is useful for understanding everything from social networks to databases.

We've actually been working with graphs a lot already. That's because the tree data structures we learned in the last section are graphs with additional rules. We'll talk about this more soon.

When we can loop to a node without backtracking, the loop is known as a **circuit**.

A graph can also be **weighted** or **unweighted**. If a graph is weighted, its edges have numerical values associated with them. If it's unweighted, its edges do not have numerical values.

As we've already mentioned, trees are a special kind of graph. For instance, let's take another look at a binary search tree.

![binary search tree](https://drive.google.com/uc?export=view&id=12HVtafm5xNzcFQQvyp4BJ6CH1OZCMFQ8) 

We've made an update to this image of a binary search tree - now we depict it with directed edges. That's because in a tree data structure, parent nodes have knowledge of their children but child nodes don't know about their parents. To traverse a binary search tree (or other tree data structures), we need to start at the root node and work our way down. We can't start at a child node and work our way up because the edges of tree structures aren't undirected.

At this point, it should be clear that a binary search tree (and other trees) are just graphs with special rules. In addition to having directed edges, they have other special rules as well. For instance, a node in a tree structure can only have one parent but many children. And binary search trees are even more specific - a node can have at most two child nodes. This is in contrast to graphs in general, where nodes don't have these specific rules.

We also take advantage of graph theory every time we use a mapping application like Google Maps, for example, when looking up directions to see which is the fastest route.

In order to find the fastest route, Google Maps will calculate the shortest distance between the first node (your position) and the last node (destination). As you might guess, the edges need to be weighted so that the actual distance between each node can be determined. It's common to use algorithms to determine the shortest path between two nodes. It's also common to use algorithms to determine the reachability of a node. 

Graph theory can get much more complex than the basics we cover here. Check out this short [video](https://www.youtube.com/watch?v=82zlRaRUsaY/) on graph theory which provides a nice overview. However, we now know enough to move onto the next step: representing graphs with code.

## Representing Graph Structures With Code
In the last lesson, we learned about the basics of graph theory and looked at a few visual representations of graphs. While visualizing graphs is very useful, as we've learned from working with tree structures, we can't simply apply visualization to code. For instance, a search tree isn't actually a tree in code - even though we can depict it that way on a piece of paper. Instead, a search tree is simply a series of nodes where the parent nodes have references to their children.

![search tree](https://drive.google.com/uc?export=view&id=1O4oi-hwCj7TgGObN1HxPKFBoE-r6EXa5)

To depict the graph above, we need to know about two things: the nodes and the edges. We can start by storing all the nodes in a data structure such as an array.
```
const nodes = ["Lub", "Nu", "Zygob", "Wobox", "Cyra"];
```
So how do we represent the edges? There are several ways to do this.
  * ### Edge Lists
    We can use an edge list to list all the edges between various nodes. Each edge can be written as a pair of values. For instance, we could depict the edge between `Lub` and `Nu` as: 
    ```
    ["Lub", "Nu"]
    ```
    To depict all of the edges in our graph, we could do the following:
    ```
    const edges = [["Lub", "Nu"], ["Zygob", "Nu"], ["Nu", "Wobox"], ["Wobox", "Cyra"]];

    ```
    Each pair represents one of the edges in our graph. The order of the pairs doesn't matter. We also only need to depict the pair once and don't also need to have a reversed copy. For instance, since we already have `["Lub", "Nu"]`, we don't need `["Nu", "Lub"]`.

    If we wanted to see if a specific edge exists in a collection where the edges aren't ordered, we'd have to do a linear search through the collection, so that means O(N) runtime.

  * ### Adjacency Lists
    Another way to represent all the edges in a graph is with an adjacency list. With an adjacency list, we'd list all of the nodes that each node is adjacent to. Let's use a JavaScript object to demonstrate.
    ```
    const adjacencyList = {
    "Zygob": ["Nu"],
    "Lub": ["Nu"],
    "Nu": ["Lub", "Zygob", "Wobox"],
    "Wobox": ["Nu", "Cyra"],
    "Cyra": ["Wobox"]
    }
    ```
    As we can see, each node is a key and all of the adjacent nodes are stored as a value. Adjacent nodes are stored multiple times - as we can see, `Lub` includes `Nu` as a value and `Nu` includes `Lub` as a value.

    We can also use an adjacency list to represent directed edges as well. For instance: 
    ```
    const adjacencyList = {
    "Wobox": [],
    "Cyra": ["Wobox"]
    }
    ```
    In this adjacency list, `Wobox` can't reach any nodes but `Cyra` can reach `Wobox`.

  * ### Adjacency Matrix
    Finally, we can use an adjacency matrix to depict all the edges in a graph. An adjacency matrix uses a series of zeroes and ones or booleans to depict whether two nodes are adjacent or not.

    Here's how this looks organized in a table.

    |   | Lub | Zygob | Cyra | Nu | Wobox |
    |:--:|:--:|:--:|:--:|:--:|:--:|
    | Lub | 0 | 0 | 0 | 1 | 0 |
    | Zygob | 0 | 0 | 0 | 1 | 0 |  
    | Cyra | 0 | 0 | 0 | 0 | 1 |
    | Nu | 1 | 1 | 0 | 0 | 1 |
    | Wobox | 0 | 0 | 1 | 1 | 0 |

    To depict this in JavaScript, we'd use nested arrays to represent each row in the matrix:
    ```
    const adjacencyMatrix = [
    [0, 0, 0, 1, 0],
    [0, 0, 0, 1, 0],
    [0, 0, 0, 0, 1],
    [1, 1, 0, 0, 1],
    [0, 0, 1, 1, 0],
    ];
    ```
    It may seem silly to represent a planet's relationship with itself in an adjacency matrix. However, there is a type of graph known as a multigraph where nodes can have multiple edges - including an edge that starts and ends at the same node. For example, a multigraph could look like this (some planets omitted for simplicity).

    ![multigraph](https://drive.google.com/uc?export=view&id=1f2KyB-Hv8gmPpMdkhu6k904HoBSqg6QI)

    For this reason, it makes sense that an adjacency matrix also checks to see if an edge connects a node to itself.

    Note that an adjacency matrix can also be used to show directed edges. For instance, the intersection of the row `Cyra` with the column `Wobox` could have a value of 1 while the intersection of the row `Wobox` with the column `Cyra` could be 0. Then the trip to `Wobox` would be one way after all.

    If we want to see whether two nodes are adjacent in an adjacency matrix, we can do so in constant time O(1). For instance, in order to check if `Lub` and `Nu` are adjacent, we simply need to check `adjacencyMatrix[0][3]`. In other words, we just need to find the intersection of a specific column and row in the matrix.

    However, there's also a downside. Adjacency matrices can take up a lot of space, especially when considering sparse graphs. A graph is sparse if it doesn't have very many edges in relation to nodes. Why does this matter in terms of an adjacency matrix? Well, look at all the zeroes in the matrix above. We have to store information about the relationship between every pair of nodes in the graph - even if they don't have an edge between them. That's a lot of empty space that doesn't exist in either an edge list or an adjacency list.

    Also, if we want to find all the edges that the row `Cyra` has, we need to do a linear search of the entire row. Many of those values could be zeroes, so in a large data set, we are searching a lot of elements just to find a few adjacent nodes - especially in a sparse graph. Meanwhile, in an adjacency list, all the values corresponding to a node would represent adjacent nodes so there are fewer values to iterate through to get all the adjacent nodes.

## Building Graphs - Part 1
Imagine we've been hired to work on an application that keeps track of friendships between people. This kind of application has many use cases - after all, this is the foundation of many social networks. Remember that a graph is just another way of depicting a network. So when we think about portraying a social network, we could also call it a social graph. So let's get to work! In our application, we should be able to do the following:

* Add people (nodes) to the social network. To keep it simple, they will only have a `name` property.
* Add friendships between people (edges between nodes). These will be unweighted and undirected.
* Remove nodes from the social network. Some people may want to revoke their accounts.
* Remove edges from the social network. Sadly, some people may want to end their friendships.
This graph will not be a `connected` graph. A connected graph is one where a potential path exists between every single node in the graph. However, in an actual social network, we can't assume that everyone knows each other. Our social network could look like this:

![connected graph](https://drive.google.com/uc?export=view&id=12czLDEhaSf7Ta7GEHHBPO5b20gicKBPj)

In the image above, Ada, Grace and Eve are friends. However, no one in this group is friends (yet) with Mary and Lina, who have their own friend group.
* An algorithm for checking the reachability between two nodes. For instance, we can reach Ada via Eve or Grace but not via Mary or Lina. In a social network trying to bring different groups together, knowing the reachability between two nodes could be very useful. For instance, in a social network working to mobilize voters, it might be useful to see if various people in a city are connected or not. If not, introducing them could allow them to work together and better mobilize. We will walk through the process of writing this algorithm.
* An algorithm checking the shortest path between two nodes. In the chart above, the shortest path between all reachable nodes is just one edge but a more complex social network would have paths of varying lengths. In this way, we can check the degrees of separation between two people in the network. This could have many uses in a real-world application as well. For instance, in a professional network like LinkedIn, it might be useful to see your closest connection at a place you hope to work - and how you are connected. Or you might want to design a friend finder that automatically recommends connecting with friends of friends - which, in the parlance of graphs, would be all adjacent nodes of a node. We won't walk through this method - instead, you will have the opportunity to use one of the algorithms we discuss in order to solve this problem on your own.

You can probably imagine all kinds of ways to build this application out further. Imagine, for instance, that we decided to weight edges based on the number of social interactions between two nodes. We could then determine their overall connectivity (and perhaps even make assumptions about how close they are as friends). And you can likely also imagine all kinds of other use cases for graphs besides social networks ranging from transportation networks to HR applications to business applications tracking international trade.

In the next lesson, we'll actually start building our application.
