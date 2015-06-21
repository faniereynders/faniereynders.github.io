---
layout: post
title: Clouds in my CoffeeScript
date: '2014-01-07T12:12:00.001+02:00'
author: Fanie Reynders
tags:
- javascript
- coffeescript
- azure
modified_time: '2014-01-07T13:56:51.801+02:00'
thumbnail: http://lh6.ggpht.com/-M5JuRqTFiMI/UsvTAvdMhoI/AAAAAAAAAlY/LQNmkanXC_Y/s72-c/image_thumb%25255B6%25255D.png?imgmax=800
blogger_id: tag:blogger.com,1999:blog-2911174713310708108.post-5922683429692015212
blogger_orig_url: http://www.faniereynders.com/2014/01/clouds-in-my-coffeescript.html
---

The "<a href="http://en.wikipedia.org/wiki/CoffeeScript">mystery language</a>" <a href="http://coffeescript.org/">CoffeeScript</a> has been around for about 5 years now and I only recently started to play around in it. Like <a href="http://www.typescriptlang.org/">TypeScript</a>, I must add that it's a bit of a learning curve, but once you get the hang of it, it's smooth sailing from there. I've promised myself to keep this short and sweet, so I decided to write an article about calling a <a href="http://www.windowsazure.com/en-us/develop/mobile/">Windows Azure Mobile Service (WAMS)</a> from CoffeeScript. 

<!--more-->

Let's face it, there aren't any smoke and mirrors here. CoffeeScript transcompiles down to JavaScript, like TypeScript does, so calling any REST-based service with the help of your favourite JS framework (in my case <a href="http://jquery.com/">JQuery</a>) shouldn't be any different. 

[In my previous post]({% post_url 2014-01-06-introducing-geekfest %}), I've created a Mobile Service that periodically polls tweets from Twitter and pushes it to a table called 'tweets'. So next, using the power of CoffeeScript, let's build a small sample that reads the tweets from the table and show it on the screen. 

In this example, my weapons of choice are: 

- Visual Studio 2013 
- <a href="http://visualstudiogallery.msdn.microsoft.com/56633663-6799-41d7-9df7-0f2a504ca361">Web Essentials 2013</a>
- <a href="http://www.nuget.org/packages/WindowsAzure.MobileServices/">Windows Azure Mobile Services</a> JavaScript client (from Nuget)
- <a href="http://jquery.com/">JQuery</a>

Here we have simple HTML with references to JQuery, WAMS JS client and a custom script, which is the product of compiled CoffeeScript:

```html
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <title></title>
</head>
<body>
    <ul id="tweets"></ul>
 
    <script src="http://code.jquery.com/jquery-1.10.1.min.js"></script>
    <script src="http://ajax.aspnetcdn.com/ajax/mobileservices/MobileServices.Web-1.1.0.min.js"></script>
    <script src="app.min.js"></script>
</body>
</html>
```

The nice thing about Web Essentials is that it supports CoffeeScript out of the box, I’ve just added a file with a ‘.coffee’ extension and we’re good to go. This extension will generate the optimized JavaScript (including a minified version) for us upon save.

The app.coffee script:

```coffeescript
url = "https://myservice.azure-mobile.net/"
key = "sUp3RsEcR3T-k3Y"
client = new WindowsAzure.MobileServiceClient(url, key)
 
tweetsTable = client.getTable("tweets")
 
results = tweetsTable.read().then(
    (results) -> showTweets(results), 
    (error) -> error(error))
 
showTweets = (result) ->
    $.each(result, ->
        $("#tweets").append("<li>" + this.text + "</li>"))
 
error = (error) ->
    alert "ERROR: " + error
```

When we save this, it immediately generates an app.js and app.min.js that we can reference from within the HTML page.

<a href="http://lh5.ggpht.com/-LXRaDClAlho/UsvS_xd64cI/AAAAAAAAAlQ/kwqRRbwZ0x4/s1600-h/image%25255B10%25255D.png"><img alt="image" border="0" src="http://lh6.ggpht.com/-M5JuRqTFiMI/UsvTAvdMhoI/AAAAAAAAAlY/LQNmkanXC_Y/image_thumb%25255B6%25255D.png?imgmax=800" height="178" title="image" width="240" /></a> 

Comparing the generated JavaScript to the original CoffeeScript we notice the substantial difference in row count:

<a href="http://lh6.ggpht.com/-mMuJgfKUFmc/UsvTBBbCR1I/AAAAAAAAAlg/icOc4lU1uCI/s1600-h/image%25255B27%25255D.png"><img alt="image" border="0" src="http://lh4.ggpht.com/-WnuDKXHCyMA/UsvTBwfB5bI/AAAAAAAAAlo/11quXz_lGro/image_thumb%25255B19%25255D.png?imgmax=800" height="307" title="image" width="640" /></a>

*Till next time!*