#Spark Examples #
##Creating a Spark Streaming Application in Java ##

Spark Streaming uses the power of Spark on streams of data, often data generated in real time by many producers. A typical use case is analysis on a streaming source of events such as website clicks or ad impressions. In this tutorial I'll create a Spark Streaming application that analyzes fake events streamed from another process. If you're new to running Spark take a look at the Getting Started With Spark tutorial to get yourself up and running. The code used in this tutorial is available on github.

Update: The code below has been updated to work with Spark 2.3. For the Spark 1.6 version see here.

The streaming data source
Spark Streaming can read input from many sources, most are designed to consume the input data and buffer it for consumption by the streaming application (Apache Kafka and Amazon Kinesis fall into this category). For this tutorial we'll feed data to Spark from a TCP socket written to by a process running locally.

The Java code for the data generating server. The server sets up a socket and generates data of the form "username:event", where event could be "login". There also is a python version of the server in the `python` folder.


Run this class before any of the streaming programs so they have something to get data from!

A "hello world" Spark Streaming application
Here is a "hello world" Spark Streaming application. It connects to the server running in EventServer.java, reads the data as it comes in and prints the data that's been received every `n` seconds.

See com.ahlquist.spark.SimpleStreamingApp


##A more complex Spark Streaming application ##
Moving on to a more complicated application we'll collect aggregate results from all the data that has come in on the stream so far. For this we need to enable checkpointing, otherwise Spark would need to keep a full history of the stream to recreate data lost due the failure of a Spark worker. With checkpointing Spark can pick up from the last checkpoint.

The Spark application below parses each event into a (userName, eventType) pair, then aggregates all the events over the life of the stream into per-user data. This is done through the updateStateByKey() method of Spark Streaming's PairDStream. Here we just print the output, in production calls to foreachRDD() would likely persist the data to a database or otherwise do something useful.
@See com.ahlquist.spark.EventStreamingApp


Adding more analysis
If you're interested in aggregating the data across all users, adding the following code would do that:


Conclusion
The above code code in this project is a template that can get you starting running a simple Spark Streaming application.
