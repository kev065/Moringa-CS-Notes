# GRAPH THEORY
## Objectives
* To learn what a graph is and how it is used.
* To implement the graph abstract data type using multiple internal representations.
* To see how graphs can be used to solve a wide variety of problems.

In this chapter we will study graphs. Graphs are a more general structure than the trees we studied in the last chapter; in fact you can think of a tree as a special kind of graph. Graphs can be used to represent many interesting things about our world, including systems of roads, airline flights from city to city, how the Internet is connected, or even the sequence of classes you must take to complete a major in computer science. We will see in this chapter that once we have a good representation for a problem, we can use some standard graph algorithms to solve what otherwise might seem to be a very difficult problem.

## Graphs
In computer science, a graph (also known as a network) is a collection of nodes (also known as vertices) and edges (also known as links or relations). We can think of nodes as things while the edges describe the relationships between those things. The study of graphs is known as graph theory and it fits into the larger field of network theory, which is useful for understanding everything from social networks to databases.

We've actually been working with graphs a lot already. That's because the tree data structures we learned in the last section are graphs with additional rules. We'll talk about this more soon.

When we can loop to a node without backtracking, the loop is known as a circuit.

A graph can also be weighted or unweighted. If a graph is weighted, its edges have numerical values associated with them. If it's unweighted, its edges do not have numerical values.

As we've already mentioned, trees are a special kind of graph. For instance, let's take another look at a binary search tree.

![binary search tree](https://drive.google.com/uc?export=view&id=12HVtafm5xNzcFQQvyp4BJ6CH1OZCMFQ8) 

We've made an update to this image of a binary search tree - now we depict it with directed edges. That's because in a tree data structure, parent nodes have knowledge of their children but child nodes don't know about their parents. To traverse a binary search tree (or other tree data structures), we need to start at the root node and work our way down. We can't start at a child node and work our way up because the edges of tree structures aren't undirected.

At this point, it should be clear that a binary search tree (and other trees) are just graphs with special rules. In addition to having directed edges, they have other special rules as well. For instance, a node in a tree structure can only have one parent but many children. And binary search trees are even more specific - a node can have at most two child nodes. This is in contrast to graphs in general, where nodes don't have these specific rules.

We also take advantage of graph theory every time we use a mapping application like Google Maps, for example, when looking up directions to see which is the fastest route.

In order to find the fastest route, Google Maps will calculate the shortest distance between the first node (your position) and the last node (destination). As you might guess, the edges need to be weighted so that the actual distance between each node can be determined. It's common to use algorithms to determine the shortest path between two nodes. It's also common to use algorithms to determine the reachability of a node. 

Graph theory can get much more complex than the basics we cover here. Check out this [short video] [! [Watch the video] (https://img.youtube.com/vi/82zlRaRUsaY/hqdefault.jpg)] (https://www.youtube.com/embed/82zlRaRUsaY/)
 on graph theory which provides a nice overview. However, we now know enough to move onto the next step: representing graphs with code.