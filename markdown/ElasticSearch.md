![](images/ElasticSearch/elasticsearch-logo.png) 
Note: The first tutorial I took at OSCON was on ElasticSearch.  


##What is ElasticSearch
Note: Elastic is a server that can be used do to full-text search on arbitrary data.


####What is ElasticSearch
##Lucene 
Note: Lucene is a software library that allows the user to to do searching in documents, it is powerful but difficult to use.

ElasticSearch abstracts away from the user the need to handle processing and storing the documents 
and provide an easier method of searching than calling the Lucene API directly. 


####What is ElasticSearch
##Scalable
Note: ElasticSearch makes it easy to add nodes to the cluster at runtime to make scaling easy. 

In fact the first thing we needed to do in the training was modify the configuration so that everyone's laptops didn't automatically join as a single cluster.


####What is ElasticSearch
##Consistent
##Available
##Partition Tolerant
Note: As with any distributed system the CAP theorem rears its ugly head.

Consistent - all nodes see the same data at the same time 

Available - a guarantee that every request receives a response about whether it was successful or failed 

Partition Tolerant - the system continues to operate despite arbitrary message loss or failure of part of the system, network partitions are when two or more running nodes can't talk to each other.


####What is ElasticSearch
##Consistent
##Available
##~~Partition Tolerant~~
Note: Elastic search is mostly eventually consistent. 

Availability is variable

Elastic Search implemented their own leader election algorithm called Zen, it has issues where it will easily end up in a Twin Brain state during even minor network partitions.

Twin Brain is the reason that Availability and Consistency are not fully promised.

http://aphyr.com/posts/317-call-me-maybe-elasticsearch


##ElasticSearch vs Solr
Note: Both ElasticSearch and Solr, which is using in the PPSS, are based on Lucene.  

Solr is designed to search many different types of text input from text to rich documents such as PDF and MS Doc.  

ElasticSearch expects JSON formatted text, and is popular because limiting the input makes it easier to get up and running.


##How to use ElasticSearch
Note: So now that we know what elastic search is, lets look through some simple examples of how it is used.


######How to use ElasticSearch
###REST service
Note: The only way to access elastic search is through its exposed REST services. 

This makes it simple to program against, but when you want to just experiment you can just


######How to use ElasticSearch
###Use cURL
    curl -X ${httpVerb} '${url}' -d '${requestBody}'
Note: If you are not familiar with curl, it is a command line interface for interacting with any URL using a wide range of
protocols, and it is perfect for trying out ElasticSearch.

Just replace the values in the squiggly brackets and run it.  Request body is optional.


######How to use ElasticSearch
### Create an index
    $ curl -X PUT 'http://localhost:9200/test/'
###Response Body
    {"acknowledged":true}
Note: An index in ElasticSearch is similar to a database Table or a NoSql Document Store, because ElasticSearch's entire existence is 
to index text for searching the name Index fits even though when thinking about a SQL index it does not. 

To create the index we use the PUT command to send a URL where the text after the port is the index name, 
in this example we are creating an index called test. 

The response body is simple JSON format that shows that the request was successful.


######How to use ElasticSearch
###Create a document
    $ curl -X PUT 'http://127.0.0.1:9200/test/book/1' -d '{
        "title": "All About Fish",
        "author": "Fishy McFishstein",
        "pages": 3015
    }'
####Response Body
	{"_index":"test","_type":"book","_id":"1","_version":9,"created":true}
Note: To create a document we need just include a request body that is the entire JSON document that we want to be able to search.

The response body tells us that the request created the document in the "test" index, the document is of type "book" and the new document has an id of "1".

So we we look back at the url we can tell that the pattern just index slash type slash id to create a document.


######How to use ElasticSearch
###Read a document
    $ curl -X GET 'http://127.0.0.1:9200/test/book/1'
###Response Body
	{"_index":"test","_type":"book","_id":"1","_version":9,"found":true,"_source":{
	    "title": "All About Fish",
	    "author": "Fishy McFishstein",
	    "pages": 3015
	}}
Note: If we want to ready the document back out you just use the same URL and change the http verb to GET. 

The response includes the original document in the underscore source variable.  The rest of the variables contain meta data about the document.


######How to use ElasticSearch
###Read a document formatted
    $ curl -X GET 'http://127.0.0.1:9200/test/book/1?pretty'
###Response Body
    {
      "_index" : "test",
      "_type" : "book",
      "_id" : "1",
      "_version" : 9,
      "found" : true,
      "_source":{
        "title": "All About Fish",
        "author": "Fishy McFishstein",
        "pages": 3015
    }
    }
Note: Of course the default response is fine for processing programmatically 

but if you want it to be human readable you can just add the pretty parameter to the URL 

You can see that the source maintained the exact formatting we passed in during the create step in both requests.


######How to use ElasticSearch
###Delete a document
    $ curl -X DELETE 'http://localhost:9200/test/book/1?pretty'
###Response Body
    {
      "found" : true,
      "_index" : "test",
      "_type" : "book",
      "_id" : "1",
      "_version" : 3
    }
Note: To delete a document from the index is also just switching the HTTP verb to the DELETE

The request will return a 200 if the document existed or not, and the response bodies found variable contains the information about if a document was actually deleted or not.


######How to use ElasticSearch
###Read a missing document
    $ curl -X GET 'http://localhost:9200/test/book/1?pretty'
###Response Body
    {
      "_index" : "test",
      "_type" : "book",
      "_id" : "1",
      "found" : false
    }
Note: If you attempt to get a document that does not exist, the response's found variable reflect that and there is no source.


######How to use ElasticSearch
###Search
    $ curl -X GET \
    > 'http://127.0.0.1:9200/test/book/_search?q=title:Python&pretty'
###Response Body
    {
	  "took" : 2,
	  "timed_out" : false,
	  "_shards" : {
		"total" : 5,
		"successful" : 5,
		"failed" : 0
	  },
	  "hits" : {
		"total" : 2,
		"max_score" : 0.15342641,
		"hits" : [ {
		  "_index" : "test",
		  "_type" : "book",
		  "_id" : "4",
		  "_score" : 0.15342641,
		  "_source":{"title": "Python for Dogs", "author": "Rufus Milan", "pages": 4, "publication date": "2012-12-20", "category": "Web Development", "summary": "A unique focus on only the parts of the Python language that are interesting to can  ines.", "rating": 5 }
		}, {
		  "_index" : "test",
		  "_type" : "book",
		  "_id" : "5",
		  "_score" : 0.15342641,
		  "_source":{"title": "Python for Rocket Scientists", "author": "Neil Sagan", "pages": 563, "publication date": "2014-03-02", "category": "Web Development", "summary": "A unique focus on only the parts of the Python language that are interesting to rocket scientists", "rating": 5 }
		} ]
	  }
	}
Note: This request URL shows that we are searching in the test index for documents of type book with a query that specifies that the title should contain the text python.

Now the default elastic search is configured to ignore case, and to break on whitespace, so that won't be a problem. 

We can see that the response contains information about how the search performed as well as the number of hits that matched the search.

Scrolling down into the hits section we can see that if found 2 documents that matched as well as the total best matching score found.


######How to use ElasticSearch
###Search 
    $ curl -X GET 'http://localhost:9200/test/book/_search?pretty' -d '{
		"query" : {
				"match" : { "title" : "Python" }
		}
	}'
###Response Body
	{
	  "...": "...",
	  "hits" : {
		"total" : 2,
		"max_score" : 0.15342641,
		"hits" : [ {
		  "_index" : "test", "_type" : "book", "_id" : "4",
		  "_score" : 0.15342641,
		  "_source":{"title": "Python for Dogs", "...": "..." }
		}, {
		  "_index" : "test", "_type" : "book", "_id" : "5",
		  "_score" : 0.15342641,
		  "_source":{"title": "Python for Rocket Scientists", "...": "..." }
		} ]
	  }
	}
Note: Doing the search setup completely in the URL will quickly lead to a horribly complex unreadable mess.

So Elastic Search supports moving the search into the request body using a JSON formatted query language. 

This example query is identical to the previous URL based query, although I removed much of the response body and changed the formatting to make the response better fit ont the screen.

One thing to take away from this when designing your own REST services is the fact that while http verb GET doesn't by convention use a request body, there is nothing in the standard that prevents it.  


######How to use ElasticSearch
###Filter Search 
    $ curl -X GET 'http://localhost:9200/test/book/_search?pretty=true' -d '{
		"query" : {
			"filtered" : {
				"filter" : {
					"term" : {
						"category" : "web"
					}
				},
				"query" : {
					"match" : {
						"summary" : "Python"
					}
				}
			}
		}
	}'
Note: The document either matches the entire or it does not to get a boolean response.  Filters do an exact match on a word, so if you want to match on plural forms of a word you can't use a filter.

Queries are relevance scored which means partial matches still count.

Filters are orders of magnitude faster than queries, not only because the logic is simpler, but also because they are cacheable.

Filter when you can, query when you must.


######How to use ElasticSearch
###Filter Search Response
	{
	  "...": "...",
	  "hits" : {
		"total" : 2,
		"max_score" : 0.067124054,
		"hits" : [ {
		  "_index" : "test", "_type" : "book", "_id" : "4",
		  "_score" : 0.067124054,
		  "_source":{"category": "Web Development", "summary": "... Python ...", "...": "..."}
		}, {
		  "_index" : "test", "_type" : "book", "_id" : "5",
		  "_score" : 0.067124054,
		  "_source":{"category": "Web Development", "summary": "... Python ...",  "...": "..."}
		} ]
	  }
	}
Note: The results returned from the filtered query are the same as the original query for all Python except the scores have changed.

ElasticSearch's query language is powerful and complex, it allows arbitrary nesting of logic, and I am not an expert so we will stop here.


###ElasticSearch Problems
Note: Of course elastic search does have its gotchas, and we should go over some of the more common.


######ElasticSearch Problems
###Implicit Mappings
Note: When you add a document to an index that does not have a schema defined it will do implicit mappings of each fields datatype.

If you have a string field that contains a string that looks like a date, then it will implicitly map that field to a date field.  

Then when you try and add a document that contains something other than a date in that field the add will fail.


######ElasticSearch Problems
###Twin Brain
Note: When we were discussing the CAP theorem we mentioned that multiple masters or twin brains is a problem that occurs when there is a network partition. 

This problem is so common in ElasticSearch that it should be mentioned again, this problem causes the data stored to be unreliable the more scaled ElasticSearch is.

You can configure the Zen leader election to not start unless it can reach a quorm of half the nodes plus one to lessen twin brain, but it is still possible for some types of partial network partitioning.

The easiest way to prevent twin brain from becoming a problem is to just continually repopulate the data in elastic search.


######ElasticSearch Problems
###No Security
Note: Elastic search should be behind a firewall, given a un-guessable cluster name, and configured to not "discover" nodes outside the firewall.

