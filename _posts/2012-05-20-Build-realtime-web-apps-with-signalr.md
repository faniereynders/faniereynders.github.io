---
layout: post
title: 'Build real-time, multi-user interactive web apps with SignalR'
date: '2012-05-20T15:17:00.000+02:00'
author: Fanie Reynders
tags:
- aspnet
- signalr
modified_time: '2012-11-19T16:18:40.566+02:00'
blogger_id: tag:blogger.com,1999:blog-2911174713310708108.post-4401858995276481215
blogger_orig_url: http://www.faniereynders.com/2012/05/some-history-while-back-i-was-involved.html
---

A while back I was involved in developing an online-bidding app. This may not sound too exciting nor complicated, but because this kind of application was web-based, it introduced a whole new set of challenges. One of the core requirements of this app was to be critically up-to-date with real-time results. As a user bids on something, the action needs to be broadcast to the rest of the players bidding. The main challenge was achieving this on the web.

<!--more-->

The way web applications work, is the client sends a request to the server for certain information then the server sends back the response which contains the information. In our case where the client needed to be aware of certain server-side changes, the most obvious way was to poll the server periodically.

> *"Polling is a common example of hammering a screw</strong>. Trying to make a chat program? Poll every 5 seconds. Got a really long running transaction? Throw up an animated GIF and poll until eternity, my friend!"* - <a href="http://www.hanselman.com/blog/AsynchronousScalableWebApplicationsWithRealtimePersistentLongrunningConnectionsWithSignalR.aspx" target="_blank">Scott Hanselman</a>

Another way to achieve this is a concept called *<a href="http://en.wikipedia.org/wiki/Long_polling#Long_polling" target="_blank">Long Polling</a>*. Basically, open a connection and keep it open forcing the client to wait, pretending it's taking a long time to return. If you have enough control on your server-side programming model, this can allow you to return data as you like over this "open connection." If the connection breaks, it's transparently re-opened and the break is hidden from both sides. In the future things like <a href="http://en.wikipedia.org/wiki/Websockets" target="_blank">WebSockets</a> will be another way to solve this problem when it's baked.

###Then SignalR came along
Doing this kind of persistent connection in an app (like our bidding site) hasn't been easy in ASP.NET because there hasn't been a decent abstraction for this on the server or client library to talk to it.

SignalR is an asynchronous signaling library to build real-time interactive multi-user web applications.

###Easy elegant implementation
I'm not going into too much detail on this, the only thing I want to show is how easy we implement something as simple as a basic chat app. 

In your web-app project - hopefully its MVC :) - install the SignalR NuGet package. You can also do this using the <a href="http://docs.nuget.org/docs/start-here/using-the-package-manager-console" target="_blank">Package Manager Console</a>:

####Setup
```powershell
Install-Package SignalR
```

####Server
Create a class that derives from Hub, then add a function "Send" that expects a message as string. We will then use the Clients property and call a dynamic function "addMessage":

```csharp
public class Chat : Hub 
{
	public void Send(string message) 
	{
		Clients.addMessage(message);
	}
}
```

####Client
Add references to JQuery, SignalR & the virtual path "/signalr/hubs"

```html
<script src="Scripts/jquery-1.6.2.min.js" type="text/javascript"></script>
<script src="Scripts/jquery.signalR-0.5.0.min.js" type="text/javascript"></script>
<script src="/signalr/hubs" type="text/javascript"></script>
```

The HTML:

```html
<input type="text" id="msg" />
<input type="button" id="broadcast" value="broadcast" />
<ul id="messages"></ul>
```

Next, we instantiate our server class as a proxy on the client-side:

```html
<script type="text/javascript">
$(function () {
	var chat = $.connection.chat;
	//...
});
</script>
```

Keep in mind that the connection.chat directly translates to our Chat class (which derives from Hub). 

Next, we declare a function on the chat hub so the server can invoke it:

```javascript
chat.addMessage = function(message) {
	$('#messages').append('<li>' + message + '</li>');
};
```

When we click the 'broadcast' button, we want to call the server-side "Send" function:

```javascript
$("#broadcast").click(function () {
	chat.send($('#msg').val());
});
```

And last (but not least) we start the connection:

```javascript
$.connection.hub.start();
```

This is not a lot of code, but yet again you probably can make it shorter and sexier using <a href="http://knockoutjs.com/" target="_blank">KnockoutJs</a> ([I blogged about it recently]({% post_url 2012-05-14-mvvm-for-web-using-knockoutjs %})). SignalR will work really well with KnockoutJs.

###Demo
There is a small chat application running on Azure at <a href="http://jabbr.net/">http://jabbr.net</a> that was built with this technology. Feel free to check it out and give it a try.

<a href="http://www.hanselman.com/blog/content/binary/WindowsLiveWriter/Asynchronousscalablewebapplicationswithp_122E/SignalR%20Chat%20-%20Google%20Chrome%20(69)_thumb.png" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="544" src="http://www.hanselman.com/blog/content/binary/WindowsLiveWriter/Asynchronousscalablewebapplicationswithp_122E/SignalR%20Chat%20-%20Google%20Chrome%20(69)_thumb.png" width="640" /></a>

I hope someone finds this useful.

Thanks! Until next time...

<a href="http://twitter.com/faniereynders" target="_blank">@FanieReynders</a>