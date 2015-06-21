---
layout: post
title: Some API "best suggestions"
date: '2013-02-17T18:09:00.003+02:00'
author: Fanie Reynders
tags:
- rest
- api
- webapi
modified_time: '2013-02-17T18:16:04.559+02:00'
thumbnail: http://4.bp.blogspot.com/-md5_3TGkVKE/USD5huk0YnI/AAAAAAAAARI/qI2QQSpxRRY/s72-c/push.png
blogger_id: tag:blogger.com,1999:blog-2911174713310708108.post-2720948169129580399
blogger_orig_url: http://www.faniereynders.com/2013/02/some-api-best-suggestions.html
---

In today’s modern world of computing it’s a no-brainer to expose some kind of service for the outside world to access your enterprise platform in a controlled fashion. Because of good old legacy systems, we all know that *“the perfect world”* never exists in any of our clients’ current environment. There are always compatibility issues regarding unified communication between these systems and we end up building all kinds of middle layers to facilitate this interaction. The latter makes it a nightmare to implement any kind of external service.

By building a platform with a collection of loosely coupled components and exposing specific functionality via an unified interface is much better than having all kinds of *silo systems* talking to each other. We can achieve this by implementing an API over <a href="http://en.wikipedia.org/wiki/Representational_state_transfer">REST (Representational state transfer)</a>. Here are some guidelines for doing just that.

<!--more-->

###API by definition
An application programming interface (API) is a protocol intended to be used as an interface by software components to communicate with each other. The API design should be approached from the “outside-in” perspective, i.e. Start by asking “<em>What are we trying to achieve with this API</em>?”

###The developer is the user
Unlike normal systems, whereby you have end-users using the different functionality of a system, the developers, in this case, <em>are</em> the users of the API. The developer is the lynchpin of the entire strategy and the primary design principle when crafting your API should be to maximize the developer productivity and success. This is <em>pragmatic REST</em>.

It is extremely important to get the design right, because design communicates how something will be used. Your API should guide the developer on the best possible way to achieve a certain task. To do this, we need to use a collection of standard best practice guidelines.

####Use nouns, not verbs
Before I continue, it is important to remember to<strong> keep things simple</strong>. Your base URL should be simple and intuitive. This is the number <strong>one principle</strong> for ease of use.

> Affordance is a design property that communicates how something should be used without requiring documentation.  

A door handle's design should communicate whether you pull or push. Here's an example of a conflict between design affordance and documentation:

<a href="http://4.bp.blogspot.com/-md5_3TGkVKE/USD5huk0YnI/AAAAAAAAARI/qI2QQSpxRRY/s1600/push.png" imageanchor="1" ><img border="0" height="254" src="http://4.bp.blogspot.com/-md5_3TGkVKE/USD5huk0YnI/AAAAAAAAARI/qI2QQSpxRRY/s320/push.png" width="320" /></a>

A key practice in Web API design is that there should be <strong>only 2 base URLs per resource</strong>. 

For example, if we model an API around a simple object, like a car, we would have the following (the first URL is for a collection; the second is for a specific element in the collection

```
/cars
/cars/2615
```

> *Keep verbs out of your base URLs* - for any resource that you model, like the car, you can never consider one object in isolation. Rather, there are always related and interacting resources to account for.

####Use verbs to operate on the collection and elements
For our car resources, we have 2 base URLs and can operate them with HTTP verbs: `GET`, `PUT` and `DELETE`. This maps to the acronym, <strong>CRUD</strong> (<strong>C</strong>reate, <strong>R</strong>ead, <strong>U</strong>pdate, <strong>D</strong>elete).

Given our 2 resources and 4 verbs, it result in a rich set of capability that’s intuitive for the developer:


<table border="1" cellpadding="0" cellspacing="0" class="MsoTableGrid" style="border-collapse: collapse; border: currentColor; mso-border-alt: solid windowtext .5pt; mso-padding-alt: 0cm 5.4pt 0cm 5.4pt; mso-yfti-tbllook: 1184;"> <tbody><tr style="mso-yfti-firstrow: yes; mso-yfti-irow: 0;">  <td style="background-color: transparent; border: 1pt solid windowtext; mso-border-alt: solid windowtext .5pt; padding: 0cm 5.4pt; width: 95.75pt;" valign="top" width="128"><div class="MsoNormal" style="line-height: normal; margin: 0cm 0cm 0pt;"><strong><span style="font-family: inherit;">Resource</span></strong></div></td><td style="background-color: transparent; border-color: windowtext windowtext windowtext rgb(0, 0, 0); border-style: solid solid solid none; border-width: 1pt 1pt 1pt 0px; mso-border-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; padding: 0cm 5.4pt; width: 95.75pt;" valign="top" width="128"><strong><span style="font-family: inherit;">&nbsp;POST </span></strong><br /><strong><span style="font-family: inherit;">  </span></strong></td><td style="background-color: transparent; border-color: windowtext windowtext windowtext rgb(0, 0, 0); border-style: solid solid solid none; border-width: 1pt 1pt 1pt 0px; mso-border-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; padding: 0cm 5.4pt; width: 95.75pt;" valign="top" width="128"><strong><span style="font-family: inherit;"> GET</span></strong><br /><strong><span style="font-family: inherit;">  </span></strong></td><td style="background-color: transparent; border-color: windowtext windowtext windowtext rgb(0, 0, 0); border-style: solid solid solid none; border-width: 1pt 1pt 1pt 0px; mso-border-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; padding: 0cm 5.4pt; width: 95.75pt;" valign="top" width="128"><strong><span style="font-family: inherit;">&nbsp;PUT</span></strong><br /><strong><span style="font-family: inherit;">  </span></strong></td><td style="background-color: transparent; border-color: windowtext windowtext windowtext rgb(0, 0, 0); border-style: solid solid solid none; border-width: 1pt 1pt 1pt 0px; mso-border-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; padding: 0cm 5.4pt; width: 95.8pt;" valign="top" width="128"><strong><span style="font-family: inherit;">&nbsp;DELETE</span></strong></td></tr><tr style="mso-yfti-irow: 1;"><td style="background-color: transparent; border-color: rgb(0, 0, 0) windowtext windowtext; border-style: none solid solid; border-width: 0px 1pt 1pt; mso-border-alt: solid windowtext .5pt; mso-border-top-alt: solid windowtext .5pt; padding: 0cm 5.4pt; width: 95.75pt;" valign="top" width="128"><br /><div class="MsoNormal" style="line-height: normal; margin: 0cm 0cm 0pt;"><span style="font-family: &quot;Courier New&quot;, Courier, monospace;">/cars</span></div><span style="font-family: inherit;">  </span></td><td style="background-color: transparent; border-color: rgb(0, 0, 0) windowtext windowtext rgb(0, 0, 0); border-style: none solid solid none; border-width: 0px 1pt 1pt 0px; mso-border-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-top-alt: solid windowtext .5pt; padding: 0cm 5.4pt; width: 95.75pt;" valign="top" width="128"><span style="font-family: inherit;">  </span><br /><div class="MsoNormal" style="line-height: normal; margin: 0cm 0cm 0pt;"><span style="font-family: inherit;">Create a new car</span></div><span style="font-family: inherit;">  </span></td><td style="background-color: transparent; border-color: rgb(0, 0, 0) windowtext windowtext rgb(0, 0, 0); border-style: none solid solid none; border-width: 0px 1pt 1pt 0px; mso-border-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-top-alt: solid windowtext .5pt; padding: 0cm 5.4pt; width: 95.75pt;" valign="top" width="128"><span style="font-family: inherit;">  </span><br /><div class="MsoNormal" style="line-height: normal; margin: 0cm 0cm 0pt;"><span style="font-family: inherit;">List cars</span></div><span style="font-family: inherit;">  </span></td><td style="background-color: transparent; border-color: rgb(0, 0, 0) windowtext windowtext rgb(0, 0, 0); border-style: none solid solid none; border-width: 0px 1pt 1pt 0px; mso-border-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-top-alt: solid windowtext .5pt; padding: 0cm 5.4pt; width: 95.75pt;" valign="top" width="128"><span style="font-family: inherit;">  </span><br /><div class="MsoNormal" style="line-height: normal; margin: 0cm 0cm 0pt;"><span style="font-family: inherit;">Bulk update cars</span></div><span style="font-family: inherit;">  </span></td><td style="background-color: transparent; border-color: rgb(0, 0, 0) windowtext windowtext rgb(0, 0, 0); border-style: none solid solid none; border-width: 0px 1pt 1pt 0px; mso-border-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-top-alt: solid windowtext .5pt; padding: 0cm 5.4pt; width: 95.8pt;" valign="top" width="128"><span style="font-family: inherit;">  </span><br /><div class="MsoNormal" style="line-height: normal; margin: 0cm 0cm 0pt;"><span style="font-family: inherit;">Delete all cars</span></div><span style="font-family: inherit;">  </span></td></tr><tr style="mso-yfti-irow: 2; mso-yfti-lastrow: yes;"><td style="background-color: transparent; border-color: rgb(0, 0, 0) windowtext windowtext; border-style: none solid solid; border-width: 0px 1pt 1pt; mso-border-alt: solid windowtext .5pt; mso-border-top-alt: solid windowtext .5pt; padding: 0cm 5.4pt; width: 95.75pt;" valign="top" width="128"><span style="font-family: inherit;">  </span><br /><div class="MsoNormal" style="line-height: normal; margin: 0cm 0cm 0pt;"><span style="font-family: &quot;Courier New&quot;, Courier, monospace;">/cars/2615</span></div><span style="font-family: inherit;">  </span></td><td style="background-color: transparent; border-color: rgb(0, 0, 0) windowtext windowtext rgb(0, 0, 0); border-style: none solid solid none; border-width: 0px 1pt 1pt 0px; mso-border-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-top-alt: solid windowtext .5pt; padding: 0cm 5.4pt; width: 95.75pt;" valign="top" width="128"><span style="font-family: inherit;">  </span><br /><div class="MsoNormal" style="line-height: normal; margin: 0cm 0cm 0pt;"><span style="font-family: inherit;">Error</span></div><span style="font-family: inherit;">  </span></td><td style="background-color: transparent; border-color: rgb(0, 0, 0) windowtext windowtext rgb(0, 0, 0); border-style: none solid solid none; border-width: 0px 1pt 1pt 0px; mso-border-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-top-alt: solid windowtext .5pt; padding: 0cm 5.4pt; width: 95.75pt;" valign="top" width="128"><span style="font-family: inherit;">  </span><br /><div class="MsoNormal" style="line-height: normal; margin: 0cm 0cm 0pt;"><span style="font-family: inherit;">Show car 2615</span></div><span style="font-family: inherit;">  </span></td><td style="background-color: transparent; border-color: rgb(0, 0, 0) windowtext windowtext rgb(0, 0, 0); border-style: none solid solid none; border-width: 0px 1pt 1pt 0px; mso-border-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-top-alt: solid windowtext .5pt; padding: 0cm 5.4pt; width: 95.75pt;" valign="top" width="128"><span style="font-family: inherit;">  </span><br /><div class="MsoNormal" style="line-height: normal; margin: 0cm 0cm 0pt;"><span style="font-family: inherit;">Update car 2615, if not exist, error</span></div><span style="font-family: inherit;">  </span></td><td style="background-color: transparent; border-color: rgb(0, 0, 0) windowtext windowtext rgb(0, 0, 0); border-style: none solid solid none; border-width: 0px 1pt 1pt 0px; mso-border-alt: solid windowtext .5pt; mso-border-left-alt: solid windowtext .5pt; mso-border-top-alt: solid windowtext .5pt; padding: 0cm 5.4pt; width: 95.8pt;" valign="top" width="128"><span style="font-family: inherit;">  </span><br /><div class="MsoNormal" style="line-height: normal; margin: 0cm 0cm 0pt;"><span style="font-family: inherit;">Delete car 2615</span></div></td></tr></tbody></table>

####Plural nouns and concrete names
The most requests in a *RESTful* API is a `GET` in which it makes sense to use plural nouns. By this I am not dictating to use plurals, but to be consistent in your model. Avoid mixed cases where you use plural for one resource and singular for another. Being consistent allows developers to guess and predict the method calls as they learn to work with your API.<

Concrete names are better than abstracts. Achieving pure abstraction is sometimes the goal of API architects, but it seldom means anything meaningful to developers.

The level of abstraction is depending on your scenario, but yet again, remember to keep it simple and intuitive. Aim to keep number of resources between 12 and 24.

####Simplify associations
Resources almost always have relationships with other resources, this makes simple associations tricky. Given the following example of getting the drivers of a specific car:

```
GET /cars/2615/drivers
```
Nice and simple, isn’t it? Now let’s extend the query with drivers of a specific car from JHB and had their license since 2004:

```
GET /cars/2615/drivers/jhb/2004
```

See where I am going with this? Already it starts to get messy. The solution is simple: <strong>Sweep complexity under the ‘?’</strong>. Make it simple for developers to use the base URL by putting optional filters behind the HTTP question mark:

```
GET /cars/2615/drivers?from=jhb&since=2004

```

Keep the levels shallow – limit routes to `/resource/identifier/resource`.

####Handling errors
It’s a fact, we as developers don’t like to think about errors and exception handling but it’s an important piece of the puzzle for any software developer; especially for API designers.

<a href="http://en.wikipedia.org/wiki/Test_Driven_Development">Test Driven Development (TDD)</a> is a common practice today and developers depend on proper error messages that makes sense to guide them through the support process to resolve critical issues.

As a rule of thumb, try to utilize as many standard HTTP status codes that you can, but not all of them. Group them logically into existing ones that will&nbsp;still makes sense.

It all comes down to <em>3 outcomes</em>:
- Everything worked – success (`200 OK`)
- The application did something wrong – client error (`400 Bad Request`)
- The API did something wrong – server error (`500 Internal Server Error`)

Start at 3, if you need more, add them, but don’t go more than 8. Also include user-friendly messages in the response for the mere mortals out there. It comes highly recommended to include a link to the description to additional information:

```
401 – Unauthorized
{
       Message: “The API key provided is not valid”,
       ErrorId: 273343,
       MoreInfo: “http://dev.someapi.net/errors/273343
}
```

####Versioning
Never release your API without versioning. This is one of the most important considerations when designing the API.

Specify the version with a "v" prefix and move it all the way to the left so it has the highest scope:

```
/v1/cars/221
```

Stick with simple ordinal numbers. Do not use dot-notation v1.2 as it implies a granularity of versioning that doesn’t work the APIs – <em>it’s an interface, not an implementation!</em>

> Try to maintain at least one version back and give developers at least <em>one cycle to react</em> before obsoleting a version.

Now for the question: <em>Should the version and format be in the URLs or the header?</em> 

Although using headers are more correct, because it leverages existing HTTP standards, I believe one should consider the following workflow:
- If it changes the logic you write to handle the response, put it in the URL so you can see it easily.
- If it doesn't change the logic for each response, like OAuth information, put it in the header.

These for example all represent the same resource, but the code we write might be very different:

```
Content-Type: application/json
GET /dogs/1

Content-Type: application/xml
GET /dogs/1
```

####Partial responses
Give developers just the right amount of information they need and let them control what gets delivered to them. For example, if the developer wants a list of car names, you could have something like:

```
/cars?fields=id,name
```

It doesn't make sense giving him the other information as it will not be used within the current use case. For performance purposes, make it easy for developers to page elements in a collection – use <strong>limit and offsets</strong>:

```
/cars?limit=10&offset=4
```

Allow developers to get partial responses by default by assigning default values. For pagination, use a default for limit = 10 and offset 0, for example.

> *Provide useful meta-data and have defaults* – It is always good to provide meta-data like created date or record count. 

#####Some recommendations regarding responses
- Support<strong> multiple formats</strong> - Use JSON as default
- Follow JavaScript naming conventions

#####Responses that don’t involve resources
In cases where you need to do calculations or do conversions, <strong>use verbs and not nouns</strong>. Because the requested resource like “<em>Calculate</em>” doesn't really return a projected collection out of a database but a result of a calculation, it makes more sense to use verbs in this cases.

####What about Search?
While a simple search could be&nbsp;modelled&nbsp;as a resourceful API (like `/cars/?q=bmw`), a more complex search across multiple resources requires a different design.

- Scoped search: `/cars/2231/drivers?q=Fanie`
- Global search: `/search?q=Fanie`

####Consolidate your API under one sub-domain
It’s cleaner, easier and more intuitive for developers to build cool apps using your API. Try keeping your API apart from your documentation:
- http://api.somesite.net
- http://developers.somesite.net

Consider also providing a SDK for major languages to allow developers to better use your API.

####Authentication
There are many ways to do authentication, it all depends on how secure you want your API to be as well as what information you categorize as sensitive.

> When you need to authenticate, <strong>NEVER pass passwords through the wire</strong> in plain text. Rather hash the password and compare the hash on the server with the service-generated hash (over SSL of course).

It is a good practice to use OAuth – <a href="http://oauth.net/">read more here</a>. 

Consumers of your API need to register their application first to use the API. Upon registration, the application gets assigned a unique <em>App ID</em> and <em>App Secret</em>.

The App Secret needs to be kept safe and must never be transferred over the wire as is, rather one-way hashing it using <a href="http://en.wikipedia.org/wiki/HMAC">HMAC</a> or something before sending it together with the App ID to the server.

When the service gets the request, it looks up the App ID and gets the applicable App Secret. This gets hashed using the same method than the client. 

The service compares these 2 hashes and if they match, the consumer is verified. This is called <a href="http://en.wikipedia.org/wiki/Claims-based_identity">claims-based authentication</a>.

Using tokens instead of encrypted passwords are less sensitive and allows the API team to revoke access to a specific consumer.

###Conclusion
Designing APIs are fun, but if done incorrectly they might come back to haunt you at a later stage. Having a flexible and intuitive API will attract many developers and as a result maximize publicity for your service, or; it will save you a lot of time developing all sorts of funny requests from outsiders interested to integrate to your environment.

Happy coding!

Follow me on Twitter - <a href="http://twitter.com/faniereynders">@FanieReynders</a>

References: <a href="http://apigee.com/">http://apigee.com</a>
