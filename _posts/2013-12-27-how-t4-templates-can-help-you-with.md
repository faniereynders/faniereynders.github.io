---
layout: post
title: How T4 templates can help you with maintainable multi-page apps in JQM
date: '2013-12-27T15:37:00.001+02:00'
author: Fanie Reynders
tags:
- T4
- PhoneGap
- JQuery Mobile
modified_time: '2013-12-31T10:30:41.936+02:00'
thumbnail: http://1.bp.blogspot.com/-3wzEs1wHdZs/Ur2pYWGQz6I/AAAAAAAAAcw/rAMeV82fHn4/s72-c/t4%255B4%255D
blogger_id: tag:blogger.com,1999:blog-2911174713310708108.post-7831988718138464292
blogger_orig_url: http://www.faniereynders.com/2013/12/how-t4-templates-can-help-you-with.html
---

We can utilize the power of T4 templates to generate a single HTML page which comprises of multiple separate HTML ‘children’ pages and a layout page, making multi-page apps more maintainable.

<!--more-->

<a href="http://1.bp.blogspot.com/-3wzEs1wHdZs/Ur2pYWGQz6I/AAAAAAAAAcw/rAMeV82fHn4/s1600/t4%255B4%255D" imageanchor="1"><img border="0" height="193" src="http://1.bp.blogspot.com/-3wzEs1wHdZs/Ur2pYWGQz6I/AAAAAAAAAcw/rAMeV82fHn4/s200/t4%255B4%255D" width="200" /></a>

I’ve been playing around with the new <a href="http://jquerymobile.com/download/" target="_blank">recent release of JQuery Mobile</a> which brings <a href="http://jquerymobile.com/changelog/1.4.0/" target="_blank">plenty of cool features</a> to the table, so I’ve decided to build a small sample to help myself learn more about this wonderful mobile presentation framework.

During development I came across the simple choice that any developer needs to make when building apps with JQM: Single-page vs. Multi-page. Being a huge fan of the <a href="http://demos.jquerymobile.com/1.4.0/pages-multi-page/" target="_blank">multi-page template</a>, I’ve decided to go with the latter after weighing up the <a href="http://dev.chetankjain.net/2011/12/jquery-mobile-single-page-vs-multi-page.html" target="_blank">pros and cons</a> based on my scenario.<br />It wasn’t long thereafter that I realized my dilemma about the maintainability of all the “internal” pages packed together within a single html file, so I began to research ways of keep using the power of multi-pages with the benefit of having each page maintained in a separate file. 

I came across quite a few implementations, <a href="http://mrsarker.wordpress.com/2011/12/13/jquery-mobile-use-single-page-architecture-with-multiple-html-files/" target="_blank">one of which uses AJAX to load all the external pages into the DOM at run-time</a>. One can also use the <a href="http://en.wikipedia.org/wiki/ASP.NET_Razor_view_engine" target="_blank">Razor view engine</a> to render separately maintained pages into one massive page including a layout page. The problem with these approaches is that we are limited to them when using a mobile framework like <a href="http://phonegap.com/" target="_blank">PhoneGap</a>.

> When using PhoneGap as a wrapper around your app, it will behave like a native app and no longer as a web-based app, which could have an impact on some functionality.

In my situation I was using PhoneGap, so the use of AJAX to load external local pages into the DOM wasn’t possible due to the <a href="http://www.w3.org/TR/cors/" target="_blank">CORS</a> security measures and the use of the Razor view engine was also not possible due to the fact that I am using pure HTML files within a PhoneGap application.<br />After further brainstorming, I remembered [my previous post about T4 templates]({% post_url 2013-03-10-creating-a-proxy-for-aspnet-web-api-t4 %}) and suddenly had a great idea of using T4 templates for rendering a single HTML page from multiple external pages based on a layout of my choice.

###The concept
I wanted to keep things simple and stick to what I have, so the idea is to have a T4 template and an asset manifest which contains references to the external HTML and JavaScript assets as well as the layout for the single page. All the magic happens within the T4 template when it runs by rendering a single HTML page based on the configuration in the the manifest.

###Talk is cheap, show me the code</h2>The asset manifest:

```xml
<assets layout="SampleLayout.html">
  <asset html="Page1.html" js="Page1.js"/>
  <asset html="Page2.html" js="Page2.js"/>
</assets>
``` 

The T4 template:

```csharp
<#@ template hostspecific="true" #>
<#@ assembly name="System.Xml" #>
<#@ import namespace="System.Xml" #>
<#@ import namespace="System.Text" #>
<#@ import namespace="System.IO" #>
<#@ output extension=".html" #>
<#
string manifest = Host.ResolvePath("assetManifest.xml");
XmlDocument document = new XmlDocument();
document.Load(manifest);
 
string layoutFile =  document.SelectSingleNode("/assets").Attributes["layout"].Value;
string layout = File.ReadAllText(Host.ResolvePath(layoutFile));
 
StringBuilder html = new StringBuilder();
StringBuilder js = new StringBuilder();
 
foreach (XmlNode item in document.SelectNodes("/assets/asset"))
{
	html.AppendLine(String.Format("\t<!--{0}-->",item.Attributes["html"].Value));
    html.AppendLine("\t" + File.ReadAllText(Host.ResolvePath(item.Attributes["html"].Value)));
	html.AppendLine();
    js.AppendLine("\t" + String.Format(@"<script type=""text/javascript"" src=""{0}""></script>",item.Attributes["js"].Value));
 
}
 
layout = layout.Replace("<!--Html-->",html.ToString()).Replace("<!--Js-->",js.ToString());
 
#>
<#= layout #>
```

The Layout:

```html
<!DOCTYPE html>
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
    <meta name="format-detection" content="telephone=no" />
    <meta name="viewport" content="user-scalable=no, initial-scale=1, maximum-scale=1, minimum-scale=1, width=device-width, height=device-height, target-densitydpi=device-dpi" />
    <meta name="msapplication-tap-highlight" content="no" /> 
    <link rel="stylesheet" type="text/css" href="css/vendor/jquery.mobile-1.4.0.min.css" />
    <title>My App</title>
</head>
<body>
<!--Html-->
 
    <script type="text/javascript" src="cordova.js"></script>
    <script type="text/javascript" src="js/vendor/jquery.js"></script>
    <script type="text/javascript" src="js/vendor/jquery.mobile.js"></script>
<!--Js-->
 
</body>
</html>
```

The <!—Html—> and <!—Js—> comment tags will be replaced by the assets referenced in the manifest.

To generate the single page, we simply right-click on the Index.tt file and select ‘*Run Custom Tool*’

<a href="http://lh3.ggpht.com/-dl4X37pK5xY/Ur2etIblV-I/AAAAAAAAAcU/NvjXZ-vE8I8/s1600-h/run%25255B5%25255D.png"><img alt="run" border="0" height="189" src="http://lh5.ggpht.com/-pFnlSQbxLp4/Ur2et3rcmMI/AAAAAAAAAcY/rSulEoxFXug/run_thumb%25255B3%25255D.png?imgmax=800"title="run" width="244" /></a>

> Note that this is done from within Visual Studio but you can also run the template from using the <a href="http://msdn.microsoft.com/en-us/library/bb126245.aspx" target="_blank">TextTransform</a> command line utility.

The end result after running T4 tool:

```html
<!DOCTYPE html>
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
    <meta name="format-detection" content="telephone=no" />
    <meta name="viewport" content="user-scalable=no, initial-scale=1, maximum-scale=1, minimum-scale=1, width=device-width, height=device-height, target-densitydpi=device-dpi" />
    <meta name="msapplication-tap-highlight" content="no" /> 
    <link rel="stylesheet" type="text/css" href="css/vendor/jquery.mobile-1.4.0.min.css" />
    <title>My App</title>
</head>
<body>
    <!--Page1.html-->
    <div data-role="page" id="Page1">
    <div data-role="header">
        Page 1
    </div>
    <div role="main" class="ui-content">
        Hello from Page 1
    </div>
    </div>
 
    <!--Page2.html-->
    <div data-role="page" id="Page2">
    <div data-role="header">
        Page 2
    </div>
    <div role="main" class="ui-content">
        Hello from Page 2
    </div>
  </div>
 
  <script type="text/javascript" src="cordova.js"></script>
  <script type="text/javascript" src="js/vendor/jquery.js"></script>
  <script type="text/javascript" src="js/vendor/jquery.mobile.js"></script>
  <script type="text/javascript" src="Page1.js"></script>
  <script type="text/javascript" src="Page2.js"></script>
</body>
</html>
```

This solution enabled me to have the maintainability of having multiple HTML pages with the benefits of the multi-page template in JQuery Mobile.

I took the liberty of also creating a <a href="https://www.nuget.org/packages/HtmlMerge/" target="_blank">Nuget package</a> of this for anyone that might want to leverage this approach in their applications. 

You can install it from the Nuget Package Manager console like this:

```
Install-Package HtmlMerge
```

*Enjoy!*