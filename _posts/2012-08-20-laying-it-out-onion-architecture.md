---
layout: post
title: 'Laying it out: Onion Architecture'
date: '2012-08-20T15:15:00.000+02:00'
author: Fanie Reynders
tags:
- onion architecture
- domain driven design
- design patterns
- test driven design
modified_time: '2012-11-19T16:16:08.868+02:00'
thumbnail: http://3.bp.blogspot.com/-Z79OTGhMQNY/UDJIu7SwGXI/AAAAAAAAAPE/3U_KIGDV9YI/s72-c/Traditional.png
blogger_id: tag:blogger.com,1999:blog-2911174713310708108.post-5342880208211715338
blogger_orig_url: http://www.faniereynders.com/2012/08/laying-it-out-onion-architecture.html
---

It's a fact, there is no <i>Single Silver Bullet</i> for every solution, but we can come damn close. Having the right architecture sets a solid foundation for any solution. When I say "<i>architecture</i>", I mean structuring the solution in a specific strategic way; with the aim to have the components (or layers) loosely coupled and tightly cohesive to the maximum extent possible keeping in mind the feasibility of the trade-off.

<!--more-->

One can implement different design patterns on top of an architecture pattern to standardise the usability of the code for a specific solution. Building solutions depends on the applicable technology and the following questions come to mind: Is it available? How often does it change? Is it supported?

I want to emphasise one particular question - <i>How often does it [the technology] change?</i> Baring in mind the complexity of the solution; there's a big chance of changing technology. In this context, having the words "change" and "complex" in one sentence raises warning signs that change is eventually inevitable for the dependent technology.

###Comfortably embrace change - be ready
We must strive to have maintainable and extensible components in our solution. This is where the term "Onion Architecture" comes into play. It emphasizes separation of concerns throughout the system.

I must stress that this type of architecture is best suited for complex behaviour and long-lived business applications. It may not be feasible for small websites.<br /><br />Onion Architecture emphasizes the use of Contracts (Interfaces) and forces externalization of the implementation thereof. This gives the ability to freely interchange different implementations (that may be dependent on different types of technology) in our system.

###Traditional Layered Architecture
<a href="http://3.bp.blogspot.com/-Z79OTGhMQNY/UDJIu7SwGXI/AAAAAAAAAPE/3U_KIGDV9YI/s1600/Traditional.png" imageanchor="1"><img border="0" height="182" src="http://3.bp.blogspot.com/-Z79OTGhMQNY/UDJIu7SwGXI/AAAAAAAAAPE/3U_KIGDV9YI/s320/Traditional.png" width="320" /></a>
The Traditional Layered Architecture (Flattened)

The problem with the "traditional" <i>N-tier</i> architectural layered design in this context is that the upper layers are dependent on the layers beneath as well as the possible "shared" layer (Infrastructure), making the very bottom layer highly dependable.

In this case, any changes in the "Storage" layer can easily cause a ripple-effect to the other layers.

###Problems with Traditional Architecture
- Very easy for developers, over time to put more and more business logic in the UI layer 
- Counter-productive to build your application on top of a specific technology that is sure to change over time 
- Logic is easily scattered all over, locating code becomes a major effort. 
- Developers over time struggle to determine where code should go… DAL? BLL? Utilities?
- Business logic has many tentacles extending from it (directly and indirectly)
- Library explosion: Makes it easy take a dependency without putting much thought into it, and now it’s littered all over the code base

This problem can be solved of course using a <a href="http://en.wikipedia.org/wiki/Domain-driven_designdUjP8fJmXUmMgx1bH4g">Domain Driven Design (DDD)</a> pattern that make use of Interfaces to act as an <a href="http://stackoverflow.com/questions/909264/ddd-anti-corruption-layer-how-to">Anti-Corruption Layer (ACL)</a>.

<a href="http://4.bp.blogspot.com/-WSqbQRVooik/UDJIyjcPHTI/AAAAAAAAAPc/DmCmsYZrSQU/s1600/Traditional+Wrong+Onion.png" imageanchor="1"><img border="0" height="320" src="http://4.bp.blogspot.com/-WSqbQRVooik/UDJIyjcPHTI/AAAAAAAAAPc/DmCmsYZrSQU/s320/Traditional+Wrong+Onion.png" width="320" /></a>

The illustration on the right shows a similar architecture: Both the User Interface (Presentation-layer) &amp; Tests on the outside reference the Business Logic that references the Data Access Layer that talks to the persistant storage.

> Note that references work from the outside towards the centre.

Even with an ACL (represented by the white spaces), bending the layout in a circle we discover something significant: The Storage-layer (database) is in the centre.

<blockquote class="twitter-tweet tw-align-center">The center of your application is not the database, it's the use cases of your application.<br />— Fanie Reynders (@FanieReynders) <a data-datetime="2012-05-16T19:23:49+00:00" href="https://twitter.com/FanieReynders/status/202841804004012034">May 16, 2012</a></blockquote>

###Wait, what?
Looking back at one of my <a href="https://twitter.com/FanieReynders/status/202841804004012034">previous Twitter quotes</a>, I remembered my philosophical moment: The database should not be the centre concern of our application. <i>Why </i>should the business case dictate &amp; worry about <i>where </i>and <i>how </i>the data will be stored? Business [logic] should never be dependent on the database.

###Bring on the Onion
Derived from the onion's circular layered structure, Onion Architecture isn't a new concept. It's been around for a while now. It allows the use of pluggable components that is the external implementations of the internal contracts & models. 

It relies heavily on the <a href="http://en.wikipedia.org/wiki/Dependency_inversion_principle">Dependency Inversion Principle (DIP)</a> for configuring bindings and injecting the instances toward the implementation (on the edges) of the application during run-time.

<a href="http://4.bp.blogspot.com/-b4L9u8dyxgU/UDJIxbJt89I/AAAAAAAAAPU/JED0ustIIuM/s1600/Overview.png" imageanchor="1"><img border="0" height="316" src="http://4.bp.blogspot.com/-b4L9u8dyxgU/UDJIxbJt89I/AAAAAAAAAPU/JED0ustIIuM/s320/Overview.png" width="320" /></a>

Starting from the outside, we have a Dependency Resolution layer responsible for providing instances to contracts using the DIP like <a href="http://en.wikipedia.org/wiki/Inversion_of_control">Inversion of Control (IoC)</a> and references all the layers.

- The UI layer represents the front-end logic of the application.
- Infrastructure is externalized and provides the implementations responsible for invoking web-services, data access, logging, mapping etc. Only technology-specific code (non-business) belongs here.
- The Tests layer houses all tests in the solution.
- All externally exposed functionality (like REST & WCF) is implemented in the API Service layer.
- The UI-, Infrastructure-, Tests- and API Service layer respectfully references the contracts and models in the Core layer (represented by the white line).

The Business Logic, or the Core layer; is in the centre. Here you will find everything unique to the business: Domain model, validation rules and business workflows. This layer cannot reference any external libraries and contains no technology-specific code.

Here is a graphical representation in more detail:
<a href="http://2.bp.blogspot.com/-oLD-EX0gJn8/UDJIwLb_uII/AAAAAAAAAPM/e-3YWqxeyB4/s1600/Detail.png" imageanchor="1"><img border="0" height="273" src="http://2.bp.blogspot.com/-oLD-EX0gJn8/UDJIwLb_uII/AAAAAAAAAPM/e-3YWqxeyB4/s640/Detail.png" width="640" /></a>
###Key aspects of Onion Architecture:
- The application is built around an independent object model
- Inner layers define interfaces. Outer layers implement interfaces
- Direction of coupling is toward the centre
- All application core code can be compiled and run separate from infrastructure

###Benefits
- True loose coupling between layers/components
- Limit re-write necessity over time, business value-adding logic is entirely self-contained 
- Application becomes very portable – next version of .NET or an entirely new language/platform 
- Business logic has no dependency tentacles (aside from your platform dependencies) 
- Architecture is more easily sustained over time, developers know exactly where components go, a lot of guesswork is removed 
- Infrastructure is free to use any data access library or external web services to do its work 
- UI and Data Access “layers” become much smaller, deal strictly with technology-related code 
- No more need for Common/Shared/Utilities project 
- Compiler enforces dependencies boundaries, impossible for Core to reference Infrastructure

<i>That's all folks!</i> Hopefully someone will find this interesting and especially useful somewhere in their solution implementations. 

> Remember to draw the system using concentric circles which can only take dependency on something provided in an inner layer. Externalise all technology-related code and push complexity as far out as possible.

References: <a href="http://www.matthidinger.com/archive/2011/05/17/Onion-Architecture-code-and-slides-from-Chicago-Code-Camp.aspx">Matt Hidinger</a>, <a href="http://jeffreypalermo.com/blog/the-onion-architecture-part-1/">Jeffrey Palermo</a>

Till next time...