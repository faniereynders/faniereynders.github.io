---
layout: post
title: Using the Twitter REST API v1.1 from Azure Mobile Services
date: '2013-06-20T21:01:00.001+02:00'
author: Fanie Reynders
tags:
- twitter api
- mobile services
- azure
modified_time: '2013-06-20T21:06:34.475+02:00'
blogger_id: tag:blogger.com,1999:blog-2911174713310708108.post-3098746597741610973
blogger_orig_url: http://www.faniereynders.com/2013/06/using-twitter-rest-api-v11-from-azure.html
---

Since the <a href="https://dev.twitter.com/blog/api-v1-is-retired" target="_blank">retirement of the Twitter REST API v1</a>, I have been having trouble doing a simple query using the <a href="https://dev.twitter.com/docs/api/1.1/get/search/tweets" target="_blank">new search API introduced in version 1.1</a> from Azure Mobile Services because it now requires a <a href="https://dev.twitter.com/docs/auth/oauth#v1-1" target="_blank">signed authentication</a> header in the request using the <a href="http://tools.ietf.org/html/rfc5849#section-1.2" target="_blank">OAuth 1.0 protocol</a>.

<!--more-->

> I would suggest reading the tutorial on  <a href="http://www.windowsazure.com/en-us/develop/mobile/tutorials/schedule-backend-tasks/" target="_blank">Schedule recurring jobs in Mobile Services</a> which provides a basic walk through on creating a scheduled job in Azure Mobile Services that requests tweets from Twitter and stores it in a table.

The reason you should read this tutorial is to gain some background knowledge on the concept for better understanding of my newly devised method of use.

The problem with the [out-dated] tutorial mentioned above is that it still uses version 1 of the recently depreciated Twitter REST API.

###Current (old) approach
Here's the code for the scheduler borrowed from the [current] tutorial:

```javascript
var updatesTable = tables.getTable('Updates');
var request = require('request');
 
function getUpdates() {   
    // Check what is the last tweet we stored when the job last ran
    // and ask Twitter to only give us more recent tweets
    appendLastTweetId(
        'http://search.twitter.com/search.json?q=%23mobileservices&result_type=recent', 
        function twitterUrlReady(url){
            request(url, function tweetsLoaded (error, response, body) {
                if (!error && response.statusCode == 200) {
                    var results = JSON.parse(body).results;
                    if(results){
                        console.log('Fetched new results from Twitter');
                        results.forEach(function visitResult(tweet){
                            if(!filterOutTweet(tweet)){
                                var update = {
                                    twitterId: tweet.id,
                                    text: tweet.text,
                                    author: tweet.from_user,
                                    date: tweet.created_at
                                };
                                updatesTable.insert(update);
                            }
                        });
                    }            
                } else { 
                    console.error('Could not contact Twitter');
                }
            });
    });
}
 
// Find the largest (most recent) tweet ID we have already stored
// (if we have stored any) and ask Twitter to only return more
// recent ones
function appendLastTweetId(url, callback){
    updatesTable
    .orderByDescending('twitterId')
    .read({success: function readUpdates(updates){
        if(updates.length){
            callback(url + '&since_id=' + (updates[0].twitterId + 1));
        } else {
            callback(url);
        }
    }});
}

function filterOutTweet(tweet){
    // Remove retweets and replies
    return (tweet.text.indexOf('RT') === 0 || tweet.to_user_id);
}
```

After plenty of research (and failed attempts) on properly signing the request to have Twitter authenticate it using custom code, I realised that Azure Mobile Services is hosted by a NodeJs process and learned that the 'Request'-object comes out-of-the-box with OAuth support.

###New (updated) approach
By simply assigning the application's keys and tokens to the OAuth property of the request, it worked like a charm:

```javascript
var updatesTable = tables.getTable('Updates');
var request = require('request');
 
var url = "https://api.twitter.com/1.1/search/tweets.json?q=%23mobileservices&result_type=recent";
    
var consumerKey = '[your consumer key]',
    accessToken= '[your access token]',
    consumerSecret = '[your consumer secret]',
    accessTokenSecret = '[your access token secret]';
 
function getUpdates() {   
    // Check what is the last tweet we stored when the job last ran
    // and ask Twitter to only give us more recent tweets
    appendLastTweetId(
    	url, 
        function twitterUrlReady(url){
        	
            request.get({
                url: url,
                oauth: {
                    consumer_key: consumerKey,
                    consumer_secret: consumerSecret,
                    token: accessToken,
                    token_secret: accessTokenSecret
                }
            }, function tweetsLoaded (error, response, body) {
                if (!error && response.statusCode == 200) {
                    var results = JSON.parse(body).statuses;
                    if(results){
                        console.log('Fetched new results from Twitter');
                        results.forEach(function visitResult(tweet){
                            if(!filterOutTweet(tweet)){
                                var update = {
                                    twitterId: tweet.id,
                                    text: tweet.text,
                                    author: tweet.user.screen_name,
                                    date: tweet.created_at,
                                    photo: tweet.user.profile_image_url
                                };
                                updatesTable.insert(update);
                            }
                        });
                    }            
                } else { 
                    console.error('Could not contact Twitter');
                }
            });
    });
 }
 
// Find the largest (most recent) tweet ID we have already stored
// (if we have stored any) and ask Twitter to only return more
// recent ones
function appendLastTweetId(url, callback){
    updatesTable
    .orderByDescending('twitterId')
    .read({success: function readUpdates(updates){
        if(updates.length){
            callback(url + '&since_id=' + (updates[0].twitterId + 2));
        } else {
            callback(url);
        }
    }});
}
 
function filterOutTweet(tweet){
    // Remove retweets and replies
    return (tweet.text.indexOf('RT') === 0 || tweet.to_user_id);
}
```

> Do take note, there are some breaking changes to the API therefor I strongly recommend studying the Twitter API 1.1 documentation. Some of the other changes I had to make to the original code include changing the expected response body property 'results' to 'statuses' as well as the mapping to the.'update' object.

I hope that this saves someone out there somewhere a lot of trouble and time.

Your comments and tweets are welcome.

<a href="http://twitter.com/faniereynders" target="_blank">@FanieReynders</a>.

*Till next time!*