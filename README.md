# Distributed_Sentiment_Analysis_Of_Yelp_With_Spark

### Dataset:
For this project, I use Python and Spark to perform some graph analysis, using a graph of the Gnutella server network.   In this graph, each node represents a server, and each directed edge represents a connection between servers in Gnutella's peer-to-peer network.  The data file `p2p-Gnutella08-adj.txt`, represents the graph as an adjacency list.   Each server (node) is identified by a unique number, and each line in the file gives the adjacency list for a single server.
For example, this line:
> 91	243	1923	2194

gives the adjacency list for server `91`.   It indicates that there are edges from server `91` to servers `243`, `1923`, and `2194`.    According to the Stanford Network Analysis Project, which collected these data.

Link: http://snap.stanford.edu/data/p2p-Gnutella08.html.

### Objective:
The main objective for this project is to perform *single source personalized page rank* over the Gnutella graph. Personalized page rank is like ordinary page rank except:
- One node in the graph is designated as the *source* node. Personalized page rank is performed with respect to that source node.
- Personalized page rank is initialized by assigning all probability mass to the source node, and none to the other nodes. In contrast, ordinary page rank is initialized by giving all nodes the same probability mass.
- Whenever personalized page rank makes a random jump, it jumps back to the source node. In contrast, ordinary page rank may jump to any node.
- In personalized page rank, all probability mass lost dangling nodes is put back into the source nodes.  In ordinary page rank, lost mass is distributed evenly over all nodes.


### Input Parameters:
- source node id (a positive integer)
- iteration count (a positive integer)
- random jump factor value (a float between 0 and 1)

The function performs personalized page rank, with respect to the specified source node, over the Gnutella graph, for the specified number of iterations, using Spark.

The output of your function is a list of the 10 nodes with the highest personalized page rank with respect to the given source. For each of the 10 nodes, return the node's id and page rank value as a tuple. The list returned by the function looks like this: `[(node_id_1, highest_pagerank_value), ..., (node_id_10, 10th_highest_pagerank_value)]`

#### Alternative Stopping Criterion:

It is also common to have iterative algorithms that run until some specified termination condition is reached.

For example, for page rank, suppose the p_i(x) represents the probability mass assigned to node x after the ith iteration of the algorithm.  (p_0(x) is the initial probability mass of node x.)   We define the change of x's probability mass on the ith iteration as |p_i(x)-p_{i-1}(x)|.   Then, we can iterate personalized page rank until the maximum (over all nodes) change is less than a specified threshold, i.e, until all nodes' page ranks have converged.

The second function iterates until the maximum node change is less than 0.5/N, where N represents the number of nodes in the graph.
This version of the function takes only two inputs: the source node id and the random jump factor.
