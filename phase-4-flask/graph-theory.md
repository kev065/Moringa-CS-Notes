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

Graph theory can get much more complex than the basics we cover here. Check out this [short video](https://www.youtube.com/watch?v=82zlRaRUsaY/) on graph theory which provides a nice overview. However, we now know enough to move onto the next step: representing graphs with code.

## Representing Graph Structures With Code
In the last lesson, we learned about the basics of graph theory and looked at a few visual representations of graphs. While visualizing graphs is very useful, as we've learned from working with tree structures, we can't simply apply visualization to code. For instance, a search tree isn't actually a tree in code - even though we can depict it that way on a piece of paper. Instead, a search tree is simply a series of nodes where the parent nodes have references to their children.

![search tree](https://drive.google.com/uc?export=view&id=1O4oi-hwCj7TgGObN1HxPKFBoE-r6EXa5)

To depict the graph above, we need to know about two things: the nodes and the edges. We can start by storing all the nodes in a data structure such as an array.
```
const nodes = ["Lub", "Nu", "Zygob", "Wobox", "Cyra"];
```
So how do we represent the edges? There are several ways to do this.
  * ## Edge Lists
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

  * ## Adjacency Lists
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
