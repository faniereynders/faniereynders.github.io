---
layout: post
title: An update on WebApiProxy
date: '2015-01-24T06:05:00.001+02:00'
author: Fanie Reynders
tags:
- Web Api Proxy
- ASP.NET Web API
- WebApiProxy
- Xamarin
- nuget
- C#
- Silverlight
- Powershell
- MEX
modified_time: '2015-01-24T11:58:25.749+02:00'
thumbnail: http://3.bp.blogspot.com/-ENCgDrDKKwA/VMMZYdmnlEI/AAAAAAAAAW8/IcxeqmaLAio/s72-c/project.png
blogger_id: tag:blogger.com,1999:blog-2911174713310708108.post-7917398119189776265
blogger_orig_url: http://www.faniereynders.com/2015/01/an-update-on-webapiproxy.html
---

This project [initially started as a proof of concept]({% post_url 2013-03-10-creating-a-proxy-for-aspnet-web-api-t4 %}) but quickly grew into something very useful. For the past year or so it became quite popular and plenty of bugs came out of the cracks, but also useful feedback for features too!

<!--more-->

You can download the packages straight from NuGet: <a href="https://www.nuget.org/packages/WebApiProxy/" target="_blank">WebApiProxy Provider</a> and <a href="https://www.nuget.org/packages/WebApiProxy.CSharp/" target="_blank">Web API C# Proxy Generator</a>

###Release notes
Here are a quick summary of issues resolved and features introduced in the latest version:

####WebApiProxy Provider (version 1.0.2.4)

- <a href="http://owin.org/" target="_blank">OWIN</a> support and compatible with self-hosted Web APIs
- Custom MEX endpoint address
- Enums and Constants support in DTOs
- Support for <a href="https://msdn.microsoft.com/en-us/library/system.web.http.ihttpactionresult(v=vs.118).aspx" target="_blank">IHttpActionResult</a>
- Extended documentation on DTOs
- Plenty of bug fixes & refactoring

####WebApiProxy C# Generator (version 1.0.3.21)

- <a href="https://msdn.microsoft.com/en-us/library/gg597391%28v=vs.110%29.aspx" target="_blank">PCL</a> compatible
- Generated code now contained inside project
- Opt-in for automatic code generation during build (now disabled by default)
- On-demand code generation using <a href="http://docs.nuget.org/docs/reference/package-manager-console-powershell-reference" target="_blank">NuGet Package Manager Console</a>
- No more reloading project for unresolved types
- Cleaner config file

> In case you've missed it, please feel free to read more on WebApiProxy and how to get started with it on [this introductory blog post]({% post_url 2014-01-16-introducing-webapiproxy-providing %})

###OWIN support and compatible with self-hosted Web APIs
Thanks to great community feedback and contributions, WebApiProxy is now supported on Web APIs hosted on OWIN. Unlike the previous release, where the service meta-data endpoint <i>just worked</i> by automatically registering on start-up, it has now changed to a manual registration that one needs to make in order for it to activate. This was implemented to be more in line with the upcoming <a href="http://www.asp.net/vnext" target="_blank">ASP.NET 5 (vNext)</a> style of ad-hoc opt-in model. Another great thing is that there is no dependency on System.Web anymore! 

To register the proxy engine for activation, simply add the following line to the Register function inside the WebApiConfig class:

```csharp
config.RegisterProxyRoutes();
```

###Custom MEX endpoint address
In the previous release the metadata exchange endpoint address was fixed as `/api/proxies` but is now changeable by passing your own custom route into the `RegisterProxyRoutes()` function:

```csharp
config.RegisterProxyRoutes("/my/custom/$proxy");
```

###Enums and Constants support in DTOs
It is now possible to use Enums and Constants within your data transfer objects as this will be serialized and available for generation on the metadata document

###Support for `IHttpActionResult`
The recent release includes support for abstracted HTTP response messages with <i>IHttpActionResult</i> as the return type of the actions. You can tell WebApiProxy what response type is expected by decorating the actions with the `ResponseType` attribute:

```csharp
[ResponseType(typeof(Person))]
public IHttpActionResult Get(int id)
{
	return Ok(person);
}
```

###Extended documentation on DTOs
Any documentation annotated on and inside your data transfer objects, will also now be available on the metadata for a richer experience in C# clients.

###Plenty of bug fixes & refactoring
WebApiProxy went through a major refactoring process and most crucial bugs were fixed during this time, like duplicate model resolving that caused the endpoint to break.

###PCL compatible
I am proud to announce that the C# client generator is now completely compatible with Portable Class Libraries and works on Windows Desktop, Silverlight 5, Windows 8, Windows Phone 8.1, Universal Apps, Xamarin.iOS and Xamarin.Android.

###More control and less <i>magic</i>
The C# client-side proxy generator initially worked on a build task triggering a call to a remote service for metadata to generate client side code every time. This code was magically part of the project assembly and not easily viewable. Issues relating to remote isolated build servers were also reported. Luckily this has changed. Now, upon installation of the package, an empty class is added to the project that gets regenerated on your terms.

A new attribute in the configuration file is introduced to allow one to explicitly opt-in for automatic code generation on every build. This function is turned off by default and can be easily activated by setting the `generateOnBuild` attribute to <i>true.</i>

###On-demand code generation using NuGet Package Manager Console
It is now possible to utilize the NuGet Package Manage Console for generating client side code on demand by using the `WebApiProxy-Generate-CSharp` Powershell commandlet straight from within Visual Studio.

###Other tweaks
Gone are the days where one needed to unload and reload the project after installing the client side package as it now contains everything it needs to generate the proxy. In the new version, you will find that things have moved a bit.

<a href="http://3.bp.blogspot.com/-ENCgDrDKKwA/VMMZYdmnlEI/AAAAAAAAAW8/IcxeqmaLAio/s1600/project.png"><img border="0" src="http://3.bp.blogspot.com/-ENCgDrDKKwA/VMMZYdmnlEI/AAAAAAAAAW8/IcxeqmaLAio/s1600/project.png" /></a>

All the WebApiProxy specifics are now neatly tucked away under a folder called <i>WebApiProxy</i> that contains the generated code and the configuration file, which also got a bit of a make-over as it now only requires the MEX endpoint URI of the service.