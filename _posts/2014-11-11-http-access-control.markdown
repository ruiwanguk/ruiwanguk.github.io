---
layout:     post
title:      Cross-Origin Resource Sharing (CORS)
date:       2014-11-11 20:27:19
summary:    Cross-origin resource sharing is commonly used by modern web applications, to make cross domain requests for script, image or data. 
categories: Web
---

While implementing the new version of PRIDE Cluster website, a colleague of mine added a simple implementation (see code below) of Cross-Origin Resource Sharing (CORS) into the web service. This got me a bit curious and decided to do a bit research around this topic.  

First, let's start from the origin of the problem. 

###Same-origin policy

The same-origin policy restricts how a document or script loaded from one origin can interact with a resource from another origin. The primarily used as a way of preventing the [Cross-site Request Forgery](http://en.wikipedia.org/wiki/Cross-site_request_forgery) attack. 

By definition, two documents have the same origin if the access protocol, port (if one is specified) and host are the same. 

For example, if you have a page: _http://www.ebi.ac.uk/pride_ is trying to access a web service at _http://www.ebi.ac.uk/pride/ws/archive/project/PXD000001_, this will not be blocked by the browser's same origin component. 

However, if you deploy the web service under another domain, _http://www.proteomexchange.org/pride/ws/archive/project/PXD000001_, this request will be automatically blocked. 

Now, there are exceptions to these rules depending on the browser, and these exceptions are often non-standard. For instance in Internet Explorer, it has two exceptions:

1. Trust zone: if both domains are in highly trusted zone. e.g. corporate domains. The same-origin policy will not apply. 
2. Port: IE doesn't include port as part of their same-origin component. Therefore, for _http://www.ebi.ac.uk:8080/pride_ can access content stored at _http://www.ebi.ac.uk:8081/pride_. 

###Cross-origin network access

The same-origin policy controls the interactions between two different origins. Such as: when you follow a link on a web page, or encounter a \<img\> tag which points to an image on a different domain. These interactions can be classified into three categories:

1. Cross-origin writes are generally allowed. Such as: links, redirections and form submissions. 
2. Cross-origin reads are typically not allowed, but this can be leaked by embedding (below). 
3. Cross-origin embeddings are typically allowed. 

Here are some examples of cross-origin embedding:

1. JavaScript with _\<script src="..."\>\</script\>_. It is worth highlighting that error messages for syntax errors are only available on same-origin scripts.
2. Images with _\<img\>_.
3. Media files with _\<video\>_ and _\<audio\>_.

###Cross-origin resource sharing (CORS)

The [Web Application Working Group](http://www.w3.org/2008/webapps/) has recommended the new [Cross-Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) mechanism. It provides a way for web servers to support cross-site access controls and enable secure cross-site data transfers. 

The CORS standard works by adding new HTTP headers that allow servers to describe the set of origins that are permitted to read that information using a web browser. Additionally, for HTTP request methods can cause side-effects on user data, for example, HTTP methods other than GET, HEAD or POST with certain mime types, the standard requires the browsers to 'preflight' the request. 'preflight' basically sends an HTTP OPTIONS request method to the server first, then once the server 'approves' the request, it sends the actually request to the server. The server can also notify the clients whether the Cookies or HTTP authentication data should be sent with requests. 


### CORS code example

Going back the code added by my colleague, It is easy to follow if you know a bit about Java Servlet and its filter (Original implementation can be found in a [blog post](https://spring.io/guides/gs/rest-service-cors/) from Spring Framework. 

<script src="https://gist.github.com/ruiwanguk/bd5f0274f3c547e9b156.js"></script>

From the code, you can see that CORS is archived by adding four HTTP response headers:

	Access-Control-Allow-Origin: *

	Access-Control-Allow-Methods: POST, GET, OPTIONS, DELETE

	Access-Control-Max-Age: 3600

	Access-Control-Allow-Headers: x-requested-with

__Access-Control-Allow-Origin__ header defines domains that are allowed as the origin. In this case, we are allowing any domain using a wild card. 

__Access-Control-Allow-Methods__ header defines the HTTP methods that are allowed. Here, we are allowing POST, GET, OPTIONS and DELETE.

__Access-Control-Max-Age__ header indicates how long the results of a preflight request can be cached in seconds. We are allowing 1 hour in this case. 

__Access-Control-Allow-Headers__ is used in response to a preflight request to indicate which HTTP headers to be used when making the actual request. 

There are also two other headers which were not used in our code example. 

	Access-Control-Allow-Credentials: true
	Access-Control-Expose-Headers: X-My-Custom-Header, X-Another-Custom-Header     

__Access-Control-Allow-Credentials__ indicates whether or not the response can be exposed when the credential is used. 

__Access-Control-Expose-Headers__ header lets the server whitelist headers that browsers are allowed to access. The above example allows _X-My-Custom-Header_ and _X-Another-Custom-Header_. 


###Other resources
[HTTP access control (CORS)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS) provides a general description and a good example on how to enable CORS.

[Cross-Origin Resource Sharing Standard](http://www.w3.org/TR/cors/) is the official standard by W3C published in 16 Jan 2014.

[Same-Origin Policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy) explains the policy in details. 

[CORS in action](http://arunranga.com/examples/access-control/) provides examples for CORS access.