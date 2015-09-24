---
published: true
layout: post
author: Fanie Reynders
tags: 
  - Microsoft
  - Azure
  - Web App
  - Performance
  - Load test
title: Performance test Azure Web Apps
---

Microsoft Azure has many great offerings in the PaaS space, one of which is Azure Web Apps that allows one to easily host or create any web app built with any technology for any platform. The recent update to the new Azure Portal (in preview) brings not only a more polished look, but new functionality to web apps like performance testing.

<!--more-->

### Creating a performance test
From the dashboard in the Azure Portal, click *Web Apps*:

![snip_20150924142916.png]({{site.url}}/post-images/snip_20150924142916.png)

You will be presented with a list of all your current Web Apps. From this list, select the Web App you want to test. In this example, it's *fanie-test*:

![snip_20150924143211.png]({{site.url}}/post-images/snip_20150924143211.png)

Notice a new section on the Web App details window called *Tools*:

![snip_20150924143524.png]({{site.url}}/post-images/snip_20150924143524.png)

When opening the *Tools* section we see the new functionality right at our finger tips. Some of the features include:
- Live HTTP Traffic analyzer (under Troubleshoot)
- Performance Monitoring
- Process Explorer
- Performance Test
- Console

> There's a difference between Performance Monitoring and Performance Testing. The former applies to the existing Application Insights and the latter to the new Performance feature which is currently in preview.

Click *Performance Test* that will show a list of recent tests. 

![snip_20150924143858.png]({{site.url}}/post-images/snip_20150924143858.png)

> Performance test needs to be linked with a Visual Studio Online account.

To create a new test, click *New*:

![snip_20150924144934.png]({{site.url}}/post-images/snip_20150924144934.png)

You will then be prompted for some details about the test, like: The name, generate load from, user load and duration.

![snip_20150924145251.png]({{site.url}}/post-images/snip_20150924145251.png)

You can test the performance of your Web App by generating load from the same data center as your Web App (internal load) or even from a different data center (external load).

For this example we use a user load of 500 users and duration of 5 minutes. Click *Run test* to start and it will initiate the testing process by aquiring the needed resources.

![snip_20150924150127.png]({{site.url}}/post-images/snip_20150924150127.png)

Once it has started, sit back and watch results come in at real time:

![snip_20150924151750.png]({{site.url}}/post-images/snip_20150924151750.png)

When it's finished you can analize the results or even re-run the test.

*Till next time!*

(@FanieReynders)[https://twitter.com/faniereynders]
