---
layout: post
title: How RESTful is your service?
date: '2013-09-09T21:57:00.000+02:00'
author: Fanie Reynders
tags:
- rest
- RESTful
- ASP.NET Web API
- Roy Fielding
- RCP
- HTTP
- http services
- RESTful constraints
modified_time: '2013-09-09T21:59:53.549+02:00'
thumbnail: http://2.bp.blogspot.com/-3153RbsiwYw/Ui4IcVksckI/AAAAAAAAAW8/qbxeHT8jrxI/s72-c/RESTFUL.jpg
blogger_id: tag:blogger.com,1999:blog-2911174713310708108.post-4004231871059638979
blogger_orig_url: http://www.faniereynders.com/2013/09/how-restful-is-your-service.html
---

Web services are everywhere these days, from social media platforms to enterprise line-of-business solutions. It is extremely important to be able to expose <a href="http://en.wikipedia.org/wiki/Application_programming_interface" target="_blank">APIs</a> for your solution, in order to reach more external consumers on multiple platforms. As a modern trend, we see that more and more APIs are surfacing claiming to be <a href="http://en.wikipedia.org/wiki/Representational_state_transfer#cite_note-13" target="_blank">RESTful</a>, but in fact, they're actually just good old <a href="http://en.wikipedia.org/wiki/Remote_procedure_call" target="_blank">remote procedure calls (RPC)</a>.

<!--more-->

Many of these proclaimed "REST"-style services are implemented not even knowing what the term means, so I've decided to help try explain REST and how to get your services RESTful.

<a href="http://2.bp.blogspot.com/-3153RbsiwYw/Ui4IcVksckI/AAAAAAAAAW8/qbxeHT8jrxI/s1600/RESTFUL.jpg" imageanchor="1"><img border="0" src="http://2.bp.blogspot.com/-3153RbsiwYw/Ui4IcVksckI/AAAAAAAAAW8/qbxeHT8jrxI/s1600/RESTFUL.jpg" /></a>

###REST and RESTful
> <a href="http://roy.gbiv.com/untangled/2008/rest-apis-must-be-hypertext-driven" target="_blank">Developed by Roy Fielding</a>, Representational State Transfer (or REST) is a style of software architecture for distributed systems, such as the Web, and it</span> relies on a stateless, client-server, cache-able communications protocols, like HTTP.<

####/this/does/not/mean/restful, { "neither":"does this" }
Having an API with clean  fluent routes and that returns <a href="http://en.wikipedia.org/wiki/JSON" target="_blank">JSON </a>does not necessarily make your service RESTful. Only when adopting REST as a style and conforming to certain rules will make a service truly RESTful.

<a href="http://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm" target="_blank">Fielding outlines six constraints</a> that are required for REST. At least 5 of the constraints need to be respected to have a service be considered as RESTful:

#####1. Client-Server constraint
Clients and servers are separated by an uniform interface which allows for a client not to be concerned with business logic and data storage, and likewise, a server is not concerned with the user interface.

#####2. Stateless constraint
Each request made to the server contains all the information needed by the server to service the request, thus no state is stored on the server.
	
#####3. Cache-able constraint
Responses must be able to, explicitly or implicitly, define themselves as cache-able, to allow the client to re-use responses, eliminating round-trips to the server, thus improving performance.

#####4. Layered System constraint
The client is oblivious to the use of intermediary components, such as load balancers and proxies, improving system scalability.

#####5. Uniform Interface constraint
This is the most unique feature of REST and allows interaction between client and server in a uniform way by identifying resources from the request URI and modifying them using <a href="http://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol" target="_blank">HTTP </a>verbs. As mentioned earlier, each message contains enough information on how to process the request and uses <a href="http://en.wikipedia.org/wiki/HATEOAS" target="_blank">HATEOAS</a> as the engine for application state.

#####6. Code On Demand constraint
This optional constraint allows a client to not know how to process the resource until it is retrieved and locally executed. This is typically used to dynamically add features to deployed applications.
	
###RESTful Web APIs
Web-based application programming interfaces implementing REST and HTTP principles are known as RESTful Web APIs and contains the following properties:

- A base URI, for example http://api.mysystem.com
- A supported internet media type, like JSON<
- Operations using <a href="http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html" target="_blank">HTTP verbs</a> `GET`, `POST`, `PUT` and `DELETE`
- Hypertext driven

A RESTful Web API must be simple and easy to use and must provide useful documention. 

> *"If you have to ship an SDK for your RESTful API, it is not a RESTful API"* - <a href="http://amundsen.com/blog/archives/1046" target="_blank">source</a>

The <a href="http://asp.net/">ASP.NET</a> team developed a framework for building Web APIs based on REST principles, called ASP.NET Web API. <a href="http://asp.net/web-api" target="_blank">You can read more on this here</a>.

###That's a wrap
The constraints described above are not considered a standard, but rather as best practice guidelines to follow, in order to build rich RESTful services for the modern web.

There are only a handful of APIs that are truly RESTful, the majority are really modified RCP over HTTP. So next time you explore a new API, feel free to compare notes and find out if it is a true RESTful service.