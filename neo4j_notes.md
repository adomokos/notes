# Neo4J - Graph DB Tour

Panama Papers - triggered the idea of creating a graph DB to analyze the information.
It needed a new tool kit.

Nasa - Lessons Learned DB - A half century of collective NASA engineering knowledge
Problem: why the landing module turned over when it landed in the water?
They moved the data from Relational DB into Neo4J, they were able to find answers just a few hours.
NASA - "Neo4j saved well over two years of work and 1 million dollars."

## State of the Graph Union

"Forrester estimates that 25% of enterprises will be using graph databases by 2017." ~ 2014

Popularity of DBMS

Relational is declining, but 95% of the data is stored in relational.

Users:
Medium, LinkedIn
ADP
Comcast
ebay
Walmart

20 m downloads
400+ events
50k+ trained developers

Innovation is one of the key reasons ppl use a Graph DB.

700+ startups using it

More than 50% of enterprises are using graph databases

Recommendation engine is great for graph dbs.

### The Neo4j Graph Platform

Drivers & API, Discovery & Visualization
Graph Transactions
Graph Analytics is a new area

Neo4j Graph Algorithm Library
Neo4j Desktop
  * Mission critical for developers
  * Connect to local and remote Neo4j servers

### Machine Learning

ML is based on graphs.
Deep Learning, Evidence based, Recommendation Engines

The way Google pops-up relevant data on the right-hand side.

Study: people who are 3rd connected to me are better predictor than any information someone has about me.

We are moving to a highly connected future.
The brain is a graph.

### Evulotion of Graph Adoption

1st graph - connects like objects, people, computer, network, telco, etc.
Context paths - Desire for more context to follow connections
cross-connect
graph layers
auto graphs
Homework - Connect.

## Implementation Patterns

Create graph from tabular data
From Disparate Siles to cross-silo connections
From Data Lake Analytics to Real-Time operations

### Density Drives Value in Graphs
Metcalfe's Law of the Network (V=n^2)

5 hops = less value

100's of hops - More value

### Graphs are hungry for data

### Graph Algorithms
Predict Real World Events

1940s US Air Force crashes. 17 in a day.

Averages will fail you!!!

Do you have graph analytics problem?

* Propagation Pathways
* Flow & Dynamics
* Interaction and Resiliency

Averages aren't reality

Power Law Distribution

Graph Algorithms - Extract Structure and Infer Behavior

#### Pathfinding and Search

* Single-source shortest path
    Calculates "shortest" path between nodes
* All-pairs Shortest Path
* Minimum Weight Spanning Tree
    Calculates the path with the smallest values for visiting all nodes
* Parallel Breadth-First Search & Depth-First Search

#### Centrality
Determines the importance of distinct nodes in the network
* PageRank - which nodes have the most overall influence
    Used for urban planning, track extinctions of animals, fighting toxic waste
* Closeness - which nodes are able to reach entire group the fastest
* Betweenness - which nodes are the bridges between clusters (most shortest paths)
* Degree

#### Community Detection
* Label Propagation - spreads labels based on neighbors to infer clustersc:w
* Union Find/Weakly Connected Components
* Strongly Connected Components
* Louvain Modularity -Measures the pressumed accuracy of community grouping.
* Triangle-count & clustering coefficient

### Tools
* Neo4J Native Graph
* Cypher Query Language
* Wide Range of APOC Procedures
* Optimized Graph Algorithms

How to:
* Call as Cypher procedure
* Pass in specification (label, Prop, Query) and configuration

Cypher Projection

DEMO - Game of Thrones - examining relationships among characters

### Polyglot Persistance

Discrete Data <-> Connected Data

Transactional Graphs
* Fraud detection
* REal-time recommendations
* Network and IT operations management
* Knowledge Graphs
* Master Data Mgmt

### neo4j DB
Index-Free Adjacency
ACID Foundation
Full-Stack clustering
Languages drivers tooling
Graph Engine
Hadware Optimization

### ACID Graph Writes: Required for Graph Transactions

### Graph analytics
Graph boosted Artificial Intelligence

