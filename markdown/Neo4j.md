![](images/Neo4j/neo4j.png)
Note: One tutorial at OSCON was on the Graph Database Neo4j. 

But, before we can discuss Neo4j and graph databases we should clarify what a graph is. 


####A Graph is made up of:
##Vertices
##Edges 
Note: A Vertices is the graph theory name for what computer science normally calls a node. 

An edge is the relationship between two nodes. 

Note that an edge can't go from a node back to itself in a graph. 


#####Graph theory Warning
##O(n<sup>2</sup>)
Note: If you are looking into applying algorithms from Graph Theory you need to be very careful. 

Because, Mathematicians consider any Graph Algorithm that is Polynomial (n^2) to be efficient. 

But, any developer that delivers a polynomial time solution to a large dataset needs to go talk to the performance team.


####Neo4j's graph is
###heterogeneous
###disconnected
###directed
###multigraph
Note: The data representation of a graph in Neo4j is a specific type of graph that is:

heterogeneous - which means that you can have nodes of different types 

disconnected - you can have two nodes with no path between them 

directed - the edge relationship has a direction, with a starting node and a target node.

multigraph - you can have multple links between two nodes 

BUT a node can't have a reference to itself because then it wouldn't be a graph


####Neo4j's query language
##Cypher 
Note: Graphs are complex, but some very smart people have worked very hard at making it easy to access the data in Neo4j.

Cypher is Neo4j's custom query language, it is designed to be "easy" to read, not easy to type.  
The designers basically went back to ASCII art and designed the query langage to be ASCII art of Paths through the graph. 
I am going to use Cypher examples of how to access the Neo4j data as an example of how to use a Graph database as well as being an intro to Cypher.


Cypher

    MATCH (a) RETURN a LIMIT 100
![](images/Neo4j/blank.png)
Note: This is the simplest Cypher query there is. 

The "MATCH" keyword tells Cypher what pattern of data in the graph that you are looking for. 

The parentheses tell Cypher that you want to match and node, 
because an open and then close parentheses is the closest ASCII art can get to a circle. 
Then the letter a in the parentheses is the variable name to assign the found node to. 

The "RETURN" starts the clause telling Cypher what data the user wants back in the query. 
In this case we just want the node value we assigned to 'a'. 

The "LIMIT" clause is telling Cypher how many result rows to return, 
as this request is unbound by any qualifier the results would be every node in the database if we didn't limit the size.


Cypher

    MATCH (a) RETURN a LIMIT 100
![](images/Neo4j/selectAllData.png)
Note: If you were writing a program against Neo4j you would get the results from the Cypher query through a standard row based api.


Cypher

    MATCH (a) RETURN a LIMIT 100
![](images/Neo4j/selectAll.png)
Note: The Neo4j database does provide a nice web based data visualization tool that can be used to test Cypher queries. 

This screenshot shows a good example of why the visualization of a complex graph isn't all that useful for more than testing or training. 


Cypher

    MATCH (a)--(b) RETURN a,b LIMIT 100
![](images/Neo4j/blank.png)
Note: The match clause in this query is requesting all nodes that are connected, so we will exclude any nodes that are disconnected. 
As you can see the two dashes represents the ASCII art for an edge or relationship in the graph.


Cypher

    MATCH (a)--(b) RETURN a,b LIMIT 100
![](images/Neo4j/selectAllRelationships.png)
Note: Which doesn't affect the displayed graph much, however it would change the data available in each row of the data returned by the api. 

The Graph looks smaller because of the fact that each pair is returned, so when the same node has mutiple relationships it gets returned in 
multiple rows but is only displayed once, the previous graph every node was returned exactly once, so the limit of 100 rows showed more nodes. 


Cypher

    MATCH (p:Person)--(m:Movie) RETURN p, m
![](images/Neo4j/blank.png)
Note: Up until now we have been saying that we want any type of node, but for the graph to be useful we need to be able to specify the node
types that we are intereseted in. 

You can do that by using a colon in the node with the type name.  So this query would exclude any nodes of 
type "TV_SHOW" (if there were any in the database).


Cypher

    MATCH (p:Person)--(m:Movie) RETURN p, m
![](images/Neo4j/allPeopleAndMoviesWithARelation.png)
Note: The graph got rather large as we removed the limit clause from the request.


Cypher

    MATCH (p:Person)-->(m:Movie) RETURN p, m
![](images/Neo4j/blank.png)
Note: If you remember we stated at the beginning that Neo4j is a directed graph, but so far we have been ignoring that. 

To request that you are only interested in relationships that go from a person to a movie you just turn the line into an arrow. 

You can specify either direction by drawing the arrow in the correct direction using less than or greater than signs, to create more ASCII art.


Cypher

    MATCH (p:Person)-->(m:Movie) RETURN p, m
![](images/Neo4j/allPeopleRelatedToMovies.png)
Note: So this graph shows every Person that had a relationship to a Movie, but not any Person that the movie was related to. 


Cypher

    MATCH (p:Person {name:"Tom Hanks"})-->(m:Movie) RETURN p, m
![](images/Neo4j/blank.png)
Note:  So far all the queries have show pretty pictures, but haven't been very useful for anything. 

For the query to be useful you will probably need to specify a node to start from.

In this case we are stating that we want to start from a Person node that has a name of "Tom Hanks", but you can specify any number
of attribute fields here using standard JSON syntax.


Cypher

    MATCH (p:Person {name:"Tom Hanks"})-->(m:Movie) RETURN p, m
![](images/Neo4j/TomHanksMovies.png)
Note: This is probably the first graph that is simple enough for the image to be useful. 

But remember you would be accessing Neo4j through the API and getting this as 13 rows of data.


Cypher

    MATCH (a:Person {name:"Tom Hanks"})-[r:ACTED_IN]->(m:Movie) RETURN a, r, m
![](images/Neo4j/blank.png)
Note: Up till now we accepted any relationship, but you can specify a type for a relationship the similar 
to the way you specify a type for a node. 

The only real difference is that you use a square bracket inside the dashes that make up the edge.  
This works with or without the direction being specified. 

You can also specify attributes on the edge the same way you can for a node, although I won't be showing any examples of that.


Cypher

    MATCH (a:Person {name:"Tom Hanks"})-[r:ACTED_IN]->(m:Movie) RETURN a, r, m
![](images/Neo4j/TomHanksActedIn.png)
Note: You should note that the visualization graph displays some relationships that shouldn't have been returned by the query, 
this is a bug in the way the visualization is built, when accessing the data through the api you won't get hte incorrect relationships.


Cypher

    MATCH 
	  (a:Person)-[:ACTED_IN]->(m:Movie {title:"The Matrix"})<-[:DIRECTED]-(d) 
	RETURN a, m, d
![](images/Neo4j/blank.png)
Note: This example shows how you can build up more complex relationships using Cypher. 

By specifying two different relationships to the same node you are effectivly requesting the set of Actors and the
set of Directors for the given movie.

While this query might take a second to parse right now, you would be suprised how easy it gets to read after a couple of hours of practice. 

FYI: the formatting isn't required, it is just to get it to fit on the slide


Cypher

    MATCH 
	  (a:Person)-[:ACTED_IN]->(m:Movie {title:"The Matrix"})<-[:DIRECTED]-(d) 
	RETURN a, m, d
![](images/Neo4j/TheMatrixActorsAndDirectors.png)
Note: You can see from the graph the two different relationship types selected. 


Cypher

    MATCH (a:Person)-[:ACTED_IN]->(m:Movie)<-[:DIRECTED]-(a) RETURN a, m
![](images/Neo4j/blank.png)
Note: By using the variable A in both nodes, we can get all people that have both ACTED_IN and DIRECTED the same movie


Cypher

    MATCH (a:Person)-[:ACTED_IN]->(m:Movie)<-[:DIRECTED]-(a) RETURN a, m
![](images/Neo4j/ActedAndDirectedSameMovie.png)
Note:  So with the current dataset we find three people that have directed themselves in a movie.


Cypher

    MATCH 
	  path1 = (a:Person)-[:ACTED_IN]->(m1:Movie), 
	  path2 = (a:Person)-[:DIRECTED]->(m2:Movie) 
	RETURN path1, path2
![](images/Neo4j/blank.png)
Note:  But if we want all of the people that have both ACTED_IN and DIRECTED even if it wasn't the same movie we can define two separate paths. 

You can see that because we used the same variable 'a' for both Person nodes that links the two paths together.


Cypher

    MATCH 
	  path1 = (a:Person)-[:ACTED_IN]->(m1:Movie), 
	  path2 = (a:Person)-[:DIRECTED]->(m2:Movie) 
	RETURN path1, path2
![](images/Neo4j/ActedAndDirectedDifferentMovies.png)
Note:  So in this limited dataset we find the same three director/actors.

We could have done this query with a single path but this is an easy to understand example of a two path query.


Cypher

    MATCH 
	  (a:Person)-[:ACTED_IN]->
	    (m:Movie)<-[:ACTED_IN]-(b:Person {name:"Kevin Bacon"}) 
	RETURN a, m, b
![](images/Neo4j/blank.png)
Note:But of course you can't have a discussion of how movies link actors together without discussing Kevin Bacon. 

This query simply requests all every actor that has been in a movie with Kevin Bacon. 

Which can be rephrased as all actors with a Bacon number of 1.


Cypher

    MATCH 
	  (a:Person)-[:ACTED_IN]->
	    (m:Movie)<-[:ACTED_IN]-(b:Person {name:"Kevin Bacon"}) 
	RETURN a, m, b
![](images/Neo4j/firstDegreeKevinBaconWithMovies.png)
Note: While this is a nice graph how would we find all the actors with a Bacon number of 2. 

We could just chain the relationships, but as the Bacon number goes up the query gets longer and longer and harder and harder to read and maintain.


Cypher

    MATCH (a:Person)-[:ACTED_IN]->(:Movie)<-[:ACTED_IN]-(b:Person) 	  
    CREATE (a)-[r:WORKED_WITH]->(b)
	RETURN a, r, b
![](images/Neo4j/blank.png)
Note: One way to find simplify the request is to create a new relationship directly between actors without the intermediate node of a movie.

In the first line we again match to people that have acted in the same movie.

Then in the second line we can use the variables defined in the first line to create a new relation between those two people.


Cypher

    MATCH (a:Person)-[:ACTED_IN]->(:Movie)<-[:ACTED_IN]-(b:Person) 	  
    CREATE (a)-[r:WORKED_WITH]->(b)
	RETURN a, r, b
![](images/Neo4j/workedWithRelationships.png)
Note: We now have a graph of people linked without the movie nodes being present.


Cypher

    MATCH 
	  (a:Person)-[r:WORKED_WITH]->(b:Person {name:"Kevin Bacon"}) 
	RETURN a, r, b
![](images/Neo4j/blank.png)
Note:  Using the new relationship we can again request all people with a Bacon number of 1 without mentioning Movie at all.


Cypher

    MATCH 
	  (a:Person)-[r:WORKED_WITH]->(b:Person {name:"Kevin Bacon"}) 
	RETURN a, r, b
![](images/Neo4j/firstDegreeKevinBaconWithoutMovies.png)
Note: Now the graph has more relations showing as each set of people that was grouped by together by a movie now has a direct relationship. 


Cypher

    MATCH 
	  (a:Person)-[r:WORKED_WITH*1..3]->(b:Person {name:"Kevin Bacon"}) 
	RETURN a, r, b
    LIMIT 100
![](images/Neo4j/blank.png)
Note: Thankfully now that we have a single relationship getting all people with a Bacon number of 3 or less is relatively easy.

Cypher allows you to specify number of relationships to follow by adding a * to the end of the relationship square bracket definition.


Cypher

    MATCH 
	  (a:Person)-[r:WORKED_WITH*1..3]->(b:Person {name:"Kevin Bacon"}) 
	RETURN a, r, b
    LIMIT 100
![](images/Neo4j/workedWithKevinBaconThirdDegree.png)
Note: The graph gets really complicated quickly using this notation to get all nodes and relationships, however normally you would be processing the results programmatically.

Warning, this feature can bring the database to its knees as the number of possible paths to explore goes up. 

On a rather powerful laptop the Bacon number of 4 query didn't return after 2 hours.


Cypher

    MATCH 
	  (a:Person {name:"Keanu Reeves"})-[r:WORKED_WITH*1..3]->
	    (b:Person {name:"Kevin Bacon"}) 
	RETURN a, r, b
![](images/Neo4j/blank.png)
Note: So if you wanted to find all of the paths between Keanu Reeves and Kevin Bacon with a path of 3 or less it is trivial. 

Just add the the specific name to the starting Person node.


Cypher

    MATCH 
	  (a:Person {name:"Keanu Reeves"})-[r:WORKED_WITH*1..3]->
	    (b:Person {name:"Kevin Bacon"}) 
	RETURN a, r, b
![](images/Neo4j/keanuReevesThirdDegreePathsToKevinBacon.png)
Note: This graph should be illustrative of why increasing the possible paths increase the load on the server exponentially. 

But in reality we don't care about all of the paths, to find the Bacon number we just want the shortest path.


Cypher

    MATCH p = shortestPath(
	  (a:Person {name:"Keanu Reeves"})-[r:WORKED_WITH*..3]->
	    (b:Person {name:"Kevin Bacon"})
    ) RETURN p
![](images/Neo4j/blank.png)
Note: Cypher of course has a built in function for finding the shortest path in a query. 


Cypher

    MATCH p = shortestPath(
	  (a:Person {name:"Keanu Reeves"})-[r:WORKED_WITH*..3]->
	    (b:Person {name:"Kevin Bacon"})
    ) RETURN p
![](images/Neo4j/keanuReevesShortestPathToKevinBacon.png)
Note: Realize that it is returning the first shortest path that it found, so if there are multiple paths of length two the return path is indeterminate. 

If you care about the different shortest paths there is another method called allShortestPaths.


Cypher

    MATCH p = shortestPath(
	  (a:Person {name:"Keanu Reeves"})-[r:WORKED_WITH*..3]->
	    (b:Person {name:"Kevin Bacon"})
    ) RETURN length(p)
![](images/Neo4j/blank.png)
Note: But if you only care about the Bacon number rather than the specific path you can call the length method on the path. 


Cypher

    MATCH p = shortestPath(
	  (a:Person {name:"Keanu Reeves"})-[r:WORKED_WITH*..3]->
	    (b:Person {name:"Kevin Bacon"})
    ) RETURN length(p)
![](images/Neo4j/keanuReevesLengthOfShortestPathToKevinBacon.png)
Note: Whichs shows us clearly that Keanu Reeves has a bacon number of 2.


Cypher

    MATCH p = (a:Person {name:"Keanu Reeves"})-[r:WORKED_WITH*1..4]->
	    (b:Person {name:"Kevin Bacon"}) 
	RETURN length(p), count(length(p))
![](images/Neo4j/blank.png)
Note: So if you wanted to know how many paths there are of each length between Keanu and Kevin you can return the length of the path and use the count aggregate method.


Cypher

    MATCH p = (a:Person {name:"Keanu Reeves"})-[r:WORKED_WITH*1..4]->
	    (b:Person {name:"Kevin Bacon"}) 
	RETURN length(p), count(length(p))
![](images/Neo4j/keanuReevesNumberOfPathsToByLengthKevinBacon.png)
Note: In SQL you would have to use a "group by" clause to make the aggregate work, in Cyper it assumes any field that isn't an aggregate method is part of the grouping.

There are additional methods available in Cypher that we won't be covering here. 

Are there any questions before we finish the Cypher portion of the presentation?

