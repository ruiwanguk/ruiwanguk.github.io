---
layout:     post
title:      Check thread completion in thread pool
date:       2014-10-15 08:47:19
summary:    Thread pools in Java provide a easy way of managing threads. But how do you know when your threads have been completed? 
categories: Java Concurrency
---

At work, we came across a seemingly trivial problem. We have a few thousands of files of the same format, we would like to parse all of them and generate some statistics. The size of these files varies ranging from a few megabyte to a few gigabyte, and we want to process these files in a reasonable time frame.   

Since these are independent chunks of work and can be run in parallel, we decided to process them concurrently in a thread pool using Java. Now, its relative easy spin off new threads, and most of the thread pools in Java provide some kind of queuing to prevent the pool from been overwhelmed. But does one know these threads have completed? In our case, we want to run some logic after all the threads have finished. 

In this post, I will demonstrate three different ways of archiving this goal. 

### 1. Shut down thread pool

All the thread pools provided by JDK implements [ExecutorService](http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/ExecutorService.html) interface, which provides two ways of shutting down the pool (see code below). 
	
	# create a fixed sized thread pool 
	ExecutorService executorService = Executors.newFixedThreadPool(10);
	 
	# shut down the pool orderly, previously submitted tasks are executed
	# but no new task will be accepted. 
	executorService.shutdown();

	# shut down the pool immediately, stop all actively executing tasks and 
	# halt all the waiting tasks
	executorService.shutdownNow();

Now, you probably think this is easy, just call the shutdown() method. Not quite, the catch is that the shutdown() method does not wait for the submitted tasks to complete execution. This means that if the code in your thread handles interruption, they will be terminated before completion. One possible fix is to remove the code handles interruption, but this is very bad practice. 

Alternatively, you can call the awaitTermination() method after calling the shutdown() method. This method will block until all the threads have been fully completed (see code below).

	# create a fixed sized thread pool 
	ExecutorService executorService = Executors.newFixedThreadPool(10);
	 
	try {
	  
	  # add tasks  
	  addTasks(executorService);

	} finally {

	  # shutdown thread pool but wait for all threads to finish	
	  executorService.shutdown();
	  executorService.awaitTermination(Long.MAX_VALUE, TimeUnit.DAYS);					
	}
	
Here is a full working [example](https://github.com/ruiwanguk/concurrency/blob/master/src/main/java/org/harrywang/concurrency/pool/completion/TaskCompletionUseShutdown.java).


### Other resources

[RMySQL vignette](http://cran.r-project.org/web/packages/RMySQL/RMySQL.pdf) provides a list of commonly used commands

[MySQL and R](http://www.r-bloggers.com/mysql-and-r/) is a nice blog post on additional RMySQL tips. 
