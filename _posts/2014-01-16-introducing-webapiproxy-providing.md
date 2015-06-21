---
layout: post
title: 'Introducing WebApiProxy: Providing JavaScript & C# proxies with Intellisense
  including documentation for ASP.NET Web API'
date: '2014-01-16T15:18:00.001+02:00'
author: Fanie Reynders
tags:
- Web Api Proxy
- T4
- RESTful
- ASP.NET Web API
- webapi
modified_time: '2015-01-29T21:17:17.882+02:00'
blogger_id: tag:blogger.com,1999:blog-2911174713310708108.post-1632368194739129029
blogger_orig_url: http://www.faniereynders.com/2014/01/introducing-webapiproxy-providing.html
---

<a href="https://www.nuget.org/packages/WebApiProxy/" target="_blank">WebApiProxy</a> extends your ASP.NET Web API service with a proxy endpoint, providing a ready-to-use JavaScript client as well as metadata, exposing the types used in the service. If you are using the documentation provider from <a href="https://www.nuget.org/packages/Microsoft.AspNet.WebApi.HelpPage" target="_blank">ASP.NET Web API Help Pages</a>, the usage documentation is also included inside the metadata, giving the client-side developer a rich experience with <a href="http://en.wikipedia.org/wiki/Intelligent_code_completion" target="_blank">Intellisense</a>.
  
> This release has been updated and includes much more great features. [Read more on the update here.]({%post_url 2015-01-24-an-update-on-webapiproxy %}) 

<!--more-->

A while ago, [I wrote about the concept of using T4 templates, to generate C# client proxies for RESTful Web APIs in ASP.NET]({%post_url 2013-03-10-creating-a-proxy-for-aspnet-web-api-t4 %}); and received good feedback. I’ve received plenty of e-mails requesting more information on the official release date. At that time, there were already some super cool implementations around, like <a href="http://blog.greatrexpectations.com/2012/11/06/proxyapi-automatic-javascript-proxies-for-webapi-and-mvc/" target="_blank">ProxyApi</a>, that provides a JavaScript proxy for ASP.NET Web APIs, but no C# proxy (yet).
  
The idea of <a href="http://www.novanet.no/blog/yngve-bakken-nilsen/dates/2012/7/clean-up-your-mvc-app-with-signalr/" target="_blank">JavaScript proxy generation is inspired</a> by <a href="http://www.asp.net/signalr" target="_blank">SignalR</a> whereby the code is scanned for controllers and methods. Based on the type information, it compiles a JavaScript proxy that can be referenced from HTML. This got me thinking – why not expose the metadata as well? By doing this, it gives us the opportunity to create a client side generator, like a build task, to generate a proxy based on the metadata for communicating to a ASP.NET Web API service, in any language – like C#.
  
Currently, we have the following technologies:
  
- <a href="https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Core/5.0.0" target="_blank">ASP.NET Web API 2</a> – create RESTful services in ASP.NET
- <a href="https://www.nuget.org/packages/Microsoft.AspNet.WebApi.HelpPage" target="_blank">ASP.NET Web API 2 Help Pages</a> – providing a wiki-like platform for documentation annotated inside the code
- <a href="https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/5.0.0" title="https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/5.0.0">ASP.NET Web API Client Libraries</a> – PCL for communicating to RESTful services from any .NET platform

I’ve noticed that ASP.NET Web API 2 comes stock standard with an <a href="http://msdn.microsoft.com/en-us/library/system.web.http.description.apiexplorer(v=vs.118).aspx">ApiExplorer</a> that retrieves the type information of the service, so I built a proxy provider that installs a message handler, using the ApiExplorer to return the service metadata as the response. Also, if the documentation provider (from ASP.NET Web API 2 Help Pages) is used in your service, the documentation will also appear in the metadata response.
  
###The WebApiProxy Provider
This extension provides a proxy endpoint in your service as `/api/proxies` that serves a JavaScript client and service metadata. All you need to do is install it from NuGet:

```
Install-Package WebApiProxy
```

> Note: This package requires the core libraries of ASP.NET Web API (version 5 or higher)

Given a Person API on the server:

```csharp
public class PersonController : ApiController
{
	public Person Get(int id) {
    return new Person { Name = "Steve" }
	}
}
```
  
Allows you to use it in JavaScript like this:
  
```javascript
$.proxies.person.get(2)
  .done(function(person) {
    //use person
  });
```

Simply reference the proxy endpoint provided inside your HTML and you're good to go:
  
```html
<script src="api/proxies"></script>
```

> This functionality was adopted from <a href="https://github.com/stevegreatrex/ProxyApi" target="_blank">ProxyApi</a> - kudos to Stephen Greatrex :)

Invoke the service on its proxy endpoint with the request header `X-PROXY-TYPE` as "metadata" and the service metadata (including documentation*) will be in the response.
  
###The WebApi C# Proxy Generator

The WebApi C# Proxy Generator is a build task to seamlessly generate a client-side C# Proxy upon project build, for communicating to a ASP.NET Web API service that implements the WebApi Proxy Provider on the server.
  
```
Install-Package WebApiProxy.CSharp
```
> This package requires the libraries of ASP.NET Web API Client (version 5 or higher)

The C# proxy code will be generated every time you re-build the project and is based on specific configuration in the `WebApiProxy.config` file:

> The generated code is cached to avoid compilation errors if the service isn't reachable at that time

Now, given a PeopleController on the service-side: It can be used like this on a client like a Console Application or Windows Phone app:

> If the types are not found (or resolved) after build, simply give your project a restart or restart Visual Studio.

It even has nice Intellisense including documentation provided by the documentation provider:
  
<img src="https://github-camo.global.ssl.fastly.net/7114820a6d021a4939d145f4e0c644590f0bee72/687474703a2f2f312e62702e626c6f6773706f742e636f6d2f2d585973444e5a4c38474b6f2f55746164447843754a37492f4141414141414141416d452f70686646734a31574234632f73313630302f696e74656c6c6973656e73652e706e67" />

> The documentation on the Intellisense will only appear if the service uses the documentation provider. You can use the <a href="https://www.nuget.org/packages/Microsoft.AspNet.WebApi.HelpPage/5.0.0">ASP.NET Web API Help Page package</a> on NuGet

You can follow the project <a href="https://github.com/faniereynders/WebApiProxy">on Github here</a>

*Till next time!*