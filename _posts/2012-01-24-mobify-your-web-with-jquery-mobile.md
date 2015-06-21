---
layout: post
title: Mobify your web with JQuery Mobile
date: '2012-01-24T16:08:00.000+02:00'
author: Fanie Reynders
tags:
- jquerymobile
modified_time: '2012-11-19T16:40:42.268+02:00'
thumbnail: http://4.bp.blogspot.com/-r04Wc09JNCo/Tx5-7E3JbsI/AAAAAAAAAKg/yydwsDRIu1o/s72-c/jquerysp.jpg
blogger_id: tag:blogger.com,1999:blog-2911174713310708108.post-1853144069467555522
blogger_orig_url: http://www.faniereynders.com/2012/01/mobify-your-web-with-jquery-mobile.html
---

Let's face it, we all use our (smart) phones most of the time to browse the web. Then you get sites that aren't formatted for mobiles and you end up zooming and panning away, confusing yourself with the *"all over the show"* navigation, not to mention the teeny tiny links that makes it difficult to click - if you have toes for fingers (like me).

It's no doubt about it, we as web developers need to cater for mobile devices when we do web apps & -sites. Usually we achieve this by duplicating pages & logic in a sub-web that is formatted specifically for mobile devices, having our site *www.example.com* for PCs and *m.example.com* for mobile. This can become quite cumbersome - especially when doing site maintenance.

<!--more-->

<a href="http://4.bp.blogspot.com/-r04Wc09JNCo/Tx5-7E3JbsI/AAAAAAAAAKg/yydwsDRIu1o/s1600/jquerysp.jpg" imageanchor="1"><img border="0" src="http://4.bp.blogspot.com/-r04Wc09JNCo/Tx5-7E3JbsI/AAAAAAAAAKg/yydwsDRIu1o/s1600/jquerysp.jpg" /></a>

###Have no fear, JQuery Mobile is here!
For those that doesn't know what JQuery is, <a href="http://jquery.com/" target="_blank">go here</a> before moving on.

> "JQuery Mobile is a unified user interface system that works seamlessly across all popular mobile device platforms, built on the rock-solid JQuery and JQuery UI foundation. Focused on a lightweight code base built on progressive enhancement with a flexible, easily themeable design."

Built with HTML5 in mind, we can now leverage the super sexy UI on touch devices right on our web apps! The JQuery Mobile "page" structure is optimized to support either single pages, or local internal linked "pages" within a page.

The goal of this model is to allow developers to create websites using best practices — where ordinary links will "just work" without any special configuration — while creating a rich, native-like experience that can't be achieved with standard HTTP requests. You can read more on JQuery Mobile on <a href="http://www.jquerymobile.com/">www.jquerymobile.com</a> or get it with <a href="http://nuget.org/" target="_blank">NuGet</a>

> If you're not sure what NuGet is, [I blogged about it a while back]({% post_url 2011-11-22-jump-start-your-next-project-with-nuget %})

###Mobile View Engines to the rescue!
From the origin of the Razor view engine, things just got better. While working on an app (built with MVC & Razor), I came across a nifty Nuget package called *MobileViewEngines*

This adds all the plumbing for making your web app mobile viewable. In MVC this works great: If you have a view called "Index.chtml" for instance, just create another one called "Index.Mobile.chtml".

Everything else, will work seamlessly together - your controller will still return the "Index" view, but it will be dependant on the device if it will use the desktop or mobile version. This can also be implemented on old school web forms. Now, implement JQuery Mobile on top of this; and you have a truly winning solution. <a href="http://www.hanselman.com/blog/NuGetPackageOfTheWeek10NewMobileViewEnginesForASPNETMVC3SpeccompatibleWithASPNETMVC4.aspx" target="_blank">Here is some information on this</a>

I hope this can help anyone out there that wants to go mobile with their apps. The mobile world is bound to happen, it's not a question of&nbsp;<strong><em>if</em></strong>, it's a question of&nbsp;<strong><em>when</em></strong>. So best start now.<br /><br />May you have a blessed 2012 and become the best in what you do -&nbsp;<strong>code</strong>! :) and remember:&nbsp;<em>"Don't over do it, rather do it over"</em><br />

Follow me on Twitter&nbsp;<a href="http://twitter.com/faniereynders" target="_blank">@FanieReynders</a></div>