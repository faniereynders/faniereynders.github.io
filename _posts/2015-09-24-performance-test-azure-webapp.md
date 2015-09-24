---
published: false
layout: post
author: Fanie Reynders
tags: 
  - Microsoft
  - Azure
  - Web App
  - Performance
  - Load test
---


Microsoft Azure has many great offerings in the PaaS space, one of which is Azure Web Apps that allows one to easily host or create any web app built with any technology for any platform. The recent update to the new Azure Portal (in preview) brings not only a more polished look, but new functionality to web apps like performance testing.

## Creating a performance test
From the dashboard in the Azure Portal, click *Web Apps*:

![snip_20150924142916.png]({{site.baseurl}}/_posts/snip_20150924142916.png)

You will be presented with a list of all your current Web Apps. From this list, select the Web App you want to test. In this example, it's *fanie-test*:

![snip_20150924143211.png]({{site.baseurl}}/_posts/snip_20150924143211.png)

Notice a new section on the Web App details window called *Tools*:

![snip_20150924143524.png]({{site.baseurl}}/_posts/snip_20150924143524.png)

When opening the *Tools* section we see the new functionality right at our finger tips. Some of the features include:
- Live HTTP Traffic analyzer (under Troubleshoot)
- Performance Monitoring
- Process Explorer
- Performance Test
- Console

> There's a difference between Performance Monitoring and Performance Testing. The former applies to the existing Application Insights and the latter to the new Performance feature which is currently in preview.

Click *Performance test* that will show a list of present/past tests. 

