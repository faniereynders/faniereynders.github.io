---
layout: post
title: Jump Start your next project with Nuget
date: '2011-11-22T16:06:00.000+02:00'
author: Fanie Reynders
tags:
- nuget
modified_time: '2012-11-19T16:07:27.113+02:00'
thumbnail: http://1.bp.blogspot.com/-mAWhZi_l5w0/TssrEK2E2kI/AAAAAAAAAJQ/zJxhGb9_K7I/s72-c/tools.png
blogger_id: tag:blogger.com,1999:blog-2911174713310708108.post-5714265895836370550
blogger_orig_url: http://www.faniereynders.com/2011/11/jump-start-your-next-project-with-nuget.html
---

In between the company end-year functions, demanding clients and of course my fabulous life :) ,&nbsp;I've deceided to do some blogging again. In fact, it's been a while since any Ninja laid a finger on a keyboard here, (or an iThing)... 

A while ago I came across a cool little thing called "NuGet" (pronounced&nbsp;<em>new-get</em>). No, it's not the kind of things you get from Mc Donalds. According to the&nbsp;<a href="http://nuget.org/" target="_blank">website</a>, NuGet is a Visual Studio extension that makes it easy to install and update [open source] libraries and tools in Visual Studio. But, I believe they're being modest. It's way more cooler than what meets the eye.

<!--more-->

Okay, so whats all the fuss about? Well, the main idea behind it is to allow you to install any files (libraries, media, code files etc) into your project / solution in a managed way with versioning &amp; dependency detection. In the NuGet world, these files are bundles into things&nbsp;called "packages".

When you use NuGet to install a package, it copies the [library] files to your solution and automatically updates your project (add references, change config files, etc). If you remove a package, NuGet reverses whatever changes it made so that no clutter is left.

There is a wide variety of libraries ready to be downloaded &amp; used&nbsp;(3680 to be exact) from the offical&nbsp;<a href="http://nuget.org/List/Packages" target="_blank">NuGet Package Source</a>. You can even create your own package source that can be hosted to be available externally or internally on your intranet - I will get to this later.

So, let's install NuGet...

Obviosly, first you need to open every Ninja's biggest asset: Visual Studio 2010.
	
1. Open up the 'Extension Manager' from the Tools menu
	
<a href="http://1.bp.blogspot.com/-mAWhZi_l5w0/TssrEK2E2kI/AAAAAAAAAJQ/zJxhGb9_K7I/s1600/tools.png" imageanchor="1">
	<img border="0" height="320" src="http://1.bp.blogspot.com/-mAWhZi_l5w0/TssrEK2E2kI/AAAAAAAAAJQ/zJxhGb9_K7I/s320/tools.png" width="270" />
</a>

2. Under the Online Gallery section, search for "NuGet" and click "Download"

<a href="http://1.bp.blogspot.com/-X30b863BfAg/TsssMktFoeI/AAAAAAAAAJY/m-edZLMJuqU/s1600/ExtensionMan.png" imageanchor="1">
<img border="0" height="392" src="http://1.bp.blogspot.com/-X30b863BfAg/TsssMktFoeI/AAAAAAAAAJY/m-edZLMJuqU/s640/ExtensionMan.png" width="640" /></a>

After downloading, "reading" the legal stuff &amp; installing, you need to restart VS for the changes to take effect.

That's it, now we have NuGet installed &amp; we can get "Nugetting" :)

For this example, I'm going to create a blank web forms project (yes&nbsp;I said web&nbsp;forms) and&nbsp;install "EntityFramework.Patterns" onto it. This is only to show the functionality of NuGet.

1. After&nbsp;you've created a blank project, right-click on 'References' there is an additional item "Manage NuGet Packages" - Click it. (or if you're using a touch screen, I rest my case)

<a href="http://4.bp.blogspot.com/-9O2Cs3EIhPY/TsswvQr204I/AAAAAAAAAJo/xLrMOF3Sc7k/s1600/ref.png" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" src="http://4.bp.blogspot.com/-9O2Cs3EIhPY/TsswvQr204I/AAAAAAAAAJo/xLrMOF3Sc7k/s1600/ref.png" /></a>

2. Next you will be presented by the main NuGet Manager<br /><div class="separator" style="clear: both; text-align: center;"><a href="http://2.bp.blogspot.com/-hZblqCOAAeQ/TssxFlyotiI/AAAAAAAAAJw/bm3H_hADyC0/s1600/NuGet.png" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="426" src="http://2.bp.blogspot.com/-hZblqCOAAeQ/TssxFlyotiI/AAAAAAAAAJw/bm3H_hADyC0/s640/NuGet.png" width="640" /></a></div><br />3. From the 'Online' section (on the left), search for "EntityFramework.Patterns" and hit "Install"<br /><div class="separator" style="clear: both; text-align: center;"><a href="http://4.bp.blogspot.com/-CGlxy9zuBio/TssyY02q7UI/AAAAAAAAAJ4/jCrV4dw70bw/s1600/Patterns.png" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="424" src="http://4.bp.blogspot.com/-CGlxy9zuBio/TssyY02q7UI/AAAAAAAAAJ4/jCrV4dw70bw/s640/Patterns.png" width="640" /></a></div><br />Note that EntityFramework 4.1 is required for this library to work (under dependencies). But have no fear NuGet handles it for us:<br /><div class="separator" style="clear: both; text-align: center;"><a href="http://4.bp.blogspot.com/-l1ZYf0mUTKc/Tssy_0mPq8I/AAAAAAAAAKA/pG9Gm9qikk4/s1600/installing.png" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="468" src="http://4.bp.blogspot.com/-l1ZYf0mUTKc/Tssy_0mPq8I/AAAAAAAAAKA/pG9Gm9qikk4/s640/installing.png" width="640" /></a></div><br />That's it... So if we go back to our little sample project you will notice that a few references were added including a packages.config file. (Also note that some content has been added to the web.config file)<br /><div class="separator" style="clear: both; text-align: center;"><a href="http://4.bp.blogspot.com/-IkqovDRvIN4/Tssz0sMWqBI/AAAAAAAAAKI/WjyB9ZZDoH8/s1600/refadded.png" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="400" src="http://4.bp.blogspot.com/-IkqovDRvIN4/Tssz0sMWqBI/AAAAAAAAAKI/WjyB9ZZDoH8/s400/refadded.png" width="212" /></a></div><br />There is of coarse an alternative "Ninja" solution to do all this, by using the Library Package Manager Console (Tools &gt; Library Package Manager &gt; Package Manager Console). Just type:<br /><br /><span style="font-family: &quot;Courier New&quot;, Courier, monospace;">Install<span style="color: grey;">-</span>Package EntityFramework.Patterns</span><br /><br />You can now use the installed library in your code. Im not going into how we use EntityFramework.Patterns, but hopefully you get the idea.<br /><br />But, where is the actual packages? If you go to your solution folder, you will note a "packages" folder. When you open it up, you'll notice all the packages that is installed in the solution. So best checking this folder into TFS!<br /><br /><span style="font-size: x-large;">RIGTH!</span><br />Now, for the cool stuff... Creating your own package...<br /><br />Creating your package is exactly like adding files to a ZIP file. But in this instance, the "zip" file is called a NuGet Package file (*.nupkg). There is also a specifications file called a NuSpec file (*.nuspec) that contains all the meta data of the package like version information &amp; dependencies etc.<br /><br />There are many ways of creating and publishing your packages:<br /><br /><a href="http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package" target="_blank">The Ninja Way</a><br /><a href="http://docs.nuget.org/docs/creating-packages/using-a-gui-to-build-packages" target="_blank">The Easy Way</a><br /><br />You can also<a href="http://docs.nuget.org/docs/creating-packages/hosting-your-own-nuget-feeds" target="_blank">&nbsp;host your own package feed</a><br /><br />You can view a variety of&nbsp;<a href="http://docs.nuget.org/docs/start-here/videos" target="_blank">videos available on this subject</a><br /><br />So, Ninjas... Now you have no excuse to deliver the "best in the business" code. Get coding away... Get packaging away and make your life easier with NuGet.<br /><br /><a href="http://www.twitter.com/faniereynders" target="_blank">Follow me on Twitter @FanieReynders</a><br /><br />Until next time..... Cheers.<br /><div><br /></div></div>