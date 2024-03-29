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

This graph will not be a **connected** graph. A connected graph is one where a potential path exists between every single node in the graph. However, in an actual social network, we can't assume that everyone knows each other. Our social network could look like this:

![connected graph](https://drive.google.com/uc?export=view&id=12czLDEhaSf7Ta7GEHHBPO5b20gicKBPj)

In the image above, Ada, Grace and Eve are friends. However, no one in this group is friends (yet) with Mary and Lina, who have their own friend group.
* An algorithm for checking the reachability between two nodes. For instance, we can reach Ada via Eve or Grace but not via Mary or Lina. In a social network trying to bring different groups together, knowing the reachability between two nodes could be very useful. For instance, in a social network working to mobilize voters, it might be useful to see if various people in a city are connected or not. If not, introducing them could allow them to work together and better mobilize. We will walk through the process of writing this algorithm.
* An algorithm checking the shortest path between two nodes. In the chart above, the shortest path between all reachable nodes is just one edge but a more complex social network would have paths of varying lengths. In this way, we can check the degrees of separation between two people in the network. This could have many uses in a real-world application as well. For instance, in a professional network like LinkedIn, it might be useful to see your closest connection at a place you hope to work - and how you are connected. Or you might want to design a friend finder that automatically recommends connecting with friends of friends - which, in the parlance of graphs, would be all adjacent nodes of a node. We won't walk through this method - instead, you will have the opportunity to use one of the algorithms we discuss in order to solve this problem on your own.

You can probably imagine all kinds of ways to build this application out further. Imagine, for instance, that we decided to weight edges based on the number of social interactions between two nodes. We could then determine their overall connectivity (and perhaps even make assumptions about how close they are as friends). And you can likely also imagine all kinds of other use cases for graphs besides social networks ranging from transportation networks to HR applications to business applications tracking international trade.

In the next lesson, we'll actually start building our application.

## Building Graphs - Part 2
We are going to keep this application very simple. In fact, our nodes will just be strings. That way, we can focus entirely on building a `Graph` class without worrying about a `Node` class.

So let's think about the simplest possible behavior our `Graph` class should have. We should be able to instantiate a graph with an empty data structure where we will store information about nodes and edges. So which data structure should we use?

Remember, we have three options:

* Edge list
* Adjacency list
* Adjacency matrix

We aren't going to do an edge list because honestly, it's not that great for searching. We'd have to look through the entire collection of pairs just to see a list of someone's friends. That's not very efficient.

That leaves an adjacency list or an adjacency matrix. We are going to use an adjacency list for a number of reasons. First, we can store additional data in an adjacency list (such as information about nodes and edges). We won't do that here, but it's an advantage that an adjacency list has over an adjacency matrix.

An adjacency list is also more efficient than an adjacency matrix for checking a node's adjacent nodes - and in the context of this application, that means it's more efficient for looking at a list of someone's friends. That is a pretty important operation. Why is it more efficient? Well, finding adjacent nodes is an O(N) search for both adjacency lists and adjacency matrices. However, with an adjacency list, we just need to iterate over a list of adjacent nodes (in this case, a list of someone's friends). In the case of an adjacency matrix, we'd need to iterate over all nodes - that is, everyone in the application, and check to see if each person is a friend or not. So imagine if our application has a million users. We want to see a list of Jasmine's friends. (She has fifty friends in the application in all.) It makes much more sense to iterate through a list of those fifty friends to see them than to iterate through all one million users, checking to see if each is a friend or not.

Now that we've decided to use an adjacency list, we know that when we instantiate a `Graph`, it should come with an empty adjacency list.

We are going to use a `Map` to store this list. While we could keep things very simple and just use a JavaScript object, there are performance advantages to using a `Map` - and we can easily use a Map's built-in methods to add nodes and edges to our adjacency list. What are the performance advantages? Well, for one thing, a `Map` uses a hash table lookup algorithm under the hood. The ES6 specs guarantee at least an average of **sub-linear** complexity for `Map` implementations. But what does that mean? Well, sub-linear complexity means that the Big O needs to be better than linear time (O(N)), so that could be something like O(1) constant time or O(log n) logarithmic time.

Meanwhile, if we used an object instead, we'd need to iterate through the object - and object iteration isn't optimized already, which means we'd have a Big O of O(N). So iterating through objects is slower.

  * ### Instantiating a Graph
    Our first check will make sure that we can correctly instantiate a new graph with an empty adjacency list:

    *`__tests__/graph.test.js`*
    ```
    import Graph from '../src/graph.js';

    describe('Graph', () => {

      let graph = new Graph();

      afterEach(() => {
        graph = new Graph();
      });

      test('should correctly instantiate a graph', () => {
        expect(graph.adjacencyList.size).toEqual(0);
      });
    });
    ```
    Note that we start by instantiating a new graph and also add an `afterEach()` block to reset the graph after each test. It's overkill for just one test but it will DRY up future tests.

    In the test itself, we `expect(graph.adjacencyList.size).toEqual(0)`. Maps have a `size` property, which means we can just check to see if our graph's `adjacencyList` has a `size` property that's equal to zero (an empty map).

    Here's the code to get this passing:

    *`src/graph.js`*
    ```
    export default class Graph {
    constructor() {
        this.adjacencyList = new Map();
    }
    }
    ```

  * ### Creating Nodes
    What's the next thing we need to do? Well, we need to be able to add people to our social network. People are the equivalent of nodes in a graph. We'll keep our methods sufficiently abstract so they pertain more to creating graphs in general than building social networks more specifically. For that reason, we'll add a ```Graph.prototype.addNode()``` method, not a ```Graph.prototype.addFriend()``` method.

    Here's our new test: 

    *`__tests__/graph.test.js`*
    ```
    ...
    test('should add a new node', () => {
        graph.addNode("Jasmine");
        expect(graph.adjacencyList.get("Jasmine").size).toEqual(0);
    });
    ...
    ```
    We should be able to add a node via a ```Graph.prototype.addNode()``` method. We don't have a ```Graph.prototype.getNode()``` method yet so we can't use that for our expectations. For that reason, we'll just access the node via the graph's ```adjacencyList```. Since the ```adjacencyList``` is a ```Map```, we can use ```Map.prototype.get()``` to get the value associated with the key. Why are we looking for a ```size``` property again once we get the value related to Jasmine? Well, each key in our adjacency list will be a ```Set```.

    A ```Set``` is a collection of entirely unique values. Jasmine and Ada can be friends only once - not three times - so we want to make sure that a node can occur in the collection only once. That's where using a JavaScript ```Set``` comes in handy.

    Once again, the ES6 specifications guarantee that ```Set``` implementations generally have sub-linear complexity - so Sets are also pretty efficient and much better than using a collection like an array.

    Here's the code we need to pass our test:

    *`src/graph.js`*
    ```
    export default class Graph {
    ...
    addNode(name) {
        this.adjacencyList.set(name, new Set());
    }
    }
    ```

    As we can see, the implementation is simple. We use ```Map.prototype.set()``` to add a key-value pair to our ```adjacencyList```. The key will be the name of the node we are adding and the value will be an empty Set.

  * ### Checking to See if Nodes Exist
    What's the next behavior we should implement? Well, we need to know whether a node exists in our graph or not. That means the next step is to add a Graph.prototype.hasNode() method. The simplest behavior for this method will be to return false if the node doesn't exist in the adjacency list.

    Here's a test:

    *`__tests__/graph.test.js`*

    ```
    ...
    test('should return false if the node does not exist in the adjacency list', () => {
        expect(graph.hasNode("Ada")).toEqual(false);
    });
    ...
    ```
    And here's the minimum code in a `Graph.prototype.getNode()` method to get this test passing:

    *`src/graph.js`*
    ```
    ...
    hasNode(name) {
        return false;
    }
    ...
    ```
    Now we need to write a test for when a node does exist in a graph:

    *`__tests__/graph.test.js`*

    ```
    ...
    test('check to see if node exists in graph', () => {
        graph.addNode("Jasmine");
        expect(graph.hasNode("Jasmine")).toEqual(true);
    });
    ...
    ```

    To get this test passing, we can use a method that a Map's prototype offers. Here's the update to our method:

    *`src/graph.js`*
    ```
    ...
    hasNode(name) {
        if (this.adjacencyList.get(name)) {
        return true;
        }
        return false;
    }
    ...
    ```

    We take advantage of `Map.prototype.get()`, which either returns a value of a key in a map or returns undefined if the key doesn't exist. Here's a situation where our code is already more efficient because we are using maps instead of just iterating through a basic object. To check if our graph has a node, we need to do a search. Because we are using `Map.prototype.get()` instead of looping through an array or object, calling this method has sub-linear complexity instead of linear complexity (O(N)).

    Now that we've added all the basic functionality for nodes other than removing nodes, which we'll get to later, we're ready to start adding functionality for edges.

  * ### Adding Edges
    At this point, we have methods to add nodes to a graph and to see whether a node exists in our graph. The next step is to add functionality to add relationships between nodes.

    In other words, we need to be able to add edges, which means we need a `Graph.prototype.createEdge()` method.

    As always, we'll start with a test:

    *`__tests__/graph.test.js`*
    ```
    ...
    test('add an edge between two nodes', () => {
        graph.addNode("Jasmine");
        graph.addNode("Ada");
        graph.createEdge("Jasmine", "Ada");
        expect(graph.adjacencyList.get("Jasmine").has("Ada")).toEqual(true);
        expect(graph.adjacencyList.get("Ada").has("Jasmine")).toEqual(true);
    });
    ...
    ```

    First, we need to add two nodes to our test. Then we need to create an edge between these two nodes with a `Graph.prototype.createEdge()` method. Note that this test has two expectations. Remember that this is an undirected graph - a friendship goes two ways, not one. That means we need to add each node to the other's adjacency node list. If this were a directed graph, we'd just update one adjacency list - and the order of arguments in our `Graph.prototype.createEdge()` method would matter.

    Note also that we have to do some chaining in our expectations to reach the values we want. We start by using `Map.prototype.get()` to get the value associated with a node, which happens to be a set. Then we use `Set.prototype.has()` to determine whether the set actually has the node we are looking for.

    If the worst-case scenario in terms of Big O is O(N) for both of these operations, that means with chaining we have O(2N) - so still O(N). However, the worst-case will be rare and the average-case will be sub-linear complexity.

    Here's the method to get this test to pass:

    *`src/graph.js`*
    ```
    ...
    createEdge(node1, node2) {
        let set1 = this.adjacencyList.get(node1);
        let set2 = this.adjacencyList.get(node2);
        set1.add(node2);
        set2.add(node1);
    }
    ...
    ```

    To create an edge, we need information about the nodes we want to create an edge between. That means our method takes two arguments. We need to grab the set associated with each node. Then we need to add each node to the other node's list of adjacent nodes. Remember that because these are sets, we don't need to worry about duplicates - JavaScript will take care of that for us.

    We can also make our method more concise with additional chaining:

    *`src/graph.js`*
    ```
    createEdge(node1, node2) {
    this.adjacencyList.get(node1).add(node2);
    this.adjacencyList.get(node2).add(node1);
    }
    ```

  * ### Checking to See if a Node Exists
    We could just do something like the following whenever we want to see if an edge (in this case, a friendship) exists:

    ```
    graph.adjacencyList.get("Ada").has("Jasmine")
    ```

    This would check to see if Ada and Jasmine are friends.

    However, this is pretty important functionality for our application to have. That means we should add it.

    Let's start with the easiest behavior - just returning `false`.

    *`__tests__/graph.test.js`*

    ```
    ...
    test('check to see if edge exists in graph', () => {
        graph.addNode("Jasmine");
        graph.addNode("Ada");
        expect(graph.hasEdge("Jasmine", "Ada")).toEqual(false);
    });
    ...
    ```

    Here's the implementation:

    *`src/graph.js`*

    ```
    ...
    hasEdge(node1, node2) {
        return false;
    }
    ...
    ```

    Now that we have that test passing, we're ready to move on. Typically, we won't update tests we've already written but we are going to make an exception here.

    We will update the following test to use our new method:

    *`__tests__/graph.test.js`*

    ```
    ...
    test('add an edge between two nodes', () => {
        graph.addNode("Jasmine");
        graph.addNode("Ada");
        graph.createEdge("Jasmine", "Ada");
        expect(graph.adjacencyList.get("Jasmine").has("Ada")).toEqual(true);
        expect(graph.adjacencyList.get("Ada").has("Jasmine")).toEqual(true);
    });
    ...
    ```

    Instead of expecting `graph.adjacencyList.get("Ada").has("Jasmine")` to equal true, why not just apply our new method instead?

    Our updated test looks like this:

    *`__tests__/graph.test.js`*

    ```
    ...
    test('add an edge between two nodes', () => {
        graph.addNode("Jasmine");
        graph.addNode("Ada");
        graph.createEdge("Jasmine", "Ada");
        expect(graph.hasEdge("Ada", "Jasmine")).toEqual(true);
    });
    ...
    ```

    As we can see here, we now expect `graph.hasEdge("Ada", "Jasmine")).toEqual(true);`.

    This test will fail - let's update our `Graph.prototype.hasEdge()` method now:

    *`src/graph.js`*

    ```
    ...
    hasEdge(node1, node2) {
        if (this.adjacencyList.get(node1).has(node2)) {
        return true
        }
        return false;
    }
    ...
    ```

    Because this is an undirected graph, it doesn't matter which way we check the relationship - each node is adjacent to the other node. So we just check that the set associated with the `node1` key includes `node2`. If it does, our method will return true.

    That's all we need to get all our tests passing.

  * ### Removing Edges
    We should also be able to remove an edge as well - no more friendship, sadly…

    Here's the test:

    *`__tests__/graph.test.js`*

    ```
    ...
    test('remove an edge between two nodes', () => {
        graph.addNode("Jasmine");
        graph.addNode("Ada");
        graph.createEdge("Jasmine", "Ada");
        graph.removeEdge("Jasmine", "Ada");
        expect(graph.hasEdge("Ada", "Jasmine")).toEqual(false);
    });
    ```

    In this test, we first create two nodes and add an edge. Next, we'll call `graph.removeEdge("Jasmine", "Ada");`. Based on removing that edge, our expectation should show that there's no longer an edge between the Ada and Jasmine nodes.

    Now let's get this test passing:

    *`src/graph.js`*

    ```
    ...
    removeEdge(node1, node2) {
        this.adjacencyList.get(node1).delete(node2);
        this.adjacencyList.get(node2).delete(node1);
    }
    ...
    ```

    Because this is an undirected graph, both nodes include the other node in their set of adjacent nodes. That means we need to remove `node1` from `node2`'s adjacent nodes and vice versa. Here's another situation where having sub-linear complexity benefits us. If we were doing our own implementation with objects and arrays, we'd need to do an O(N) search to get a node and then another O(N) search to find the node to delete. Removing the node from the array would then shift all the remaining indexes of the array - yet another O(N) operation. Then we'd do that all over again going in the other direction. While O(6N) still boils down to O(N), it should be clear that having more efficient code can really help us in the long run, especially if our datasets get especially large.

  * ### Removing Nodes
    Finally, let's add some functionality for removing nodes. We saved this for last since it's a bit more complex than the functionality we've built so far. That's because it's not as simple as just deleting a node. We also need to delete all references to that node in the rest of the adjacency list. In the context of a social network application, when we remove a person from the app, everyone else that's friends with that person will no longer have them listed as a friend because it's only possible to be friends with someone who is also on the app.

    Let’s first write a test:

    *`_tests_/graph.test.js`*

    ```
    ...
    test('should delete a node and all of its adjacent nodes', () => {
        graph.addNode("Ada");
        graph.addNode("Jasmine");
        graph.addNode("Lydia");
        graph.createEdge("Ada", "Jasmine");
        graph.createEdge("Ada", "Lydia");
        graph.removeNode("Ada");
        expect(graph.hasNode("Ada")).toEqual(false);
        expect(graph.hasEdge("Jasmine", "Ada")).toEqual(false);
        expect(graph.hasEdge("Lydia", "Ada")).toEqual(false);
    });
    ...
    ```

    This is a pretty wordy test (and at some point, it would be good to look at ways to DRY our tests further). First, we need to add three nodes and then create edges between our first node and the others. Why are we doing this? Well, we want to make sure that we are correctly removing all edges that refer to the deleted node. We also have three expectations - which is on the heavy side. Ideally, we'd break this test up into a few smaller behaviors (just deleting a node first before testing to see if edges were successfully removed), but we're doing it all in one test here.

    We expect the node we've deleted to be gone. However, we still have two other nodes that have references to the deleted node - so we also expect those references to be gone as well.

    Let's implement the method now:

    *`src/graph.js`*

    ```
    ...
    removeNode(name) {
        if (this.adjacencyList.has(name)) {
        this.adjacencyList.get(name).forEach((edge) => {
            this.adjacencyList.get(edge).delete(name);
        });
        this.adjacencyList.delete(name);
        }
    }
    ...
    ```

    First, we check if the node we want to delete exists as a key in the adjacency list (`(this.adjacencyList.has(name))`). Based on our test, the node represents Ada which exists in our graph.

    Next, we call `this.adjacencyList.get(name)`. This is the Set of edges associated with the node we want to delete. In the case of Ada, the edges (or friendships) are Jasmine and Lydia.

    For each edge, we need to do the following: `this.adjacencyList.get(edge).delete(name);`. So for our first edge (Jasmine), we grab the key associated with Jasmine and then remove Ada from her friendships. Next, we do the same with the second edge (Lydia). We grab the key associated with Lydia and then remove Ada from her friendships, too.

    At this point, we've removed all of the edges related to Ada in other nodes. The last step is to delete the node itself - in this case, Ada, removing her entirely from the application.

    A few other things to note here. We use arrow notation (`=>`) so that `this.adjacencyList` remains bound to the same `this` both inside and outside the loop. If it weren't for `=>`, the `this` inside the loop would lose its binding and be `undefined`.

    So what is the runtime complexity of this algorithm? It's hard to pinpoint exactly without fully knowing everything JavaScript is doing to optimize sets and maps under the hood. However, we can make some educated guesses.

    * `this.adjacencyList.has(name)` has sublinear complexity. We know that maps use hash tables under the hood. This may be close to O(1) lookup.
    * Looping through the set of adjacent nodes is O(N), which is typical with basic loops.
    * Inside the loop, we need to get an edge and remove it - which depending on how ES6 implements deleting items from a set, is at least sublinear complexity and possibly even close to O(1).

    On the other hand, imagine that we implemented our own basic functionality where we needed to first iterate through the set of adjacent nodes and then had to iterate again through each node's adjacent nodes to find which one needed to be deleted. That would be O(AB), which isn't so great. (It would not be O(n²) because the nested loop iterates over a different set in the outer loop.)

    At this point, we have everything in place to add and remove nodes and edges from our graph application. We can also check to see if a node or edge exists in the adjacency list and we can return all of a node's edges.

    The next step is to learn about traversing our graph - and that means applying BFS and DFS - breadth-first search and depth-first search algorithms. We will start learning about these algorithms in the next lesson.

## Stacks and Queues
In this lesson, we're going to learn about **stacks** and **queues**. We'll start by covering a brief overview of each. Then we'll write basic stack and queue functions.

Stacks and queues are both data structures that hold a list of elements. However, there is a key difference in how they work. A queue is **first in, first out** or **FIFO**. On the other hand, a stack is **last in, first out** or **LIFO**.

Let's use some examples of how both the FIFO and LIFO principles apply in our daily lives.

* When we get in a line (at the grocery store checkout counter, to go to a movie, or anything else), we expect the first person in line to get served first and so on. This is a prime example of FIFO.
* On the other hand, let's say we are reorganizing and stacking books from a bookshelf one at a time. When we take a book off the stack, we will most likely take it off the top of the stack, not the bottom. This is an example of LIFO because the last book added to the stack is the first one that's taken off the stack.

There are plenty of examples in computer programming where we'll use queues and stacks as well. For instance, waiting to download something and others are queued to download that thing first.

When it comes to stacks, we work with the JavaScript call stack every time we write JavaScript code. We can see this clearly in the following example:

```
function first() {
  return second();
}

function second() {
  return third();
}

function third() {
  return "hello!"
}
```

When we call `first()`, what happens? `first()` calls `second()` which calls `third()`. But which one is actually resolved first? Well, `first()` can't be resolved until `second()` is - and `second()` can't be resolved until `third()` is. How can the `first()` function ultimately return `"hello!"` unless the `third()` function resolves first?

This is the stack:

```
TOP OF THE STACK

third()
second()
first()

BOTTOM OF THE STACK
```

The technical term for each function is a stack is a **frame**. Whenever a function is called in the JavaScript runtime (the time our code is actually executed), the runtime creates a stack frame for that function. There is a limit to that stack - which you've probably noticed if you've ever run an infinite loop by accident and received a `Range error: maximum call stack size exceeded` error.

By the way, if you are confused about what the runtime is, in the Chrome browser or in Node (JavaScript backend environment) the runtime is the V8 engine.

And when it comes to queues, we are actually working with a queue every time we run async JavaScript code in the browser. The browser actually uses separate web APIs to run async code and when that async code is ready to run (such as when a response from an API is received), that code is actually put in a `callback` queue which is not the call stack. So our synchronous code is put on the call stack (LIFO) while our asynchronous code is queued up in the callback queue (FIFO). Meanwhile, an event loop determines whether to run code from the call stack or the callback queue. You don't need to have a deep understanding of this to work with JavaScript. 

  * ### Writing our Own Stack and Queue Functions
    #### Exercise
    Next, let's do a little exercise. See if you can write your own `stack()` and `queue()` classes which add and remove elements as needed. Here are some hints - and don't overthink it. It's actually surprisingly simple to do.
    * Remember that both stacks and queues are data structures.
    * What common data structure will allow us to easily add and remove elements?
    * Stacks are last in, first out, which means we need to add things to the end of the data structure and remove things from the end of the data structure.
    * Queues are first in, first out, which means we need to add things to the end of the data structure and then remove things from the beginning of the data structure.

    Take a few minutes to write out these classes and then check out the solution below.

    #### Solution 
    The hints hopefully made it fairly clear that we can use arrays to easily model both queues and stacks. `array.prototype.push()` can be used to add elements to the end of either a stack or a queue. Meanwhile, we can use `array.prototype.pop()` to remove elements from the end of a stack and we can use `array.prototype.shift()` to remove elements from the beginning of a queue.

    Here's our simple implementation of a stack:

        ```
        class Stack {
        constructor() {
            this.elements = [];
        }

        push(element) {
            return this.elements.push(element);
        }

        pop() {
            return this.elements.pop();
        }
        }
        ```
    
    Yep, that's it. That is really all a stack is doing - pushing and popping. Note that we can also use `array.prototype.shift()` and `array.prototype.unshift()` as well. This feels a bit closer to how a stack actually works since it adds an element to the beginning of the collection and then removes it from the beginning of the collection. The one downside to this approach is that it's a bit slower - that's because JavaScript needs to re-index the array each time an element is added or removed from the beginning of the stack.

    Ultimately, either approach is fine - the most important thing is that whichever element was added to the stack most recently must also be the one that gets removed most recently - that's last-in, first-out (LIFO).

    Here's our implementation of a queue - which will look very similar:

        ```
        class Queue {
        constructor() {
            this.elements = [];
        }

        enqueue(element) {
            return this.elements.push(element);
        }

        dequeue() {
            return this.elements.shift();
        }
        }
        ```

    There are a few differences here. First, we call our methods `queue.prototype.enqueue()` and `queue.prototype.dequeue()`. That's because when we add an item to a queue, it's known as **enqueuing** while removing an item from a queue is known as **dequeuing**.

    Actual queues and stacks generally have other methods as well - and they are more complex than these very simple implementations. However, on a basic conceptual level, the code examples above encapsulate what we need to know about stacks and queues.

    We'll get more practice with stacks and queues when we write certain algorithms. For instance, it's common to use a queue with a breadth-first search algorithm and a stack with a depth-first search algorithm.


## Depth and Breadth Search Algorithms
There are two ways we can search a tree - regardless of whether that's a binary search tree or a more general tree. We can take a **depth-first search (DFS)** approach or a **breadth-first search (BFS)** approach. If we are looking at a tree that has a root node and child nodes, a depth-first search algorithm will search a tree vertically while a breadth-first search algorithm will search a tree horizontally. A graph can't really be measured vertically or horizontally in this way but the same concept still applies - we can search broadly or go deeper into each branch first. We just don't necessarily do so from a root node. Instead, we can start our search from any node in the graph.

  * ### Depth-First Search
    In a depth-first search, our algorithm will completely traverse the left-most branch. Once that is complete, it will backtrack and then completely traverse the next branch. It will continue doing this until it finds what it's searching for - or, if the value doesn't exist, until the tree is completely traversed.

    But how does the algorithm actually backtrack? Well, a DFS algorithm commonly uses a **stack** (last-in, first-out) to search through each branch to the very end. 

    Let's walk through this process for our perfect binary search tree - and in doing so, we'll see how using a stack allows our algorithm to traverse and automatically backtrack through a tree's branches.

    In the perfect binary search tree above, we start with the root node.

    Here's our stack so far:

    ```
    let stack = [4]
    ```
    There's only one element, so we can't really demonstrate its LIFO yet. We get the children of 4 (2 and 6) and remove the 4 from the stack. Now our stack looks like this:

    ```
    stack = [2, 6];
    ```
    Remember, we want to search the left-most branch first. So now we grab 2 from the beginning of the stack and add this node's children to the beginning of the stack: 1 and 3. Here's the stack now:

    ```
    stack = [1, 3, 6];
    ```
    Once again, to continue down the left-most branch, we just need to grab the first value. We take the 1 from the beginning of the stack. It has no children, so we are done with that branch. So here's our stack now:

    ```
    stack = [3, 6];
    ```
    Look at that - the first element in our stack is the next branch! We see that the 3 has no children of its own - so now our stack is just this:

    ```
    stack = [6];
    ```

    We've completely evaluated the left subtree and now only the right subtree remains. We take 6 from the stack, evaluate its children, and so on.

    Now that we've looked at a basic example of how a DFS uses a stack, let's apply a more complex example. This time we'll use a graph. After all, we are in the middle of learning about graph theory.

    ![graph of friends](https://drive.google.com/uc?export=view&id=11RJhwpk2debAIlEX173fjpqJ9k17tJX5)

    Here's a graph of friends. We will actually be using this very graph when we actually write our BFS and DFS algorithms over the next several lessons. Let's say we want to find a connection between Jasmine and Thomas. How would we do that with a depth-first search?

    First, we'll get an array of Jasmine's friends. This is our initial stack:

    ```
    let stack = ["Ada", "Lydia", "Rose"];
    ```

    Now we want to navigate down through the first branch on the list. There's really no working from left-to-right here - nor can we really say we are searching the graph horizontally or vertically like we would with a binary search tree. Instead, we'd just grab the list of Jasmine's friends from an adjacency list or similar structure. Because this is a stack, we are starting with the first friend in the array. We remove Ada from the stack and get her list of friends.

    Ada is connected to Dylan and Lydia. Let's add them to the top of the stack. Now our stack looks like this:

    ```
    stack = ["Dylan", "Lydia", "Lydia", "Rose"];
    ```

    Ada has been removed from the top of the stack while her friends have been added. Because this is a stack, we need to continue with the last-in, first-out approach. But now hopefully it's clear why we need to do that. What gets added to the top of the stack is any remaining friends in the branch we are currently traversing - as well as friends in sub-branches we will be traversing soon. Meanwhile, what's at the bottom of the stack? A completely different branch. So everything we add right now is what we need to evaluate right now - in other words, last-in, first-out.

    So now we take Dylan from the top of the stack and grab his friends. There is just Allison, which we add to the top of the stack.

    ```
    stack = ["Allison", "Lydia", "Lydia", "Rose"];
    ```
    When we take Allison from the top of the stack, we'll see that she only has Dylan as a friend. However, we have already traversed the node that represents Dylan - and we don't want to traverse it again. To avoid that, we flag nodes as we traverse them. This is a key difference between how we'd traverse this graph versus how we'd traverse a binary search tree. Our binary search tree is directed - information can only flow from parents to children - and children know nothing about their parents. There is no risk of accidentally going back to a node that has already been traversed. However, our graph is undirected - so we need to make sure our algorithm doesn't accidentally traverse nodes that have already been covered.

    So at this point, because we've traversed through nodes representing Jasmine, Ada and Dylan already, we wouldn't add them back to the stack. So when Allison is removed from the top of the stack, no one else is added.

    So now our stack looks like this:

    ```
    stack = ["Lydia", "Lydia", "Rose"];
    ```
    We are moving onto our next branch! Once we reach the end of a branch, there are no more nodes to traverse. The last item in the stack will now be the most recent sub-branch we haven't explored yet. In this case, it is Lydia. But note that we are exploring Lydia's connection to Ada, not Lydia's connection to Jasmine here.

    We remove that from the top of the stack and find Thomas. We've successfully determined the reachability between Thomas and Jasmine. So at this point, if we've been tracking our traversal, we can see that there's a connection going like this: `Jasmine -> Ada -> Lydia -> Thomas`. So we could say that Thomas is a friend of a friend of a friend.

    But if we actually look at our graph, we can quickly see that there is a closer connection. After all, we normally call them friends of friends if we can - not friends of friends of friends. If we wanted to determine not just the reachability of a node but the shortest path, our depth-first search would continue.

    By the way, here's a very important reason we need to flag nodes as we traverse them. Jasmine is Lydia's friend. If we didn't flag that we'd already traversed through that node, our algorithm could check Jasmine's friends, find Ada, and then check Ada's friends, find Lydia, and then check Lydia's friends, only to find Jasmine, forever and ever in an infinite loop of friends that sounds wonderful in practice but not in programming.

    So the key takeaways here, at least from a programming perspective, is that we need to use a stack to walk through the graph with a depth-first search, and also that we need to flag nodes we've already traversed so we don't run into infinite loops or other inefficiencies.

    If this is still confusing, don't worry. It should become clearer when we use TDD to write a depth-first search algorithm in the next lesson.

    #### Advantages
    * DFSs are great for building games because they allow the algorithm to explore winning and losing possibilities.
    * If we know that the answer we are looking for will be at the bottom of a tree, a DFS will usually be faster than a BFS. For instance, we might be looking for living descendants of a long family tree - and everyone alive will be at the bottom of the tree.
    * DFS usually takes less memory than BFS.


  * ### Breadth-First Search
    On the other hand, a breadth-first search algorithm searches horizontally. The image below demonstrates this:

    ![binary search tree](https://drive.google.com/uc?export=view&id=17GHfIcsBknYOAb54ChmLKzEzYFQL4urJ)

    As we can see, the algorithm will search each row in the tree completely before moving onto the next one.

    In contrast to a depth-first search, a breadth-first search uses a **queue** instead of a stack. That means that when we add new nodes to the collection, they are added at the end. Meanwhile, we evaluate the node at the beginning of the collection first.

    Once again, let's demonstrate how this works first on the perfect binary search tree in the diagram above and then on our graph of friends.

    Our queue starts like this:
    ```
    let queue = [4];
    ```

    We grab 4 and then add 2 and 6 to the queue.
    ```
    queue = [2, 6];
    ```

    2 is first in line, so let's add its children: 1 and 3. Here's the big difference between the stack we used before and the queue we are using now. We add 1 and 3 to the end of the queue - not to the beginning (or "top") of the stack.

    So now our queue looks like this:
    ```
    queue = [6, 1, 3];
    ```

    We take 6 from the front and add its children (5 and 7) to the queue:
    ```
    queue = [1, 3, 5, 7];
    ```

    As we can see, we've finished an entire "row" of the tree - and all that's left to traverse is the bottom row.

    In this case, we want to check all of Jasmine's friends before we check her friends' friends.

    First, we add all of her friends to an array:
    ```
    let queue = ["Ada", "Lydia", "Rose"];
    ```

    The first friend in the queue is Ada. We remove Ada from the queue and get her friends: Dylan and Lydia. We add them to the end of the queue:
    ```
    queue = ["Lydia", "Rose", "Dylan", "Lydia"];
    ```

    As we can see, the first few names still on the list are the ones that are Jasmine's friends - we save the names we added (friends of friends) to the end of the list. Remember, nodes added to the end of the list always represent going deeper into the tree. If we want to go depth-first, we add elements to the beginning of the collection. If we want to go breadth-first, we add new elements to the end of the collection.

    #### Advantages
    * If we know that an answer will be close to the root node, a BFS is more efficient. For instance, in the family tree example, we might be looking for a direct descendant that lived 50 years later than the ancestor at the root node, not 300 years later.
    * BFSs are great for computing the most efficient path between two nodes. For that reason, they are great for GPS and mapping applications and even for finding people near each other in social networks.


  * ### Depth First Algorithms
    In the previous lesson, we covered depth-first search and breadth-first search approaches. There are a lot of benefits of using these algorithms. We could determine the **reachability** between two nodes or we could determine the shortest path between two nodes, just to name a few use cases.

    We'll start with the depth-first algorithm because it is a bit easier to implement. To actually use a TDD approach and test our algorithms in a graph, we are going to need to code that graph first. We will add this graph to our test file.

    To keep things separated out, we are going to create a new test file called `dfs.test.js` and run all tests related to our depth-first search there. Here is the starter code which includes a graph.

    *`__tests__/dfs.test.js`*

    ```
    import Graph from '../src/graph.js';

    describe('depth-first search', () => {

      let graph = new Graph();
      graph.addNode("Jasmine");
      graph.addNode("Ada");
      graph.addNode("Lydia");
      graph.addNode("Rose");
      graph.addNode("Dylan");
      graph.addNode("Allison");
      graph.addNode("Thomas");
      graph.addNode("Sarah");
      graph.createEdge("Jasmine", "Ada");
      graph.createEdge("Jasmine", "Lydia");
      graph.createEdge("Jasmine", "Rose");
      graph.createEdge("Ada", "Dylan");
      graph.createEdge("Lydia", "Ada");
      graph.createEdge("Dylan", "Allison");
      graph.createEdge("Lydia", "Thomas");
    });
    ```

    Note that we don't put the graph in a `beforeEach()` or `afterEach()` block. We aren't actually going to modify this graph, just traverse it, so there's no need to re-instantiate it before or after every single test.

    Next, let's include a visual representation of the coded graph above. It's the same one we were using in the last lesson.

    ![graph illustration](https://drive.google.com/uc?export=view&id=13tXeHzkEz6UNr2bJ9jDl3mNe3H1--Hda)

    There is one small but key difference about our graph that is not included in our illustration. We've also added Sarah, a node that isn't connected to anyone in the graph. We'll need this for one of our tests - which we'll cover in a moment.

    Our DFS will just check to see if a node is reachable from another node. In this case, we just want to see if we can reach Thomas via Jasmine. While it's obvious from our graph that all its nodes are reachable from each other (except for Sarah), in a real-world application, that might not be the case.

    Let's write our first test. We'll call our method `graph.prototype.depthFirstReachable()` and it will return a boolean. The reason for the wordy name is because we'll write a method called `graph.prototype.breadthFirstReachable()` in the next lesson that does the exact same thing as this method - but with a different algorithm.

    The simplest implementation of our `graph.prototype.depthFirstReachable()` method is to return false if a potential node doesn't even exist in the graph. For the sake of brevity, we'll code two tests right now: one to check the starting node, one to check the target node. Here are the tests:

    *`__tests__/dfs.test.js`*

      ```
      ...
      test('should return false if the target node does not exist', () => {
          expect(graph.depthFirstReachable("Jasmine", "Albert")).toEqual(false);
        });

        test('should return false if the starting node does not exist', () => {
          expect(graph.depthFirstReachable("Albert", "Thomas")).toEqual(false);
        });
      ...
      ```

    Now let's get these tests passing:

    *`src/graph.js`*

      ```
      ...
        depthFirstReachable(startingNode, targetNode) {
          if ((!this.adjacencyList.has(startingNode)) || (!this.adjacencyList.has(targetNode))) {
            return false;
          }
        }
      ...
      ```
    
    Our method will take two arguments: the first, a `startingNode`, is the node we want to start from. The second is the `targetNode`, which is the node we want to find. In an undirected graph, these arguments could be interchangeable. In a directed graph, on the other hand, the order would matter.

    Our method just checks to see if both nodes exist in the adjacency list. If one or the other doesn't exist, they obviously aren't reachable. Why go to the trouble of doing a DFS, which could be computationally intensive with a big graph, when values don't even exist? Because our adjacency list is a Map, it is super-fast to do this lookup. Meanwhile, doing a DFS to search the whole graph - only to tell us the same thing - would be very inefficient.

    Next, let's get to the fun stuff. We will check to see if a direct friend of Jasmine is reachable.

    Here's the test:

    *`__tests__/dfs.test.js`*

      ```
      ...
        test('should check if the first friend in the adjacency list is reachable', () => {
          expect(graph.depthFirstReachable("Jasmine", "Ada")).toEqual(true);
        });
      ...
      ```
    
    Now the code to get the test passing:

    *`src/graph.js`*
      ```
      depthFirstReachable(startingNode, targetNode) {
        if ((!this.adjacencyList.has(startingNode)) || (!this.adjacencyList.has(targetNode))) {
          return false;
        }
        let stack = [startingNode];
        let traversedNodes = new Set();
        while (stack.length) {
          const currentNode = stack.shift();
          if (currentNode === targetNode) {
            return true;
          } else {
            traversedNodes.add(currentNode);
            const adjacencyList = this.adjacencyList.get(currentNode);
            adjacencyList.forEach(function(node) {
              if (!traversedNodes.has(node)) {
                stack.unshift(node);
              }
            });
          }
        }
      }
      ```

    Let's focus on just the new code - which is our actual DFS algorithm:

    *`src/graph.js`*

      ```
      ...
      let stack = [startingNode];
      let traversedNodes = new Set();
      while (stack.length) {
        const currentNode = stack.shift();
        if (currentNode === targetNode) {
          return true;
        } else {
          traversedNodes.add(currentNode);
          const adjacencyList = this.adjacencyList.get(currentNode);
          adjacencyList.forEach(function(node) {
            if (!traversedNodes.has(node)) {
              stack.unshift(node);
            }
          });
        }
      }
      ...
      ```

    First, we need to create our stack:

      ```
      let stack = [startingNode];
      ```

    It's an array with the `startingNode` as its only element.

    Next, we also need some way to flag the nodes that have been traversed. We don't want this to be a property on the node itself because this flag is temporary - we don't want it to persist once the method is finished running.

      ```
      let traversedNodes = new Set();
      ```
    
    This is a perfect use case for a Set. We don't want there to be duplicate values - we just want to know whether a node has been traversed or not. And looking up whether a value in a Set is super-fast - so it won't take too much time. Remember, hash table lookup is O(1) constant time - and a Set uses a hash table under the hood.

    At this point, we have collections to store a stack as well as a set of traversed nodes. We are ready to start looping!

    In general, when looping through a stack or a queue that will eventually be empty, we can just iterate until the collection's length is zero. At that point, we know we've checked every value in the stack or queue.

    That's exactly what we do here:

      ```
      while (stack.length)
      ```
    
    This just states that we should loop through the stack until it is empty. Once the length of the stack becomes zero, stack.length is false - and the loop will end.

    Next, we need to take the first element from the stack:

      ```
      const currentNode = stack.shift();
      ```
    
    `array.prototype.shift()` is a destructive method - that means that we aren't just grabbing the value of the first element in our stack - we are also removing it from the array. This is the `currentNode` that we are examining. It is a `const` because the variable itself never changes - it falls out of scope each time through the loop and is created anew.

    Next, we have a conditional that checks to see if the `currentNode === targetNode`.

      ```
      if (currentNode === targetNode) {
        return true;
      }
      ```
    
    If they are equal, we've found the target node! The target node is reachable from the starting node - and we can return `true`.

    If it isn't, we have more work to do. Here's the code in our `else` statement:

      ```
      ...
        else {
          traversedNodes.add(currentNode);
          const adjacencyList = this.adjacencyList.get(currentNode);
          adjacencyList.forEach(function(node) {
            if (!traversedNodes.has(node)) {
              stack.unshift(node);
            }
          });
        }
      ...
      ```

    First, we need to "flag" our node to show that it's been traversed:

      ```
      traversedNodes.add(currentNode);
      ```

    Here, we use the `set.prototype.add()` method to add the `currentNode` to `traversedNodes`. This way, when we need to look up whether the node has been traversed or not, we'll see that the `currentNode` has been added. The first time through the loop, this will be the `startingNode` - also known as Jasmine.

    Next, we need to find the adjacency list for the current node. In Jasmine's case, the list is a Set that includes Ada, Lydia, and Rose.

    Once we've gotten the adjacency list, we are ready to add it to the stack. To do that, we iterate through the adjacency list, first checking each node in the adjacency list to see if it's in the traversed array - and then adding that node to the top of the stack with `array.prototype.unshift()`, which adds an element to the beginning of an array. This will loop through the entire adjacency list, updating our stack. After the loop is done, our stack will look like this:

      ```
      ["Rose", "Lydia", "Ada"]
      ```
    
    Because `stack.length` has three elements, it's truthy so the loop continues. Due to the following line `const currentNode = stack.shift();`, Rose is taken off the top of the stack and becomes the `currentNode`. Then the entire process continues - Rose is added to the list of `traversedNodes` and then her adjacency list is added to the stack and so on. Eventually, our DFS will find Ada and recognize that it's equal to the `targetNode`, meaning the method will return true.

    If we update our test to look for Thomas instead of Ada, it will pass. As we can see, in order to find Ada, we had to write an algorithm that would also find Thomas - or anyone else in the graph.

    There is one more thing we need to do. What about a node that exists but isn't reachable? That's why we added a node for Sarah. This node isn't connected to any other node in the graph - but it's still part of the graph, which means our initial conditional won't return `false`.

    Here's the test:

    *`__tests__/dfs.test.js`*

      ```
      ...
        test('should return false if the target node can not be reached from the starting node', () => {
          expect(graph.depthFirstReachable("Jasmine", "Sarah")).toEqual(false);
        });
      ...
      ```

    Fortunately, getting this test passing is easy. At some point, `stack.length` is going to be false (because it's zero). At that point, we know that we've traversed every node in the graph that is reachable from the starting node. So if we've traversed the entire graph and we still haven't found the node we are looking for, we know it's not reachable. Here's the updated code:

    *`src/graph.js`*

      ```
      depthFirstReachable(startingNode, targetNode) {
        if ((!this.adjacencyList.has(startingNode)) || (!this.adjacencyList.has(targetNode))) {
          return false;
        }
        let stack = [startingNode];
        let traversedNodes = new Set();
        while (stack.length) {
          const currentNode = stack.shift();
          if (currentNode === targetNode) {
            return true;
          } else {
            traversedNodes.add(currentNode);
            const adjacencyList = this.adjacencyList.get(currentNode);
            adjacencyList.forEach(function(node) {
              if (!traversedNodes.has(node)) {
                stack.unshift(node);
              }
            });
          }
        }
        return false;
      }
      ```
    This is our entire method. All we added is the `return false`; outside the while loop. As we can see here, we have a nested loop. The Big O of this particular algorithm is something akin to O(AB) - though that's not quite accurate because the B (the adjacency list for each node) is always changing - A (the stack itself) is always changing, too. It should be clear why that initial condition is even more helpful now. Searching the entire graph to determine that a node isn't reachable is a big task - best to check if one of the nodes doesn't exist in the first place.

    At this point, if you are feeling any confusion about how this algorithm works (and it's very understandable if you are), the next step is to intentionally break one of the tests and get into [Jest's](https://jestjs.io/docs/getting-started/) debug mode so you can step through the method and watch how variables change. The GIF below shows this - all the relevant variables have been added to Watch in the left-hand pane. This loop runs until the algorithm discovers that the Thomas node is, in fact, reachable from the Jasmine node.

    ![loop gif](https://drive.google.com/uc?export=view&id=1wehPndmAOGU5E1XrXbjn0GYDDJlpOWuv)

    It can also be useful to look at what would happen if we didn't flag traversed nodes. In other words, if our code looked like this instead:

      ```
      // This will not work! It will lead to infinite loops because we aren't flagging traversed nodes.

      depthFirstReachable(startingNode, targetNode) {
          if ((!this.adjacencyList.has(startingNode)) || (!this.adjacencyList.has(targetNode))) {
            return false;
          }
          let stack = [startingNode];
          while (stack.length) {
            const currentNode = stack.shift();
            if (currentNode === targetNode) {
              return true;
            } else {
              const adjacencyList = this.adjacencyList.get(currentNode);
              adjacencyList.forEach(function(node) {
                stack.unshift(node);
              });
            }
          }
          return false;
        }
      ```
    
    Take a look at what happens when we do this:

    ![loop gif 2](https://drive.google.com/uc?export=view&id=1URiojEZOeo9KEAnj8dpq5l_qjFOXY8qZ)

    We bounce back and forth between Jasmine and Rose because Rose is a friend of Jasmine and Jasmine is a friend of Rose back and forth forever and ever. We don't want this in our code.

    So that is a depth-first search algorithm in a nutshell. We use a stack to search through each branch. In this lesson, we just focused on finding whether a node is reachable. You will get a chance to practice some other implementations soon as well.


  * ### Breadth First Algorithms
    In the last lesson, we used a depth-first algorithm to determine if a target node in our friendship network is reachable from a starting node. In this lesson, we'll test and write a `Graph.prototype.breadthFirstReachable()` method. This method will do the exact same thing as our `Graph.prototype.depthFirstReachable()` method. The only difference is how the method will work. Instead of using a depth-first search, the method will use a breadth-first search.

    This will feel a little bit different than our usual TDD process because we will use the exact same tests that we used in the last lesson. In other words, our tests are already written for us! Go ahead and create a `bfs.test.js` file in `__tests__` with the exact same tests that are in the `dfs.test.js` file. Then update the name of the `Graph.prototype.depthFirstReachable()` method to `Graph.prototype.breadthFirstReachable()`. Our goal here is just to verify that everything in our method works correctly as we won't get tests passing one at a time. That's because the method itself is extremely similar to the one we wrote in the last lesson.

    The only difference is that we'll use a queue instead of a stack. That means each time we add new nodes to traverse, they will go to the end of the queue instead of the top of the stack. We will still evaluate the first element in the collection - but because it's a queue (and new items are being pushed to the end of the queue), this will be first-in, first-out (FIFO) instead of last-in, first-out (LIFO).

    Here's the full updated method:

    *`src/bfs.js`*

      ```
      breadthFirstReachable(startingNode, targetNode) {
          if ((!this.adjacencyList.has(startingNode)) || (!this.adjacencyList.has(targetNode))) {
            return false;
          }
          let queue = [startingNode];
          let traversedNodes = new Set();
          while (queue.length) {
            const currentNode = queue.shift();
            if (currentNode === targetNode) {
              return true;
            } else {
              traversedNodes.add(currentNode);
              const adjacencyList = this.adjacencyList.get(currentNode);
              adjacencyList.forEach(function(node) {
                if (!traversedNodes.has(node)) {
                  queue.push(node);
                }
              });
            }
          }
          return false;
        }
      ```

    The first three lines in the method look exactly the same as our previous method:

      ```
      if ((!this.adjacencyList.has(startingNode)) || (!this.adjacencyList.has(targetNode))) {
        return false;
      }
      ```

    This just ensures that both the `startingNode` and the `targetNode` exist in the graph and will get our first two tests passing. Once again, we don't want the overhead of doing a search algorithm if we can quickly determine whether a node exists in the list or not (which has O(1) time).

    Now let's take a look at the very subtle difference between DFS and BFS:

    *`src/bfs.js`*

      ```
      ...
        let queue = [startingNode];
        let traversedNodes = new Set();
        while (queue.length) {
          const currentNode = queue.shift();
          if (currentNode === targetNode) {
            return true;
          } else {
            traversedNodes.add(currentNode);
            const adjacencyList = this.adjacencyList.get(currentNode);
            adjacencyList.forEach(function(node) {
              if (!traversedNodes.has(node)) {
                queue.push(node);
              }
            });
          }
        }
        return false;
      }
      ...
      ```

    Other than the fact that we changed the name of our collection from stack to queue, there is only one small change in the method. Can you find it?

    The only difference is inside our inner loop when we do the following:

      ```
      queue.push(node);
      ```

    Meanwhile, in the last lesson, we did the following instead:

      ```
      stack.unshift(node);
      ```

    In both algorithms, we always traverse the first element in the collection each iteration through the loop. The difference is that when we add new elements to the queue, they go to the end of the collection, which means they will be traversed last (a breadth-first search). Meanwhile, with the stack, they go to the beginning of the collection, which means they will be traversed first (a depth-first search).

    That is really the only difference between a DFS and a BFS. Despite that fact, the order at which items are traversed can make a huge impact on the overall efficiency of an algorithm. For instance, if we knew there was a high probability that a node is a friend or a friend of a friend, we probably won't want to do a DFS. Instead, we could just use a BFS to first check all friends and then check all friends of friends.

    Now that we've learned the basics of DFS and BFS, we're well on our way to having a basic understanding of graphs and how we can traverse them. We also have some new problem-solving tools to work with. In addition to our knowledge of two new algorithms (DFS and BFS), we can also see how we can use queues and stacks, two different data structures, to solve different kinds of problems. Whether that is using JavaScript's event loop or writing an algorithm, these data structures are absolutely essential.

    #### Practice
    Now that we've learned the basics of graph theory, it's time to practice! First, walk through all the examples in the graph theory lessons and code along if you haven't already. Once you are done, you are ready for some additional challenges.
    * Create an application that shows connections in a **directed** graph. To do this, create an application with ten nodes which are all locations. They could be locations for your hometown, for a mythical realm, or for an imaginary city. First, use [draw.io](https://app.diagrams.net/) to draw your graph. Add nodes that are both one-way and two-way. (There are both one-way and two-way arrows in [draw.io](https://app.diagrams.net/).) Make sure that some nodes are unreachable to others.
    * Once you have completed your diagram, you will need to make some updates to your code. For instance, the `Graph.prototype.addNode()` method we wrote is undirected. You can update this method or add additional methods such as `Graph.prototype.addDirectedEdge()` and `Graph.prototype.addUndirectedEdge()`. It's up to you to determine how you want to implement a directed graph application. Just make sure to use TDD.
    * Add a counter to determine how many steps it takes to traverse the entire graph using both BFS and DFS.
    * **More challenging**: Try writing recursive versions of your DFS and BFS algorithms.
    * **Difficult**: Write a `Graph.prototype.shortestPath()` method to determine the shortest path between one node and another. The method should return the path (the nodes traversed) to get from the starting node to the targeted node.
    * Make the edges **weighted**. Each edge should now have a value associated with it that represents the distance between the two nodes. To do this, create an `Edge` class. Each instantiated edge should have a `distance` property. When calculating the shortest path, your `Graph.prototype.shortestPath()` method should take into account these distances.
    * Finally, create a `Graph.prototype.allPaths()` method that returns all paths - similar to what we might see with an application like Google Maps. Once again, this should return the paths between the starting node and the target node. The difference is that all the potential paths can be returned. Sort these paths from shortest to longest.



