---
layout:     post
title:      /dev/winter
date:       2015-01-29 20:27:19
summary:    My notes on /dev/winter 2015, one-day conference on software development at Cambridge.
categories: Conference
---

Below are my notes on [/dev/winter][1] 2015, a free, one-day event for software developers in Cambridge area.

### Overall

The conference is well organized, right mix of people, good venue and food. The balance feels right, the [programme][2] has a range of talks from machine learning to software design while maintaining practicability.

### First session: Modeling complex game economy with Neo4j

The first talk, given by [Yan Cui][3] from [Gamesys][4], was both clear and informative. Yan showed us how you could use [Neo4j][5], a NOSQL database for graph data, to help on modeling and balancing the economy of a large scale on-line game. 

The key takeaway messages are:

1. Use Neo4j to model the play content as a interconnected network. Each play item will be represented as a node, whereas the edge between the nodes describes the relationship between the items. For example, to make a bread you need some flour, both bread and flour will be a separate node in the play content network. Therefore, if you change the price of the flour, the price of the bread will also be affected. This effect can be investigated using simple Neo4j queries. 

2. Neo4j can only be run on one machine, and cannot be partitioned into different machines.

3. GameSys is using [genetic algorithmm][6] written in [F#][8] to model and propose solutions for balancing the game playability. 

4. GameSys's entire system is based on [Amazon EC2][7]. They are using technologies such as: [Amazon DynamoDB][9], [MemCached][10], [CouchDB][11].  


### Second session: Git for teams of one or more

I picked this practical session since our team is making the transition from SVN to Git. [Emma Jane][12], the workshop organizer, has been doing training on source control system for many years. You can find her slides [here][13], and she is also writing a book, called [Git for Teams][14].

The key takeaway messages are:

1. Documentation on agreed git workflow is necessary for the team. This document should outline how the team members interact with the code. It should also provide a governance strategy and branching strategy for the project.
2. Different branching strategies for different use cases. If you are doing scheduled release, you can use either [Gitflow][15] or [Simplified Gitflow][16]. If you are doing constant integration, you can choose either [Branch Per Feature][17] or [GitHub Flow][18].
3. To view the commit history and the relations between different commits, there are several tools you can use: [log][19], [gitk][20], [blame][21] and [bisect][22]. 
4. Emma highlighted the differences between [merging and rebasing][23]. She recommended against using rebasing if it does not provide additional benefits for the team.  

### Third session: Java is dead, long live Scala, Kotlin, Ceylon, etc

[Russel Winder][24] showed the differences between several JVM based languages, namely Java, Scala, Kotlin, Ceylon, Groovy, Fantom and Goush. The demo was based on approximating pi using quadrature. The main idea is to investigate scaling to see how different languages solve this parallel problem. You can find all his code examples from [here][25].

The key messages are:

1. Scala has its own implementation of data structures, whereas Kotlin is using Java's native data structures. 
2. Groovy can now be run on Android, all its nice functional programming features are already available and can be used on Android devices directly. 
3. Groovy is not as slow as before, it offers comparable performance to Java.
4. Scala, Kotlin, Groovy are all offering functional-style parallelization. This lead however has been cut short since the introduction of Java 8. 
5. Upgrade to Java 8 to take advantage of [G1 garbage collector][26], [Nashorn][27], [Lambda][28] features. 

### Fourth session: How to ditch the debugger and use logging instead

The main motivation behind this tutorial is to address the problem that one cannot attach the debugger to the application in Production, and debugging in a distributed environment is even more challenging. Logging can be used in a way which means you can diagnose problems very easily in both development and production.

The key messages are:

1. Event tracing, which is a specialized use of logging to record information about a system's execution. This tracing can be used to log different execution steps as well as record their performances. 

2. For any system, there are different types of events, these can be categorized as technical level events, e.g. database connection, out-of-memory, or domain level events, e.g. bank transaction, user login. Also, depending on the nature of the events, one can categorize them into unknown events or defined events. 

3. It is important to assign event type id for each event, as this will help to group and interpret the logs. In Java, a typical way of defining these event type IDs is to use *enum*. When used in a distributed system, these event type IDs can be passed between different services, this will enable the developers to trace the events occurred on different systems. 

4. It can be difficult to choose a severity level (such as ERROR, WARNING, etc) for logging which will be appropriate for production, as the severity of an event type may need to be changed after the application has been deployed based on experience of running the application. Different environment (Dev, Test, Production) may also require different severity levels. Therefore, it is better to have the severity level of all events configurable. There is a nice blog [post][30] about how to archive this in the production system.

5. Event-tracing based logging using the Elasticsearch ELK Stack, which includes [Elasticsearch][31], [Logstash][32] and [Kibana][33]. Logstash takes logs and other event data from the systems and store them in a central place. Elasticsearch is then used for indexing the logs for searching and access. Kibana works with Elasticsearch to visualize logs and time-stamped data. 

6. Use a virtual machine manager called [Vagrant][34], which allows the user to script virtual machine configuration as well as the provisioning, to setup the Elasticsearch ELK Stack. The demo can be found [here][35].

### Remarks
All in all, a very good one-day conference. Look forward to /dev/summer 2015. 


[1]: http://devcycles.net/2015/winter/ "dev winter"
[2]: http://devcycles.net/2015/winter/programme/ "dev winter programme"
[3]: http://theburningmonk.com/
[4]: http://www.gamesyscorporate.com/
[5]: http://neo4j.com/
[6]: http://en.wikipedia.org/wiki/Genetic_algorithm
[7]: http://aws.amazon.com/ec2/
[8]: http://fsharp.org/
[9]: http://aws.amazon.com/dynamodb/
[10]: http://memcached.org/
[11]: http://couchdb.apache.org/
[12]: https://twitter.com/emmajanehw
[13]: http://gitforteams.com/workshops/devwinter
[14]: http://gitforteams.com/resources/offsite.html
[15]: http://nvie.com/posts/a-successful-git-branching-model/
[16]: http://drewfradette.ca/a-simpler-successful-git-branching-model/
[17]: https://www.acquia.com/blog/pragmatic-guide-branch-feature-git-branching-strategy
[18]: http://scottchacon.com/2011/08/31/github-flow.html
[19]: http://git-scm.com/docs/git-log
[20]: http://git-scm.com/docs/gitk
[21]: http://git-scm.com/docs/git-blame
[22]: http://git-scm.com/docs/git-bisect
[23]: http://stackoverflow.com/questions/16666089/whats-the-difference-between-git-merge-and-git-rebase
[24]: http://www.russel.org.uk/
[25]: https://github.com/russel/Pi_Quadrature
[26]: http://www.oracle.com/technetwork/tutorials/tutorials-1876574.html
[27]: http://www.oracle.com/technetwork/articles/java/jf14-nashorn-2126515.html
[28]: http://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html
[29]: http://martinfowler.com/articles/microservices.html
[30]: http://blog.matthewskelton.net/2012/12/05/tune-logging-levels-in-production-without-recompiling-code/
[31]: http://www.elasticsearch.org/overview/elasticsearch
[32]: http://www.elasticsearch.org/overview/logstash
[33]: http://www.elasticsearch.org/overview/kibana
[34]: https://www.vagrantup.com/
[35]: https://github.com/SkeltonThatcher/velk-demo