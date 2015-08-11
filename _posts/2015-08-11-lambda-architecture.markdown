---
layout:     post
title:      Lambda Architecture
date:       2015-08-11 20:27:19
summary:    What are the pros and cons of Lambda Architecture?
categories: Architecture, Big Data
---

### The Lambda architecture

The Lambda architecture is designed to address the limitations surrounding the current batch-oriented Big Data platform (e.g., Hadoop Map Reduce), by combining batch and stream processing together to provide realtime business intelligence. The idea is to be able to run ad-hoc queries against all of available data to get results, and avoid querying a petabyte dataset every time by precomputing these results as views and queries them instead.

So how to build such as a system? The Lambda architecture consists of three layers: batch layer, serving layer, and speed layer. Here's how it looks like.

![Lambda architecture diagram]({{ site.url }}/assets/images/lambda_architecture.png)

The batch layer together with serving layer provides the batch processing component of the architecture. The batch layer has two functions: (i) managing the master dataset (an immutable, append-only set of raw data); (ii) to pre-compute the batch views. The serving layer indexes the batch views so that they can b queried in low-latency, ad-hoc way.

The speed layer provides the same business logic as the batch layer in that it computes views from the data it receives. However, the speed layer needs to compensate for the high-latency of the batch layer. It does this by computing realtime views, these realtime views contain only the delta results to supplement the batch views.

It is worth pointing out, in this architecture, all incoming data are inserted to both the batch and realtime systems, both systems are also accessed when querying the data.

### My thoughts

The Lambda architecture pattern offers scalability and fault-tolerant out of box. Using systems such as: Hadoop and Storm, the resulting system should be both linearly scalable and fault-tolerant.

However, maintaining two applications that doing pretty much the same thing can lead to future maintaince issues. These issues can be worsened if the two applications are implemented in two languages.

Additionally, the reason behind splitting stream and batch is based on the fact that batch system is more efficient for reprocessing, tasks such as: fixing errors, or reprocessing after delayed arrivals. But these distinctions between batch and streaming are quickly going away, especially with the introduction of the next generation Big Data platforms, such as: Spark.

### Related links

[Lambda architecture](http://lambda-architecture.net/)

[The Lambda architecture: principles for architecturing realtime Big Data systems](http://jameskinley.tumblr.com/post/37398560534/the-lambda-architecture-principles-for)

[Data architectures for robust decision making](http://www.slideshare.net/gwenshap/data-architectures-for-robust-decision-making)
