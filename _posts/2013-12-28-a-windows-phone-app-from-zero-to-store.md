---
layout: post
title: A Windows Phone app from zero to store in 5 easy steps
date: '2013-12-28T16:59:00.001+02:00'
author: Fanie Reynders
tags:
- Windows Phone
- App Studio
modified_time: '2013-12-31T10:25:09.802+02:00'
thumbnail: http://lh6.ggpht.com/-aNMoibq3jtE/Ur7sVlgNb5I/AAAAAAAAAes/KtAWgQ17yb8/s72-c/image_thumb%25255B6%25255D.png?imgmax=800
blogger_id: tag:blogger.com,1999:blog-2911174713310708108.post-5052669024612545536
blogger_orig_url: http://www.faniereynders.com/2013/12/a-windows-phone-app-from-zero-to-store.html
---

For those savvy developers out there, who have great ideas for apps but not enough time to implement it, this comprehensive how-to guide will come in quite handy. The <a href="http://www.blogger.com/apps.windowsstore.com" target="_blank">Windows Phone App Studio</a> is an online <a href="http://en.wikipedia.org/wiki/WYSIWYG" target="_blank">WYSIWIG</a> app builder designed to get you going in a snap. It allows you to build and test your app in no time for immediate publishing to the <a href="http://www.windowsphone.com/store" target="_blank">Windows Phone Store</a> and share it with others. The great thing about this is that it generates the source code for you, enabling you to do more advanced modifications using <a href="http://www.visualstudio.com/" target="_blank">Visual Studio</a>. Oh and did I mention it compiles totally native code?

<!--more-->

In this article we will be building a simple product catalogue app for Windows Phone using Windows Phone App Studio. Once we’re done, we will go through the simple steps to submit the app to the store for approval.

###What you’ll need

- A Windows Live account – <a href="https://signup.live.com/" target="_blank">get one here</a>
- A Windows Phone Developer account for publishing apps to the store – <a href="http://msdn.microsoft.com/en-us/library/windowsphone/help/jj206719(v=vs.105).aspx" target="_blank">sign up here</a>
- A Windows Phone for testing – <a href="http://www.windowsphone.com/" target="_blank">buy one here</a>&nbsp;:)

Once you’re ready, navigate your browser to <a href="http://apps.windowsstore.com/">http://apps.windowsstore.com/</a> and click the big ‘<em>Start building</em>’ button. This will take you to the log in screen if you not logged in yet.

<a href="http://lh3.ggpht.com/-bOUSG6AMVsI/Ur7sUnMWYDI/AAAAAAAAAek/whn6Kwl35sM/s1600-h/image%25255B12%25255D.png"><img alt="image" border="0" height="340" src="http://lh6.ggpht.com/-aNMoibq3jtE/Ur7sVlgNb5I/AAAAAAAAAes/KtAWgQ17yb8/image_thumb%25255B6%25255D.png?imgmax=800" title="image" width="644" /></a>

After logging in, you will be presented with the dashboard:
<a href="http://4.bp.blogspot.com/-o40HUKiRDFk/Ur7sWa19lkI/AAAAAAAAAe4/v6m0kmwAbsY/s1600/image%255B13%255D" imageanchor="1"><img border="0" height="337" src="http://4.bp.blogspot.com/-o40HUKiRDFk/Ur7sWa19lkI/AAAAAAAAAe4/v6m0kmwAbsY/s640/image%255B13%255D" width="640" /></a>
	
###Let's begin building an app

To start creating an app, click ‘<em>Create</em>’ from the menu which will prompt you to select a template:

<a href="http://lh5.ggpht.com/-vKPybGRPKy4/Ur7sX7k70eI/AAAAAAAAAfE/PyfulCpiy00/s1600-h/image%25255B17%25255D.png"><img alt="image" border="0" height="340" src="http://lh5.ggpht.com/-4ZWjfXbQcAA/Ur7sZAj-MgI/AAAAAAAAAfM/Lx-XWW7-0GM/image_thumb%25255B9%25255D.png?imgmax=800"   title="image" width="644" /></a>

<a href="http://lh6.ggpht.com/-ATaAU5_ItH8/Ur7sZtRdw2I/AAAAAAAAAfU/647anXBPido/s1600-h/image%25255B23%25255D.png"><img align="left" alt="image" border="0" height="51" src="http://lh4.ggpht.com/-X0fKsQ0826I/Ur7saa3gmSI/AAAAAAAAAfY/mySmtKN1wbo/image_thumb%25255B13%25255D.png?imgmax=800" title="image" width="51" /></a>

The best way to go is to select a predefined template to work with, but if you really want to, you can create an empty app. For simplicity we are going to choose the ‘<em>Catalog</em>’ template and click ’<em>Create App</em>’ from the popup.

<a href="http://lh3.ggpht.com/-4LAQs38n8hk/Ur7sbGRY33I/AAAAAAAAAfk/5TyORBA7MHM/s1600-h/image%25255B28%25255D.png"><img alt="image" border="0" height="212" src="http://lh3.ggpht.com/-rg2E3BHPzcE/Ur7sbv8WkeI/AAAAAAAAAfs/6MRDoW9iMn4/image_thumb%25255B16%25255D.png?imgmax=800"   title="image" width="244" /></a>

####1. Name your app
After selecting the template, the first thing you need to do is to supply some information about your app, like the logo (160x160px only), title, description and target language. <br /><br /><a href="http://lh4.ggpht.com/-gBvm8umzj4g/Ur-yAufWrMI/AAAAAAAAAf8/jfV-dsHWdI0/s1600-h/image%25255B32%25255D.png"><img alt="image" border="0" height="344" src="http://lh5.ggpht.com/-njZ_wFTEMp4/Ur-yBRaEJ1I/AAAAAAAAAgE/Yn_aXYbZfKA/image_thumb%25255B18%25255D.png?imgmax=800"   title="image" width="644" /></a>

####2. Add some content
Content are arranged into sections and each section contains a data source and section pages. We can also add a menu page as a section in which we can add menu items to.<br /><br /><a href="http://lh4.ggpht.com/-6GsGf2ELjBU/Ur-yCCSkpsI/AAAAAAAAAgM/O3s7jLw_Zjc/s1600-h/image%25255B36%25255D.png"><img alt="image" border="0" height="344" src="http://lh4.ggpht.com/-ZArExtUGiHY/Ur-yCzU5unI/AAAAAAAAAgU/FszK8G12ywQ/image_thumb%25255B20%25255D.png?imgmax=800"   title="image" width="644" /></a>

So let’s create a new section. Click the ‘<strong>+</strong>’ icon located in the upper-right corner and select ‘<em>Add section</em>’

<a href="http://lh5.ggpht.com/-JcepFruQWUg/Ur-yD8XOqwI/AAAAAAAAAgc/moq2BYpOb60/s1600-h/image%25255B41%25255D.png"><img alt="image" border="0" height="150" src="http://lh4.ggpht.com/-V47ifd0f0Cs/Ur-yERlfeRI/AAAAAAAAAgk/OEjpJWfaE3s/image_thumb%25255B23%25255D.png?imgmax=800"   title="image" width="162" /></a>

After naming our section to something that makes sense, we have a choice between selecting from an existing data source, or creating a new one from scratch. Let’s be daring and create a new one. <br />As you can see on the screenshot below, we have six types of data sources we can pick from: Collection, RSS, YouTube, Flickr, Bing and HTML.

<a href="http://lh3.ggpht.com/-Y_vdDsAZA-Y/Ur-yE8RU3NI/AAAAAAAAAgs/0LOnoGpa348/s1600-h/image%25255B48%25255D.png"><img alt="image" border="0" height="342" src="http://lh5.ggpht.com/-EkDkjd9lvzI/Ur-yFvFj9YI/AAAAAAAAAg0/zHm7_F1GGDM/image_thumb%25255B26%25255D.png?imgmax=800"   title="image" width="644" /></a>

- The <em>collection type</em> allows us to add static or dynamic collection content as a data source. The difference between the two is that the former will be downloaded with the app, where as the latter will be downloaded on demand from a special data service. 

> Use a dynamic collection if your data changes often and you don’t want to update the whole app just to get new data.

- The <em>RSS type</em> is kind of self explanatory. Selecting this will prompt us for a RSS feed URI which we can use as the source for our data.  
- The <em>YouTube, Flickr and Bing types</em> all work a similar way by specifying the criteria for the data to be pulled from the particular service. 
- The <em>HTML type</em> is just static HTML content, like a web page. 

> Use the HTML type for static pages like ‘<em>About</em>’ or ‘<em>Contact</em>’ pages.

For this example, I’m going to pick <em>Flickr</em> as the type of data source, name it and click ‘<em>Save changes</em>’ that will take us to the ‘Edit section’ page:

<a href="http://lh4.ggpht.com/-MB8r3fEqMNs/Ur_E1GHs1ZI/AAAAAAAAAhM/C3AdD2muTXE/s1600-h/image%25255B52%25255D.png"><img alt="image" border="0" height="343" src="http://lh6.ggpht.com/-q1Ag326wwr8/Ur_E13NfXsI/AAAAAAAAAhU/src631fHLsc/image_thumb%25255B28%25255D.png?imgmax=800"   title="image" width="644" /></a>

Here we can customize the pages we want for this section. For simplicity I’m going to stick with the defaults, however, we do need to do one last thing; and that is specify the search criteria on Flickr. 

We do this by simply clicking on our newly created data source (in my case it’s <em>FlickrDataSource</em>) and specify some search criteria (in my case it’s “Men’s shoes”). Once we’re done with all our changes, click ‘<em>Save changes</em>’ that will take us back to the sections page.

<a href="http://lh3.ggpht.com/-J0OfKpnYj-g/Ur_E2hLxlAI/AAAAAAAAAhc/DBxAUjB1Gaw/s1600-h/image%25255B56%25255D.png"><img alt="image" border="0" height="322" src="http://lh3.ggpht.com/-sLqFC3_nZzY/Ur_E3ar-7SI/AAAAAAAAAhk/mwwagrT90D8/image_thumb%25255B30%25255D.png?imgmax=800"   title="image" width="644" /></a><

####3. Configure app style
In the next step we can customize the style, tiles, splash and lock screen. From within the ‘<em>Style</em>’ tab is where we want to let the artist loose from within us. Here we can pick from a broad range of colours for the accent brush, background (can also be an image), foreground brush and application bar brush.

<a href="http://lh3.ggpht.com/-Z6eZNOOk8sQ/Ur_E4COLCmI/AAAAAAAAAhs/Ado32a23cwQ/s1600-h/image%25255B60%25255D.png"><img alt="image" border="0" height="343" src="http://lh3.ggpht.com/-nU0fTMoZN-4/Ur_E4yiIFFI/AAAAAAAAAh0/iCpBOm9RcEg/image_thumb%25255B32%25255D.png?imgmax=800"   title="image" width="644" /></a>

From the ‘<em>Tiles</em>’ tab we can customize how the app’s tile must behave if its pinned to the start screen. We have a choice between cycle, flip and iconic.

<a href="http://lh3.ggpht.com/-DjygQChyK4M/Ur_E584cmHI/AAAAAAAAAh8/V7wkM5DX0h8/s1600-h/image%25255B64%25255D.png"><img alt="image" border="0" height="343" src="http://lh3.ggpht.com/-qadD0xjMZ1I/Ur_E6psJmTI/AAAAAAAAAiE/FGB_xqFLcKo/image_thumb%25255B34%25255D.png?imgmax=800"   title="image" width="644" /></a>

We can customize the splash and lock screens in the&nbsp; ‘<em>Splash &amp; lock</em>’ tab:

<a href="http://lh3.ggpht.com/-M7YGo77uIRI/Ur_E7W_yfMI/AAAAAAAAAiM/-GhwmE3K4FU/s1600-h/image%25255B72%25255D.png"><img alt="image" border="0" height="343" src="http://lh5.ggpht.com/-kJJu6azHook/Ur_E8W_8WgI/AAAAAAAAAiU/Hfiyo6hx-74/image_thumb%25255B38%25255D.png?imgmax=800"   title="image" width="644" /></a>

####4. Generate the app
Finally, once we’ve completed all our modifications and tweaks, we are ready to do some serious app generation and we click ‘<em>Generate</em>’ from the main options on the far-right.

<a href="http://lh5.ggpht.com/-QmnSfXzmHOs/Ur_E8-tyc_I/AAAAAAAAAic/9Ow7NSts2VU/s1600-h/image%25255B76%25255D.png"><img alt="image" border="0" height="343" src="http://lh3.ggpht.com/-3gii4IIZOoQ/Ur_E9jh1LGI/AAAAAAAAAik/bIshy-wwEfA/image_thumb%25255B40%25255D.png?imgmax=800"   title="image" width="644" /></a>

<a href="http://lh4.ggpht.com/-5f22Sephkas/Ur_E-IUAZmI/AAAAAAAAAis/5RihwQY7F6k/s1600-h/image%25255B80%25255D.png"><img align="left" alt="image" border="0" height="47" src="http://lh3.ggpht.com/-UK1FlDm_cSs/Ur_E-1mQt1I/AAAAAAAAAi0/lWpZI-1BODI/image_thumb%25255B42%25255D.png?imgmax=800" title="image" width="47" /></a>

To start with the app generation, we simply click the big blue ‘<em>Generate app</em>’ button and confirm at the following popup. This will start the generation process and may take some time, depending on the complexity of your app. At this stage I suggest getting a quick cup of coffee to witness the next part of our act.

<a href="http://lh3.ggpht.com/-GjoxVilU0Sc/Ur_E_TdGmgI/AAAAAAAAAi8/2UyvOoaH46E/s1600-h/image%25255B84%25255D.png"><img alt="image" border="0" height="343" src="http://lh6.ggpht.com/-KOzl2BrtGl8/Ur_FANRui7I/AAAAAAAAAjE/BFw7mfgTCa0/image_thumb%25255B44%25255D.png?imgmax=800" title="image" width="644" /></a>

<a href="http://lh6.ggpht.com/-3kNgDBF1ASo/Ur_FAzT_43I/AAAAAAAAAjM/Vi2pQsofTJY/s1600-h/image%25255B88%25255D.png"><img alt="image" border="0" height="344" src="http://lh6.ggpht.com/-i89IeSlMh7c/Ur_FBrEAvAI/AAAAAAAAAjU/6q7jUOCjJdA/image_thumb%25255B46%25255D.png?imgmax=800"   title="image" width="644" /></a>

After a while (or maybe 2 sips of your coffee) the app is finally done and ready for testing. We are able to test the app straight away from your Windows Phone by scanning the <a href="http://en.wikipedia.org/wiki/QR_code" target="_blank">QR code</a> or downloading the publish package onto your device.

> Remember to install the security certificate provided to you via e-mail. If you haven’t received it, simply click the ‘<em>install certificate</em>’ link and scan the QR code from your phone.

####5. Publish your app
If you are ready to publish your app to the Windows Phone App Store you can go to the <a href="https://dev.windowsphone.com/" target="_blank">Windows Phone Dev Center</a> and <a href="https://dev.windowsphone.com/en-us/AppSubmission/Hub?logged_in=1" target="_blank">submit your app</a>.

Once you’re there, you must complete following:

- App info – Give your app an alias (to be used on the store), a category and price.  
- Upload the XAP (we just downloaded from App Studio)

<a href="http://lh3.ggpht.com/-ul1L79p3Hfw/Ur_FCKFTqtI/AAAAAAAAAjc/_ToRThL56k0/s1600-h/image%25255B93%25255D.png"><img alt="image" border="0" height="176" src="http://lh5.ggpht.com/-5MC-mHStttg/Ur_FCwF2PVI/AAAAAAAAAjk/v4g8t8m0h0w/image_thumb%25255B49%25255D.png?imgmax=800"   title="image" width="483" /></a>

Once its finished uploading the package, the next thing to do is to specify a version number, add a description, specify some search keywords and upload the marketing images which are:

- The app title icon (300x300px) 
- Promotional background image (1000x800px) to be used in the store  
- At least one screenshot – <a href="http://www.windowsphone.com/en-za/how-to/wp8/photos/take-a-screenshot" target="_blank">see here how to take a screenshot right from your Windows Phone here</a><

> You can also configure optional additional items such as in-app advertising, market selection & custom pricing and map services

Lastly we can click the ‘<em>Review and submit</em>’ button where we can review a summary of the app to be published. This is a good place to make sure everything is in order for the big submission. Once we’re happy with everything, we simply click ‘<em>Submit</em>’ and we’re good to go. 

This will initiate the submission process where Microsoft will test and review your app for quality assurance. This usually takes about 5 business days to make sure everything meets the standard store requirements.

<a href="http://lh6.ggpht.com/-c2fewawUdX0/Ur_FDwajXyI/AAAAAAAAAjs/7xx3J8Lw5dk/s1600-h/image%25255B99%25255D.png"><img alt="image" border="0" height="230" src="http://lh3.ggpht.com/-hv0LQ5GpYrs/Ur_FERsGIoI/AAAAAAAAAj0/4fCHnBgY8CU/image_thumb%25255B53%25255D.png?imgmax=800"  title="image" width="465" /></a>

<img height="180" src="http://s3.amazonaws.com/rapgenius/filepicker%2FxkHmUmr3RXGg0HTwxkWA_Like_A_Bosss.jpg" />

*That’s all folks!* Hope this will help someone to get their brilliant idea from their head to the store in a flash.