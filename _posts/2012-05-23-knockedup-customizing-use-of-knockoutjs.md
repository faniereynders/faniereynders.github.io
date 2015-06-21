---
layout: post
title: 'KnockedUp: Making KnockoutJs more declarative'
date: '2012-05-23T15:19:00.000+02:00'
author: Fanie Reynders
tags:
- javascript
- mvvm
- knockoutjs
modified_time: '2012-11-19T16:40:03.156+02:00'
thumbnail: http://4.bp.blogspot.com/-0RKqmvHtehw/T7y6xZJUVkI/AAAAAAAAAK8/mNSSL3LvMWQ/s72-c/homepage-example.png
blogger_id: tag:blogger.com,1999:blog-2911174713310708108.post-4622386127758735671
blogger_orig_url: http://www.faniereynders.com/2012/05/knockedup-customizing-use-of-knockoutjs.html
---

I wrote this little library to make the default KnockoutJs syntax more declarative and easier to read.

<!--more-->

[I blogged previously]({% post_url 2012-05-14-mvvm-for-web-using-knockoutjs %}) about a nifty little library called <a href="http://knockoutjs.com/" target="_blank">KnockoutJs</a>.To bring you up to speed, here is a typical example on the original usage of KnockoutJs:
	
<a href="http://4.bp.blogspot.com/-0RKqmvHtehw/T7y6xZJUVkI/AAAAAAAAAK8/mNSSL3LvMWQ/s1600/homepage-example.png" imageanchor="1"><img border="0" src="http://4.bp.blogspot.com/-0RKqmvHtehw/T7y6xZJUVkI/AAAAAAAAAK8/mNSSL3LvMWQ/s1600/homepage-example.png" /></a>

> Note that you have to specify all your bindings for an element in the *data-bind* attribute.

###This got me thinking...
Doing all these bindings in one attribute may become untidy and unreadable. On the HTML, I wanted a way to explicitly <em>define my bindings as attributes</em> on a specific element.

###...then KnockedUp was born
I created a library (dependent on JQuery & KnockoutJs) that gives you the same result as KnockoutJs and called it KnockedUp :) .

KnockedUp allows you to specify your data-bindings explicitly as normal attributes and not as in line values of the "*data-bind*" attribute. What this library does, is converting the KnockedUp syntax to proper KnockoutJs syntax.

These attribute-based bindings must start with an identifier, in my case "<strong><em>ko-</em></strong>". You can then have your HTML as:

####Choose a ticket class:
```html
<select 
	ko-options="tickets" 
	ko-optionsText="'name'"
	ko-optionsCaption="'Choose...'"
	ko-value="chosenTicket">
</select>
<button ko-enable="chosenTicket"
ko-click="resetTicket">Clear</button>
<p ko-with="chosenTicket">
	You have chosen
	<b ko-text="name"></b>($ <span ko-text="price"></span>)
</p>
```

KnockedUp will scan the body for elements that contain attributes starting with "<strong><em>ko-</em></strong>" and add these attributes as values to the applicable <strong><em>data-bind</em></strong> attribute of the applicable element, then KnockoutJs will kick in and perform its magic.
	
###Usage
Simply add the reference to KnockedUp and just before calling the `ko.applyBindings(...)` function of KnockoutJs, just call KnockedUp's function: `ku.applyMappings()`

```javascript
ku.applyMappings();
ko.applyBindings(new TicketsViewModel());
```

<a href="https://skydrive.live.com/redir?resid=4F6380499064F637!203" target="_blank">You can download the plug-in here</a> - *It's only about 800 bytes!*

> Disclaimer: Please note that this is currently only in beta phase as its only a concept for now.

Tweet me <a href="http://twitter.com/faniereynders" target="_blank">@FanieReynders</a> for any questions!

Cheers!