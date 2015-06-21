---
layout: post
title: Setting up continuous integration from GitHub to NuGet using MyGet
date: '2014-06-13T18:18:00.000+02:00'
author: Fanie Reynders
tags:
- GitHub
- CI
- nuget
- MyGet
modified_time: '2014-07-03T13:40:43.451+02:00'
thumbnail: http://4.bp.blogspot.com/-lsjeF2hK6rQ/U5sJEzDTpNI/AAAAAAAAAqo/H_ClwoJiln0/s72-c/workflow.png
blogger_id: tag:blogger.com,1999:blog-2911174713310708108.post-5688969857813335508
blogger_orig_url: http://www.faniereynders.com/2014/06/setting-up-continuous-integration-from-github-to-nuget-using-myget.html
---

It's been a while since my last blog post so I decided to dust of my good ol' Blogger account and get to it. I recently relocated from sunny South Africa to begin my brand new career in the beautiful Netherlands and I must say that I'm absolutely loving it!

My first task was to implement continuous integration on one of the (soon to be) open source projects to deliver packages on <a href="http://nuget.org/">NuGet</a>. After finding plenty of resources on the web about <a href="http://www.jetbrains.com/teamcity/">TeamCity</a>, <a href="https://appharbor.com/">AppHarbour</a> and <a href="http://octopusdeploy.com/">Octopus Deploy</a>, I finally decided on using <a href="http://myget.org/">MyGet</a> which delivers a NuGet-as-a-Service experience.

<!--more-->

The main goal is to have a build server up in the sky (MyGet) that builds the latest stable branch of my solution on every commit (from <a href="http://github.com/">GitHub</a>) and runs the automated unit tests. If all the unit tests pass, it needs to increment the version number, create a NuGet package and deploy it to my (private) MyGet repository for UAT. After approval, the package owner will hit the magic button to push the package to a public package repository like NuGet.</div><a href="https://www.blogger.com/null" name="more"></a>

<a href="http://4.bp.blogspot.com/-lsjeF2hK6rQ/U5sJEzDTpNI/AAAAAAAAAqo/H_ClwoJiln0/s1600/workflow.png" imageanchor="1"><img border="0" src="http://4.bp.blogspot.com/-lsjeF2hK6rQ/U5sJEzDTpNI/AAAAAAAAAqo/H_ClwoJiln0/s1600/workflow.png" /></a>

###Creating the MyGet package feed
The first thing we need to do is create a MyGet package feed. Head over to <a href="http://octopusdeploy.com/">MyGet.org</a>, log in and click the big ‘New Feed’ button.

<a href="http://lh3.ggpht.com/-VO3nnRQTbKs/U5sjszKQQNI/AAAAAAAAAq4/zqSV-izcbvg/s1600-h/image%25255B7%25255D.png"><img alt="image" border="0" src="http://lh6.ggpht.com/-nbq9H3Nml-0/U5sjtyyrNrI/AAAAAAAAArA/zc4LzCvWrjo/image_thumb%25255B5%25255D.png?imgmax=800" height="292" title="image" width="611" /></a>

###Link up GitHub
Next, we need to add a new build source from GitHub by going to the Build Services section of the feed properties, where we click ‘Add build services…’ and by choosing ‘from GitHub’

<a href="http://lh3.ggpht.com/-zVuyZD3C020/U5sjvHPva6I/AAAAAAAAArI/4lAFt416xtU/s1600-h/image%25255B26%25255D.png"><img alt="image" border="0" src="http://lh3.ggpht.com/-zEF-VA_ik10/U5sjweQiB0I/AAAAAAAAArQ/5PNpGtMZqnk/image_thumb%25255B18%25255D.png?imgmax=800" height="352"  title="image" width="660" /></a>

This will prompt you to link your GitHub account with MyGet and after authorisation, we need to select the repository we want to link. In my case, it is ‘NugetCI’ and click ‘Add’.
	
<a href="http://lh4.ggpht.com/-S2qQTbGVZ5k/U5sjxf4TChI/AAAAAAAAArY/h4wg7HOvBrA/s1600-h/image%25255B19%25255D.png"><img alt="image" border="0" src="http://lh4.ggpht.com/-YSL574H0FcM/U5sjytMSPTI/AAAAAAAAArg/wtKlHeYZ_zE/image_thumb%25255B13%25255D.png?imgmax=800" height="105"  title="image" width="660" /></a>

Once it has been added, we can change additional properties like what branch we want to pull, versioning semantics etc., by clicking ‘Edit’.

To keep things simple, we will assume the defaults.

###Build baby, build!
Now all that is left to do is to make some changes and commit it to my repo on GitHub. Once it has synced, it queues up a new build after a short while.

<a href="http://lh5.ggpht.com/-i5eOV8kieHk/U5sjztkH2JI/AAAAAAAAAro/rmKYwKwojq8/s1600-h/image%25255B33%25255D.png"><img alt="image" border="0" src="http://lh6.ggpht.com/-zz3hlzSTU-M/U5sj2BUawOI/AAAAAAAAArw/6na2aWrISas/image_thumb%25255B21%25255D.png?imgmax=800" height="216"  title="image" width="660" /></a>

This will attempt to build the project as well as look for any unit tests to run. If everything was successful, we will be presented with a green success icon.

We can see our brand new deployed NuGet package from the Packages section and download it from the feed <a href="https://www.myget.org/F/fanies-feed/" title="https://www.myget.org/F/fanies-feed/">https://www.myget.org/F/fanies-feed/</a>

###Get it to NuGet
Lastly, we need to push it to NuGet. To do this, we need to get our API Key from NuGet, which we will find under the ‘My NuGet.org Account’ page in NuGet.

<a href="http://lh4.ggpht.com/-TNduXsiRBJY/U5sj5e-busI/AAAAAAAAAr4/O_038c9A6cU/s1600-h/image%25255B36%25255D.png"><img alt="image" border="0" src="http://lh6.ggpht.com/-XO2lj1FK5ko/U5sj7LtCm2I/AAAAAAAAAsA/cL9-e7jhcjk/image_thumb%25255B24%25255D.png?imgmax=800" height="500"  title="image" width="654" />

After heading back to MyGet, we edit the default ‘NuGet.org’ package source from the Package Sources section and add the API key from NuGet under Authentication:

<a href="http://lh5.ggpht.com/-WmPkHxzxeOg/U5sj8Mf1HqI/AAAAAAAAAsI/QAPcR2JkClk/s1600-h/image%25255B41%25255D.png"><img alt="image" border="0" src="http://lh3.ggpht.com/-pQJ5PPcLxTw/U5sj9Xe63SI/AAAAAAAAAsQ/lCxHbEKW78Y/image_thumb%25255B27%25255D.png?imgmax=800" height="234"  title="image" width="644" /></a>
	
<a href="http://lh6.ggpht.com/-uYpZ9br07k0/U5sj-M3ChQI/AAAAAAAAAsY/a7-FC07mtKg/s1600-h/image%25255B46%25255D.png"><img alt="image" border="0" src="http://lh5.ggpht.com/-6DRw1g06p70/U5sj_fy8K7I/AAAAAAAAAsg/9HPfNJr50Uk/image_thumb%25255B30%25255D.png?imgmax=800" height="500"  title="image" width="575" /></a>
	
Now we can push packages to NuGet. We do this by going to the Packages section and clicking the tiny push icon 

<a href="http://lh4.ggpht.com/-dOYfUpfzEDk/U5skAccv3lI/AAAAAAAAAsk/t1RC4BrrrXk/s1600-h/image%25255B29%25255D.png"><img alt="image" border="0" src="http://lh6.ggpht.com/-IedWP2ptwj0/U5skBPdCHZI/AAAAAAAAAsw/gGMhCOb4vXI/image_thumb%25255B19%25255D.png?imgmax=800" height="34" title="image" width="39" /></a>

MyGet will proceed to invoke NuGet’s API pushing our package to the applicable NuGet feed. One can also enable mirroring to automatically push packages periodically to NuGet. 

###That’s all folks
We’ve barely scratched the surface of MyGet’s features but I was so excited to share my find on this and I was quite impressed by all the nifty things you can do. Everything is simply clean and to the point, it even picks up your .nuspec files within each project for building more advanced NuGet packages. I hope to write more on this subject in the future.

*Till next time!*