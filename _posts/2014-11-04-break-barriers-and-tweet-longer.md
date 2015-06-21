---
layout: post
title: Break the barriers and tweet longer
date: '2014-11-04T16:18:00.000+02:00'
author: Fanie Reynders
tags:
- javascript
- mobile services
- mvc
- azure
- Twitter
- Chrome Extensions
- html
modified_time: '2014-11-04T16:18:44.746+02:00'
thumbnail: http://2.bp.blogspot.com/--7orftMBpyk/VFjfMxWa-fI/AAAAAAAAAKk/W0uifXOOyxU/s72-c/Untitled.png
blogger_id: tag:blogger.com,1999:blog-2911174713310708108.post-9149886323593591759
blogger_orig_url: http://www.faniereynders.com/2014/11/break-barriers-and-tweet-longer.html
---

It's been a while since I found a decent time to post something useful. As the saying goes: <i>There isn't any better time than the present</i>. So while on my way on a super long flight, to rainy Seattle for the <a href="http://mvp.microsoft.com/summit">MVP Summit</a>, I've got plenty of time to kill. Just wished I had WiFi earlier...

<!--more-->

Social media is the next big thing at the moment and with <a href="http://en.wikipedia.org/wiki/Cloud_computing">cloud computing</a> driving this, the possibilities are '<i>virtually</i>' endless (no pun intended). While every social platform has its place, I strongly believe that there are still lots of room for improvement.

Take <a href="http://facebook.com/">Facebook </a>for example, back in the day it was [do I dare say] the replacement for <a href="http://myspace.com/">MySpace</a>. Let's be honest, it was cool at the time, but nowadays, with the increasing number of junk posts, like <a href="http://www.farmville.com/">Farmville</a>, I personally prefer <a href="http://twitter.com/">Twitter</a> as my sharing tool of choice. This is for the simple reason of the clean-cut and down-to-the-point useful information on my terms.

As with anything, there are of course the down sides: Twitter is fundamentally designed to limit the amount of information per post. This is to force the users to really think about they're trying to say. Don't get me wrong, this is a good thing, but sometimes I want the <i>best of both worlds</i> to get my point across properly without using <a href="http://www.urbandictionary.com/define.php?term=geek+speak">G33k speak</a> that, in some cases make no sense.

<blockquote class="twitter-tweet" lang="en">I rSpect ppl hu cn txt complEt sentences.<br />— Scott Hanselman (@shanselman) <a href="https://twitter.com/shanselman/status/528617126941437952">November 1, 2014</a></blockquote><script async="" charset="utf-8" src="//platform.twitter.com/widgets.js"></script> 

###Fixing what's broken
There are times when you just want to write a short-<i>ish </i>tweet about something, but are forced to either split up the post in smaller bits or by <i>shrtng ur wrds</i>. #Dont #get #me #started #on #hashtags!

I was thinking about fixing this 'problem' in a creative way, by enabling longer tweets, but still allowing for atomic bits of information.

###Say hello to Looong Tweet
The [somewhat] <a href="http://ltwt.co/">creative way I chose to solve this problem</a> was by building a <a href="http://www.google.com/chrome/">Google Chrome</a> extension that works with Twitter's web app. In the event of you running out of your hundred and forty characters, the usual 'Tweet' button is replaced by a 'Shorten' one. When this magic button is clicked, with the power of <a href="http://azure.com/">Azure</a> as it's back-end, it will shorten your version, also including a link to the original one so it fits perfectly within the limited characters.

####The components
We need:

1. A mechanism to replace the default 'Tweet' button with the 'Shorten' when the amount of characters exceed the limit. For this, I used pure JavaScript that gets injected into the page by means of a Chrome Extension.

```javascript
var tweetBox = document.getElementById("tweet-box-mini-home-profile");
var tweetButton = document.getElementsByClassName("js-tweet-btn")[0];
var tweetCounter = document.getElementsByClassName("tweet-counter")[0];
var avatarImg = document.getElementsByClassName("DashboardProfileCard-avatarImage")[0].src;
var handle = document.getElementsByClassName("DashboardProfileCard-screennameLink")[0].innerHTML;
var loadingIcon = "fa fa-spinner fa-spin";
var normalIcon = "fa fa-cut";
var htmlRegex = /(<([^>]+)>)/ig;
var head = document.getElementsByTagName('head')[0];
var link = document.createElement("link");
var btn = document.createElement("DIV")
    
//The shorten button html
var shortenButton = '<button id="shorten-btn" class="btn primary-btn tweet-btn " type="button">' +
                    '<span class="button-text tweeting-text">' +
                    '<span id="shortenBtnIcon" style="font-size:19px;position: relative;vertical-align: middle;" class="fa fa-cut"></span> ' + 
                    'Shorten' +
                    '</span>' +
                    '</button>';
 
//Stylesheet for Font Awesome icon
link.setAttribute("href","//maxcdn.bootstrapcdn.com/font-awesome/4.2.0/css/font-awesome.min.css");
link.setAttribute("rel","stylesheet");
head.appendChild(link);
 
//Actual creation of shorten button
btn.innerHTML = shortenButton;
btn.addEventListener("click", setText);
 
//Listen for keystrokes to toggle 'Tweet' and 'Shorten' buttons
tweetBox.onkeyup = validate;
 
area.appendChild(btn);
```

Every Chrome extension also needs a manifest to describe its behavior. Here's the manifest for Looong Tweet:

```json
{
"name":"Looong Tweet",
"description":"Looong Tweet allows you to tweet longer tweets from Twitter",
"version":"1",
"manifest_version":2,
"icons": { 
  "16": "icon16.png",
  "48": "icon48.png",
  "128": "icon128.png" },
  "permissions": [
    "https://short-tweet.azure-mobile.net/"
  ],
"content_scripts": [{
    "matches": ["https://twitter.com/*"],
    "js": ["script.js","MobileServices.Web-1.2.5.min.js"]
  }]
}
```

As you can see from the snippet above, it declares two external resources (script.js and MobileServices.Web-1.2.5.min.js) that it will inject into the DOM when the address in the browser matches `https://twitter.com/*`. We also need to explicity give permission to the extension for making cross-domain AJAX calls to the mobile service in Azure.

> The extension is running locally on the browser and not in a server such as localhost

2. Some logic to send the original tweet to Azure, including metadata like the username and avatar.

```javascript
function setText(){
    toggleLoading(true);
    var client = new WindowsAzure.MobileServiceClient(
        "https://short-tweet.azure-mobile.net/",
        "<YOUR KEY HERE>"
    );
    var tweet = tweetBox.innerHTML.replace(htmlRegex,'');
    var twHandle = handle.replace(htmlRegex,'');
    var item = { key:uid(), text: tweet, avatar: avatarImg, handle: twHandle };
 
    client.getTable("Item").insert(item).done(function(data){
        tweetBox.innerHTML = "<div>" + data.text + "</div>";
        tweetBox.focus();
        validate();
        toggleLoading(false);
    });
}
```

Here we make use of the Azure Mobile Services JavaScript Client (which I just included as a local resource) to talk to our API in the sky.  

3. An API that trims the tweet to the leaner version with a link.

```javascript
function insert(item, user, request) {
    request.execute({
        success: function () {
            
            var tweet = item.text.substring(0,114) + "... ";
            var link = "http://ltwt.co/" + item.key;
            var response = {
               text: tweet + link
            };
        
            request.respond(statusCodes.OK, response );
        }
    });
}
```

4. A clean website for showing the original tweet with Disqus comments

<a href="http://2.bp.blogspot.com/--7orftMBpyk/VFjfMxWa-fI/AAAAAAAAAKk/W0uifXOOyxU/s1600/Untitled.png" imageanchor="1"><img border="0" src="http://2.bp.blogspot.com/--7orftMBpyk/VFjfMxWa-fI/AAAAAAAAAKk/W0uifXOOyxU/s1600/Untitled.png" height="475" width="640" /></a>

For this I decided to keep things simple by just using a basic MVC 5 application that uses the WAMS C# client to retrieve the tweet from the API and beautifully displays it without any clutter.

```csharp
[Route("{id}")]
public async Task<ActionResult> Tweet(string id)
{
  var item = (await MobileService.GetTable<Item>().Where(i => i.Key == id).ToListAsync()).FirstOrDefault();
  item.Text = ParseTweet(item.Text);
  return View(item);
}
```

###Start using Looong Tweet
This extension is available on the Chrome Web Store right now. So go on, go get it and start tweeting longer tweets that actually makes sense.
<a href="http://3.bp.blogspot.com/-EHMX4rz2nLU/VFje8gCCJXI/AAAAAAAAAKc/wt6cPnt3g9c/s1600/Untitled.png" imageanchor="1"><img border="0" src="http://3.bp.blogspot.com/-EHMX4rz2nLU/VFje8gCCJXI/AAAAAAAAAKc/wt6cPnt3g9c/s1600/Untitled.png" height="129" width="320" /></a>

###That's a wrap
In an upcoming post, I'll show how to package and deploy a Chrome Extension to the store. I hope to make someone wiser who wants to build extensions for Chrome. Please feel free to comment on the content. Your feedback is greatly appreciated.